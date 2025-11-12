# Kixx Agent Reference

Quick reference for AI agents working with Kixx. Focuses on conventions that aren't obvious from exploring code.

## Critical Gotchas

### 1. Non-Root Pages MUST Use Subdirectory Structure

```
❌ pages/about.jsonc + pages/about.md        (metadata works, content doesn't render!)
✅ pages/about/page.jsonc + pages/about/page.md
```

**Why**: Kixx requires subdirectory structure for all non-root pages. Flat files will load metadata (title, description) but markdown content won't render - you'll get an empty `<main>` tag.

**Exception**: Only the root page (`/`) uses flat structure: `pages/page.jsonc` + `pages/page.md`

### 2. Homepage File Naming

```
❌ pages/index.jsonc + pages/index.md
✅ pages/page.jsonc + pages/page.md
```

**Why**: Kixx strips "index" from all URL paths. The root path `/` becomes empty, mapping to `page.jsonc`, not `index.jsonc`.

### 3. Template Property Name

```jsonc
❌ { "template": "custom.html" }
✅ { "baseTemplateId": "custom.html" }
```

**Why**: The property is called `baseTemplateId` in Kixx's page handler, not `template`.

### 4. Template Extension Required

```jsonc
❌ { "baseTemplateId": "custom" }
✅ { "baseTemplateId": "custom.html" }
```

**Why**: `getBaseTemplate()` expects the full filename including extension.

## Path Mapping Rules

| URL Path | REQUIRED File Structure | Notes |
|----------|------------------------|-------|
| `/` | `pages/page.jsonc` + `pages/page.md` | Root is the ONLY page using flat structure |
| `/about` | `pages/about/page.jsonc` + `pages/about/page.md` | MUST use subdirectory |
| `/about/index` | `pages/about/page.jsonc` + `pages/about/page.md` | "index" stripped → same as `/about` |
| `/blog/post` | `pages/blog/post/page.jsonc` + `pages/blog/post/page.md` | MUST use nested subdirectories |

**Algorithm**:
1. Take URL pathname
2. Remove file extensions
3. Remove "index" from path segments
4. For root (`/`): use `pages/page.jsonc`
5. For all others: use `pages/{path}/page.jsonc` (subdirectory structure)

## File Structure

```
pages/
├── page.jsonc          # Homepage metadata (root page uses flat structure)
├── page.md             # Homepage content
├── about/              # /about page (MUST use subdirectory)
│   ├── page.jsonc      # /about metadata
│   └── page.md         # /about content
└── blog/
    ├── page.jsonc      # /blog homepage
    ├── page.md         # /blog content
    └── post1/          # /blog/post1 page (nested subdirectory)
        ├── page.jsonc  # /blog/post1 metadata
        └── page.md     # /blog/post1 content
```

## Page Metadata Schema

**File**: `pages/page.jsonc`

```jsonc
{
    // Required (processed as Handlebars template)
    "title": "Page Title",

    // Optional but recommended
    "description": "Page description",

    // Template to use (defaults to "base.html")
    "baseTemplateId": "custom.html",

    // Optional SEO
    "canonicalURL": "https://example.com/page",

    // Optional Open Graph
    "openGraph": {
        "type": "website",
        "title": "OG Title",
        "description": "OG Description",
        "image": "https://example.com/og-image.jpg"
    },

    // Optional content type override
    "contentType": "text/html"
}
```

## Template Context Variables

Available in templates via `{{ variable }}`:

```handlebars
{{ title }}          - Page title
{{ description }}    - Page description
{{ canonicalURL }}   - Canonical URL (auto-generated if not provided)
{{ body }}           - Page content (from .md or .html file)

{{#if openGraph }}
    {{ openGraph.type }}
    {{ openGraph.title }}
    {{ openGraph.description }}
    {{ openGraph.image }}
{{/if}}
```

## Template Syntax

```handlebars
<!-- Variables -->
{{ variable }}

<!-- Conditionals -->
{{#if condition }}
    <p>Condition is true</p>
{{/if}}

<!-- Partials -->
{{> partial-name.html }}

<!-- Special: Render page body -->
{{noop body }}
```

## Common Errors

### "Cannot read properties of undefined (reading 'split')"

**Cause**: Page file not found (usually wrong filename)

**Check**:
1. Is it `page.jsonc` (not `index.jsonc`)?
2. Is the file in the right directory?
3. Does the path match the URL mapping rules?

### Template Not Loading

**Check**:
1. Property name is `baseTemplateId` (not `template`)
2. Extension `.html` is included
3. Template file exists in `templates/templates/`

### Port Configuration Ignored (v5.2.0 Bug)

**Issue**: Development port config ignored, always uses 3001

**Workaround**: Just use port 3001 for development

## Known Bugs

### Kixx 6.0.0
- **CRITICAL**: Completely broken, unusable
- Missing `.gitignore` in project template
- `app.Root` user type not registered
- Recommend: Use 5.2.0 instead

### Kixx 5.2.0
- Port configuration ignored in development (always uses 3001)
- No guard against undefined in `hydrateMetadataTemplate()`

## Agent Development Tips

### 1. Check Files First
Before assuming an error is a bug, verify:
- File exists at expected path
- Filename matches conventions (especially `page.jsonc` vs `index.jsonc`)
- Property names are exact (case-sensitive)

### 2. Read Source Code
When documentation is unclear:
- Check `lib/view-service/view-service.js` for page loading
- Check `lib/request-handlers/handlers/page-handler.js` for handler logic
- Look for JSDoc comments explaining conventions

### 3. Server Restart Required
File changes may not be picked up automatically. Restart server:
```bash
pkill -f "kixx app-server"
npx kixx app-server --environment development
```

### 4. Debug Logging
Enable debug logs in `kixx-config.jsonc`:
```jsonc
{
    "environments": {
        "development": {
            "logger": {
                "level": "debug",
                "mode": "console"
            }
        }
    }
}
```

## Quick Validation Checklist

When creating a new page:
- [ ] Using `page.jsonc` (not `index.jsonc`) for homepage
- [ ] Property is `baseTemplateId` (not `template`)
- [ ] Template includes `.html` extension
- [ ] Metadata file exists: `pages/{path}.jsonc`
- [ ] Content file exists: `pages/{path}.md` or `pages/{path}.html`
- [ ] Template file exists: `templates/templates/{template}.html`
- [ ] Title and description are strings (not objects)

## Version Compatibility

| Version | Status | Notes |
|---------|--------|-------|
| 6.0.0 | ❌ Broken | Do not use - critical bugs |
| 5.2.0 | ⚠️ Usable | Minor port config bug, otherwise works |
| 5.1.x | ❓ Unknown | Not tested |

## Summary for Agents

**The #1 Thing to Remember**: Kixx uses `page.jsonc` for the homepage, NOT `index.jsonc`. This causes 90% of initial errors.

**The #2 Thing**: Always use `baseTemplateId` property (not `template`) with `.html` extension.

**The #3 Thing**: When you see low-level tokenizer errors, check file naming first before investigating deeper.
