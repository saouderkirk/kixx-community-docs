# Troubleshooting & Error Reference

Common errors, debugging tips, and solutions.

## Common Errors

### Server Won't Start

**TODO**: Document common startup errors:
- Port already in use
- Configuration errors
- Missing dependencies
- File permission issues

### Routing Issues

**TODO**: Document:
- Route not matching
- 404 errors
- Route conflicts
- Virtual host not matching

### Template Errors

**TODO**: Document:
- Template not found
- Syntax errors in templates
- Missing partials
- Variable undefined errors
- Helper function errors

### Page Rendering Issues

#### Error: "Cannot read properties of undefined (reading 'split')"

**Full Error**:
```
TypeError: Cannot read properties of undefined (reading 'split')
    at tokenize (tokenize.js:2:24)
    at PageTemplateEngine.compileTemplate
    at PageTemplateEngine.createMetadataTemplate
    at ViewService.hydrateMetadataTemplate
```

**Cause**: Page metadata file not found. This low-level error occurs when Kixx tries to process page metadata but the page file doesn't exist.

**Common Reasons**:
1. **Wrong filename**: Using `index.jsonc` instead of `page.jsonc`
2. **Wrong path**: File is in wrong directory
3. **Missing file**: Page file wasn't created

**Solution**:
```bash
# For homepage (URL: /)
# ✓ CORRECT: pages/page.jsonc
# ✗ WRONG:   pages/index.jsonc

# For /about page
# ✓ CORRECT: pages/about.jsonc OR pages/about/page.jsonc
# ✗ WRONG:   pages/about/index.jsonc
```

**Why This Happens**: Kixx strips "index" from URL paths when mapping to files. The root path `/` becomes an empty path, which maps to `page.jsonc`, not `index.jsonc`.

#### Page Not Found (404)

**TODO**: Document other 404 scenarios.

#### Markdown Rendering Errors

**TODO**: Document markdown issues.

#### Content Not Displaying

**Symptom**: Page loads with header/footer but main content area is empty. The `<main>` tag contains no content.

**Full Error**: Page returns 200 status but rendered HTML shows:
```html
<main>

</main>
```

**Cause**: Incorrect file structure for non-root pages. Using flat files (`pages/about.jsonc`) instead of subdirectory structure (`pages/about/page.jsonc`).

**Solution**:
Move page files into subdirectory structure:

```bash
# Wrong structure (content won't render):
pages/
├── about.jsonc
└── about.md

# Correct structure:
mkdir -p pages/about
mv pages/about.jsonc pages/about/page.jsonc
mv pages/about.md pages/about/page.md
```

**Why This Happens**:
- Root page (`/`) uses flat structure: `pages/page.jsonc` + `pages/page.md`
- All other pages MUST use subdirectory structure: `pages/{path}/page.jsonc` + `pages/{path}/page.md`
- Kixx will accept flat files like `pages/about.jsonc` but won't render the markdown content
- The metadata (title, description) will still work, but `{{noop body}}` will be empty

**Verification**:
After fixing, restart server and test:
```bash
curl -s http://localhost:3001/about | grep -A 5 "<main>"
```

Should show content inside `<main>` tags, not empty.

---

## Error Classes Reference

### BadRequestError (400)
**TODO**: Document when thrown and how to handle.

### UnauthorizedError (401)
**TODO**: Document when thrown and how to handle.

### ForbiddenError (403)
**TODO**: Document when thrown and how to handle.

### NotFoundError (404)
**TODO**: Document when thrown and how to handle.

### MethodNotAllowedError (405)
**TODO**: Document when thrown and how to handle.

### ConflictError (409)
**TODO**: Document when thrown and how to handle.

### ValidationError (422)
**TODO**: Document when thrown and how to handle.

### UnsupportedMediaTypeError (415)
**TODO**: Document when thrown and how to handle.

### NotImplementedError (501)
**TODO**: Document when thrown and how to handle.

---

## Debugging Tips

### Enabling Debug Logging

**What we know**:
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

**TODO**: Document:
- All log levels
- What each level shows
- How to filter logs
- Where logs are written

### Inspecting Request/Response

**TODO**: Document:
- How to log requests
- How to inspect response data
- Debugging middleware
- Debugging handlers

### Template Debugging

**TODO**: Document:
- How to see template context
- How to debug template rendering
- Common template issues

---

## Performance Issues

### Slow Page Loads
**TODO**: Document common causes and solutions.

### Memory Issues
**TODO**: Document memory leak detection and prevention.

### File System Performance
**TODO**: Document optimization strategies.

---

## Configuration Issues

### Environment Not Loading
**TODO**: Document how to verify environment configuration.

### Virtual Host Not Matching
**TODO**: Document hostname matching rules and debugging.

### Routes Not Loading
**TODO**: Document route loading order and troubleshooting.

---

## Development Workflow Issues

### Changes Not Appearing
**TODO**: Document:
- When server restart is needed
- Template caching
- File watching behavior

### Hot Reload
**TODO**: Document if/how hot reload works.

---

## Common Mistakes

### File Naming

#### Using `index.jsonc` instead of `page.jsonc`
**Wrong**:
```
pages/
├── index.jsonc     ✗ Won't work for homepage
└── index.md        ✗ Won't work for homepage
```

**Correct**:
```
pages/
├── page.jsonc      ✓ Correct for homepage
└── page.md         ✓ Correct for homepage
```

**Why**: Kixx strips "index" from paths, so `/` maps to `page.jsonc`, not `index.jsonc`.

#### Using wrong property for template

**Wrong**:
```jsonc
{
    "template": "custom"           ✗ Wrong property name
    "baseTemplateId": "custom"     ✗ Missing extension
}
```

**Correct**:
```jsonc
{
    "baseTemplateId": "custom.html"  ✓ Correct property and extension
}
```

### Configuration Syntax
**TODO**: Document common JSONC syntax errors.

### Template Syntax
**TODO**: Document common template syntax errors.

### Route Configuration
**TODO**: Document common route configuration mistakes.

---

## Getting Help

### Checking Kixx Version
```bash
npm list kixx
```

### Checking Node Version
```bash
node --version
```

### Reporting Issues
- GitHub Issues: https://github.com/kixx-platform/kixx/issues
- Include Kixx version, Node version, and minimal reproduction

---

## Frequently Asked Questions

**TODO**: Add FAQ as questions come up:
- How do I...?
- Why doesn't...?
- Can I...?
