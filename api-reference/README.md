# API Reference

Complete API documentation for the Kixx framework.

## Table of Contents

- [Configuration Files](#configuration-files)
- [Handlers](#handlers)
- [Template Helpers](#template-helpers)
- [Context & Request Objects](#context--request-objects)
- [Response Methods](#response-methods)
- [Error Classes](#error-classes)
- [Middleware](#middleware)
- [CLI Commands](#cli-commands)

---

## Configuration Files

### [kixx-config.jsonc](./config-kixx-config.md)
Main application configuration file.

**TODO**: Document all available configuration options.

### [virtual-hosts.jsonc](./config-virtual-hosts.md)
Virtual host and routing configuration.

**TODO**: Document hostname patterns, route resolution, and configuration options.

---

## Handlers

### [PageHandler](./handler-page.md)
Renders pages using templates and markdown content.

**TODO**: Document configuration, usage, and available options.

### [StaticFileHandler](./handler-static-file.md)
Serves static files (CSS, JS, images).

**TODO**: Document configuration and usage.

### Custom Handlers
**TODO**: Document how to create custom request handlers.

---

## Template Helpers

**Official Documentation**: [Templating with Kixx](https://github.com/kixx-framework/kixx/blob/main/docs/templating-with-kixx.md)

### Built-in Block Helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `#each` | `{{#each items \|item, index\|}}...{{/each}}` | Iterate over arrays, objects, Maps, Sets |
| `#if` | `{{#if condition}}...{{/if}}` | Conditional rendering based on truthiness |
| `#ifEqual` | `{{#ifEqual value1 value2}}...{{/ifEqual}}` | Compare two values using `==` equality |

### Built-in Inline Helpers

| Helper | Usage | Description |
|--------|-------|-------------|
| `formatDate` | `{{ formatDate date timezone="..." format="..." }}` | Format dates with timezone and locale |
| `unescape` | `{{ unescape htmlContent }}` | Prevent HTML escaping (trusted content only!) |
| `plusOne` | `{{ plusOne index }}` | Add 1 to number (for 1-based display) |

### Truthiness Rules

Values considered **truthy**: non-empty strings, non-zero numbers, `true`, non-empty collections
Values considered **falsy**: `false`, `0`, empty strings, `null`, `undefined`, empty collections

### Custom Helpers

JavaScript modules in `./templates/helpers/` exporting `name` string and `helper` function.

```javascript
// templates/helpers/truncate.js
export const name = "truncate";
export function helper(text, length = 100) {
    return text.length > length ? text.slice(0, length) + "..." : text;
}
```

**Block helpers** receive `renderPrimary()` and `renderInverse()` methods for control flow.

---

## Context & Request Objects

### [Application Context](./context-application.md)
**TODO**: Document the application context object and its properties.

### [Request Object](./context-request.md)
**TODO**: Document request properties, parameters, headers, body parsing.

### [Response Object](./context-response.md)
**TODO**: Document response methods, status codes, headers.

---

## Error Classes

**TODO**: Document all error classes:
- BadRequestError
- NotFoundError
- UnauthorizedError
- ForbiddenError
- ValidationError
- etc.

See [Error Reference](./errors.md)

---

## Middleware

**TODO**: Document:
- Built-in middleware
- How to create custom middleware
- Middleware execution order

See [Middleware Reference](./middleware.md)

---

## CLI Commands

### `kixx init-project`
Initialize a new Kixx project with scaffolding.

**TODO**: Document all CLI flags and options.

### `kixx app-server`
Start the application server.

**TODO**: Document environment flags, port configuration, etc.

See [CLI Reference](./cli-commands.md)

---

## Data Layer

**TODO**: Document:
- Datastore API
- File system storage
- Custom data adapters

See [Data Layer Reference](./data-layer.md)

---

## Testing

**Official Documentation**:
- [Kixx Test Framework](https://github.com/kixx-framework/kixx/blob/main/docs/kixx-test-framework.md)
- [Assertion Library](https://github.com/kixx-framework/kixx/blob/main/docs/assertion-library.md)

### Test Framework

```javascript
import { describe, before, after, it } from "kixx-test";

describe("Feature Name", ({ before, after, it }) => {
    before(() => { /* setup */ });
    after(() => { /* cleanup */ });
    it("should do something", () => { /* assertion */ });
});
```

**Key Rules**:
- Do NOT nest describe() blocks
- Use try-catch for error testing
- Prefer `.callCount` over `.calledOnce`

### Assert Library

| Function | Description |
|----------|-------------|
| `assert(value)` | Value is truthy |
| `assertFalsy(value)` | Value is falsy |
| `assertEqual(expected, actual)` | Strict equality (`===`) |
| `assertNotEqual(expected, actual)` | Not equal |
| `assertMatches(pattern, value)` | RegExp, substring, or equality |
| `assertDefined(value)` | Not undefined |
| `assertUndefined(value)` | Is undefined |
| `assertNonEmptyString(value)` | Non-empty string |
| `assertNumberNotNaN(value)` | Number, not NaN |
| `assertArray(value)` | Is array |
| `assertBoolean(value)` | Is boolean |
| `assertFunction(value)` | Is function |
| `assertValidDate(value)` | Valid Date object |
| `assertGreaterThan(control, subject)` | subject > control |
| `assertLessThan(control, subject)` | subject < control |

**Note**: No deep equality - check individual properties explicitly.

See [Testing with Kixx Guide](../how-to-guides/testing-with-kixx.md) for full examples
