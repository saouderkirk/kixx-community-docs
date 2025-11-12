# Conventions Reference

All the conventions Kixx uses to reduce configuration and code.

## Directory Structure

### Standard Project Layout

```
my-kixx-app/
├── kixx-config.jsonc       # Application configuration
├── virtual-hosts.jsonc     # Virtual host routing
├── package.json           # Node.js dependencies
├── pages/                 # Page content and metadata
├── routes/                # Route definitions
└── templates/             # HTML templates
    ├── templates/         # Main templates
    └── partials/          # Reusable template parts
```

**TODO**: Document optional directories and their purposes.

---

## File Naming Conventions

### Pages Directory

**CRITICAL - Path Mapping Rules**:

Kixx strips "index" from URL paths when mapping to files. This is the most important convention to understand:

| URL Path | REQUIRED Structure | Will NOT Work |
|----------|-------------------|---------------|
| `/` (root) | `pages/page.jsonc` + `pages/page.md` | ~~`pages/index.jsonc`~~ |
| `/about` | `pages/about/page.jsonc` + `pages/about/page.md` | ~~`pages/about.jsonc`~~ ~~`pages/about.md`~~ |
| `/blog/post` | `pages/blog/post/page.jsonc` + `pages/blog/post/page.md` | ~~`pages/blog/post.jsonc`~~ |

**IMPORTANT**: Non-root pages MUST use subdirectory structure. Flat files like `pages/about.jsonc` will not render content!

**File Types**:
- `page.jsonc` - Page metadata (title, description, baseTemplateId)
- `page.md` - Markdown content (compiled to HTML)
- `page.html` - Custom HTML content

**Key Rules**:
1. **Root page (`/`)**: Use `pages/page.jsonc` + `pages/page.md` (flat structure)
2. **All other pages**: MUST use subdirectory structure: `pages/{path}/page.jsonc` + `pages/{path}/page.md`
3. **Never use `index.jsonc`** - always use `page.jsonc`
4. Metadata and content files must have the same base name (`page`)
5. Path `/about/index` and `/about` both map to `pages/about/page.jsonc` (index is stripped)

**Common Mistake**:
```
❌ WRONG - Content won't render:
pages/
├── about.jsonc
└── about.md

✅ CORRECT - Content renders properly:
pages/
└── about/
    ├── page.jsonc
    └── page.md
```

### Routes Directory

**What we know**:
- `*.jsonc` files define routes
- Routes reference pages by name

**TODO**: Document:
- Naming conventions for route files
- How multiple route files are loaded
- Load order if it matters

### Templates Directory

**What we know**:
- `templates/` contains main templates like `base.html`
- `partials/` contains reusable components

**TODO**: Document:
- Naming conventions
- How templates are discovered
- Default template names

---

## Configuration Conventions

### kixx-config.jsonc

**What we know**:
```jsonc
{
    "name": "App Name",
    "processName": "appname",
    "environments": {
        "development": { /* config */ },
        "production": { /* config */ }
    }
}
```

**TODO**: Document:
- All available configuration keys
- Default values for each
- Which settings can be overridden

### virtual-hosts.jsonc

**What we know**:
- Hostnames are written backwards: `"com.example.subdomain"`
- Routes use protocols: `"app://file.jsonc"` or `"kixx://defaults.json"`

**TODO**: Document:
- Hostname matching rules
- Available route protocols
- Route resolution order

---

## Routing Conventions

### Route Definitions

**What we know**:
```jsonc
{
    "methods": ["GET"],
    "path": "/about",
    "handler": "PageHandler",
    "page": "about"
}
```

**TODO**: Document:
- All available handler types
- All route configuration options
- Path parameter syntax
- HTTP method conventions

---

## Template Conventions

### Template Syntax

**What we know**:
- Handlebars-like syntax
- `{{> partial.html}}` for partials
- `{{#if condition}}` for conditionals
- `{{ variable }}` for interpolation
- `{{noop body}}` for special placeholders

**TODO**: Document:
- All available syntax
- Context variables available in templates
- Helper function conventions

### Special Variables

**What we know from templates**:
- `title` - Page title
- `description` - Page description
- `openGraph` - Open Graph metadata
- `canonicalURL` - Canonical URL
- `body` - Page content

**TODO**: Document all available context variables.

---

## Page Metadata Conventions

### Standard Metadata Fields

**Required Structure** (`pages/page.jsonc`):
```jsonc
{
    "title": "Page Title",
    "description": "Page description",
    "baseTemplateId": "base.html"
}
```

**CRITICAL**: Use `baseTemplateId` (not `template`!) and include the `.html` extension.

**Known Fields**:
- `title` - Page title (processed as template, can include {{variables}})
- `description` - Meta description (processed as template)
- `baseTemplateId` - Template file name with extension (e.g., `"custom.html"`)
- `canonicalURL` - Canonical URL (auto-generated if not provided)
- `contentType` - Override content type (defaults to `text/html`)
- `openGraph` - Open Graph metadata object:
  - `type` - OG type (defaults to `"website"`)
  - `title` - OG title (defaults to page title)
  - `description` - OG description
  - `image` - OG image URL

**Default Behavior**:
- If no `baseTemplateId` specified, uses `base.html`
- Title and description support Handlebars syntax: `"title": "Welcome, {{userName}}"`
- Open Graph metadata auto-populated from page metadata

**TODO**: Document other available metadata fields beyond what we've discovered.

---

## Environment Conventions

### Environment Names

**What we know**:
- `development` - Dev environment
- `production` - Production environment

**TODO**: Document:
- Can you create custom environments?
- How to select environment at runtime
- Environment-specific conventions

---

## Default Behaviors

### Default Routes
**TODO**: Document what `"kixx://defaults.json"` includes.

### Default Templates
**TODO**: Document if there are default templates provided.

### Default Error Pages
**TODO**: Document how error pages work by default.

---

## Overriding Conventions

**TODO**: Document how to override any convention when needed.
