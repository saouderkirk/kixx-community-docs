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

### Built-in Helpers

- [`{{#if}}` - Conditional rendering](./helper-if.md) **TODO**
- [`{{#if-equal}}` - Equality comparison](./helper-if-equal.md) **TODO**
- [`{{#if-empty}}` - Empty check](./helper-if-empty.md) **TODO**
- [`{{#each}}` - Iteration](./helper-each.md) **TODO**
- [`{{noop}}` - No-operation placeholder](./helper-noop.md) **TODO**
- [`{{> partial}}` - Include partials](./helper-partial.md) **TODO**

### View Helpers

- [`format_date` - Date formatting](./helper-format-date.md) **TODO**
- [`plus_one` - Increment values](./helper-plus-one.md) **TODO**

### Custom Helpers
**TODO**: Document how to create custom template helpers.

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
