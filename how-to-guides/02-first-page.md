# Your First Page

Learn how to create a simple page in Kixx with the correct file structure.

## Prerequisites

- Kixx project initialized with `npx kixx init-project`
- Server running with `npx kixx app-server --environment development`

## Understanding Page Files

Each page in Kixx consists of two files:
1. **Metadata file** (`.jsonc`) - Contains title, description, template info
2. **Content file** (`.md` or `.html`) - Contains the actual page content

## Critical Naming Convention

**IMPORTANT**: Kixx strips "index" from URL paths when mapping to files.

| URL | Correct Files | ❌ Wrong Files |
|-----|--------------|---------------|
| `/` | `pages/page.jsonc` + `pages/page.md` | ~~`pages/index.jsonc`~~ |
| `/about` | `pages/about.jsonc` + `pages/about.md` | ~~`pages/about/index.jsonc`~~ |

## Creating the Homepage

### Step 1: Create Page Metadata

Create `pages/page.jsonc` (not `index.jsonc`!):

```jsonc
{
    "title": "My Homepage",
    "description": "Welcome to my Kixx website",
    "baseTemplateId": "base.html"
}
```

**Key Points**:
- Use `baseTemplateId` (not `template`)
- Include `.html` extension
- `base.html` is the default template

### Step 2: Create Page Content

Create `pages/page.md`:

```markdown
# Welcome to My Site

This is my first Kixx page using Markdown.

## Features

- Easy to write
- Automatically converted to HTML
- Simple and clean
```

### Step 3: View Your Page

1. Make sure server is running: `npx kixx app-server --environment development`
2. Open browser to: `http://localhost:3001/`
3. You should see your homepage!

## Creating an About Page

### Step 1: Create About Metadata

Create `pages/about.jsonc`:

```jsonc
{
    "title": "About Us",
    "description": "Learn more about us",
    "baseTemplateId": "base.html"
}
```

### Step 2: Create About Content

Create `pages/about.md`:

```markdown
# About Us

We're building awesome websites with Kixx!

## Our Mission

Making web development simple and enjoyable.
```

### Step 3: View Your Page

Open browser to: `http://localhost:3001/about`

## Using Custom Templates

### Step 1: Create Custom Template

Create `templates/templates/custom.html`:

```html
<!doctype html>
<html lang="en-US">
    <head>
        <meta charset="utf-8">
        <title>{{ title }}</title>
        {{#if description }}
            <meta name="description" content="{{ description }}">
        {{/if}}
        <meta name="viewport" content="width=device-width, initial-scale=1">

        <style>
            body {
                font-family: Arial, sans-serif;
                max-width: 800px;
                margin: 0 auto;
                padding: 2rem;
            }
        </style>
    </head>
    <body>
        <main>
            {{noop body }}
        </main>
    </body>
</html>
```

### Step 2: Use Custom Template

Update `pages/page.jsonc`:

```jsonc
{
    "title": "My Homepage",
    "description": "Welcome to my Kixx website",
    "baseTemplateId": "custom.html"
}
```

**Remember**: Always include the `.html` extension!

## Common Mistakes

### ❌ Wrong: Using index.jsonc
```
pages/
├── index.jsonc    # Won't work!
└── index.md       # Won't work!
```

### ✅ Correct: Using page.jsonc
```
pages/
├── page.jsonc     # Works!
└── page.md        # Works!
```

### ❌ Wrong: Wrong property name
```jsonc
{
    "template": "custom"  // Wrong property
}
```

### ✅ Correct: Use baseTemplateId with extension
```jsonc
{
    "baseTemplateId": "custom.html"  // Correct!
}
```

## Troubleshooting

### Error: "Cannot read properties of undefined (reading 'split')"

**Cause**: Page file not found

**Solution**: Check that you're using `page.jsonc` (not `index.jsonc`) for the homepage

### Template Not Loading

**Check**:
1. Is `baseTemplateId` spelled correctly? (not `template`)
2. Did you include the `.html` extension?
3. Does the template file exist in `templates/templates/`?

### Changes Not Appearing

**Solution**: Restart the Kixx server to pick up file changes:
```bash
# Kill existing server
pkill -f "kixx app-server"

# Start fresh
npx kixx app-server --environment development
```

## Next Steps

- [Understanding Routes](./03-routes.md)
- [Working with Templates](./04-templates.md)
- [Page Metadata & SEO](./pages-metadata.md)

## Summary

**Key Takeaways**:
1. Homepage uses `pages/page.jsonc` (NOT `index.jsonc`)
2. Use `baseTemplateId` property with `.html` extension
3. Separate metadata (`.jsonc`) and content (`.md`) files
4. Kixx automatically strips "index" from URL paths
