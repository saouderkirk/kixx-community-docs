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
├── public/                # Static files (CSS, JS, images, favicon)
├── routes/                # Route definitions
└── templates/             # HTML templates
    ├── templates/         # Main templates
    ├── partials/          # Reusable template parts
    └── helpers/           # Custom template helpers
```

### Public Directory (Static Files)

The `public/` directory serves static assets automatically. No configuration needed.

**How it works**:
- Files in `public/` are served at the root URL path
- `public/favicon.svg` → accessible at `/favicon.svg`
- `public/css/styles.css` → accessible at `/css/styles.css`
- `public/images/logo.png` → accessible at `/images/logo.png`

**Security features** (built into StaticFileServer):
- Blocks path traversal (`..` and `//` are rejected)
- Blocks hidden files (paths starting with `.`)
- Only allows alphanumeric characters, underscores, hyphens, and dots
- Returns 400 Bad Request for invalid paths

**Caching**:
- Default: `Cache-Control: no-cache`
- Supports `If-Modified-Since` conditional requests
- Returns `304 Not Modified` when appropriate
- Custom cache control via config (see Configuration section)

**Supported file types**: All common web assets (HTML, CSS, JS, images, fonts, SVG, etc.) with automatic Content-Type detection.

**Example structure**:
```
public/
├── favicon.ico
├── favicon.svg
├── robots.txt
├── css/
│   └── styles.css
├── js/
│   └── app.js
└── images/
    ├── logo.png
    └── hero.jpg
```

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

Route files define URL patterns, HTTP methods, and handler chains.

**File location**: `routes/main.jsonc` (referenced from `virtual-hosts.jsonc`)

**Route definition structure**:
```jsonc
[
    {
        "name": "AboutPage",           // Unique route identifier
        "pattern": "/about",           // URL pattern (uses path-to-regexp syntax)
        "targets": [
            {
                "name": "AboutTarget",
                "methods": ["GET", "HEAD"],  // Allowed HTTP methods
                "handlers": [
                    // Handler chain - executed in order
                    "StaticFileServer()",    // Try static file first
                    "PageHandler()"          // Then try page handler
                ]
            }
        ],
        "errorHandlers": [
            "PageErrorHandler()"       // Handle errors (404s, etc.)
        ]
    }
]
```

**Pattern syntax** (path-to-regexp):
- `/about` - Exact match
- `/blog/:slug` - Named parameter (accessible as `request.pathnameParams.slug`)
- `/files/*` - Wildcard match
- `*` - Match all paths (used in default routes)

**Default routes** (`kixx://defaults.json`):
```javascript
// What the default routes provide:
{
    name: 'DefaultPages',
    pattern: '*',                    // Match everything
    targets: [{
        methods: ['GET', 'HEAD'],
        handlers: [
            StaticFileServer(),      // 1. Try to serve from /public
            PageHandler(),           // 2. Try to render a page
        ]
    }],
    errorHandlers: [
        PageErrorHandler()           // Handle 404s with error page
    ]
}
```

**Handler execution order**:
1. First handler in the array runs first
2. If handler returns response unchanged, next handler runs
3. If handler serves content (calls `skip()`), remaining handlers are skipped
4. Error handlers run if any handler throws

**Most apps don't need custom routes** - the defaults handle static files and pages automatically. Only create custom routes for:
- API endpoints
- Custom handlers
- Form submissions
- Different handler chains per path

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

Maps hostnames to route configurations. Required for Kixx to serve requests.

**Basic structure**:
```jsonc
[
    {
        "name": "Main",
        "hostname": "com.example.www",     // Reversed hostname format
        "routes": [
            "app://main.jsonc",            // Your custom routes (from /routes)
            "kixx://defaults.json"         // Framework default routes
        ]
    }
]
```

**Hostname format** - hostnames are written **backwards** (most generic → most specific):
| Actual Hostname | Config Format |
|-----------------|---------------|
| `localhost` | `"localhost"` |
| `example.com` | `"com.example"` |
| `www.example.com` | `"com.example.www"` |
| `api.staging.example.com` | `"com.example.staging.api"` |

**Why reversed?** Enables hierarchical matching from generic (TLD) to specific (subdomain).

**Route protocols**:
- `app://filename.jsonc` - Load from your `/routes` directory
- `kixx://defaults.json` - Load Kixx framework defaults

**Route resolution order**:
1. Routes are loaded in array order
2. First matching route wins
3. Always put custom routes before `kixx://defaults.json`

**Multiple virtual hosts** (for different domains/subdomains):
```jsonc
[
    {
        "name": "API",
        "hostname": "com.example.api",
        "routes": ["app://api-routes.jsonc"]
    },
    {
        "name": "Main",
        "hostname": "com.example.www",
        "routes": ["app://main.jsonc", "kixx://defaults.json"]
    },
    {
        "name": "Fallback",
        "hostname": "com.example",
        "routes": ["kixx://defaults.json"]
    }
]
```

**Development tip**: For local development, use `"localhost"` as hostname:
```jsonc
{
    "name": "Development",
    "hostname": "localhost",
    "routes": ["app://main.jsonc", "kixx://defaults.json"]
}
```

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
