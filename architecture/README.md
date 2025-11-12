# Architecture & Concepts

Understanding how Kixx works under the hood.

## Core Concepts

### [Hypermedia-Driven Applications](./hypermedia-driven.md)
How Kixx uses HTML as the engine of application state.

**What we know**:
- Server-side rendering of all application state
- State transitions through hypermedia (links and forms)
- Progressive enhancement with minimal JavaScript
- RESTful architecture with HTML hypertext

**TODO**: Document practical examples and patterns.

### [Convention over Configuration](./convention-over-configuration.md)
How Kixx uses conventions to reduce boilerplate.

**TODO**: Document all conventions and their rationale.

---

## System Architecture

### [Request/Response Lifecycle](./request-lifecycle.md)
**TODO**: Complete flow from incoming request to rendered response:
1. Request received
2. Virtual host resolution
3. Route matching
4. Handler selection
5. Middleware execution
6. Handler execution
7. Template rendering
8. Response sent

### [Routing System](./routing-system.md)
**TODO**: How routes are resolved and matched.

### [Template Rendering Pipeline](./template-pipeline.md)
**TODO**: How templates are compiled and rendered.

### [Application Initialization](./app-initialization.md)
**TODO**: What happens when `kixx app-server` starts.

---

## Design Decisions

### [Monolithic Architecture](./monolithic-design.md)
Why Kixx favors monolithic apps over microservices.

**What we know**:
- Optimized for solo developers and small teams
- Simpler than distributed architectures
- Better velocity for small teams

**TODO**: Document trade-offs and when to use microservices instead.

### [Server-Side Rendering](./server-side-rendering.md)
Why Kixx renders on the server instead of client.

**TODO**: Document benefits, limitations, and patterns.

### [No TypeScript](./no-typescript.md)
Why Kixx uses JavaScript instead of TypeScript.

**What we know from README**: "There are reasons for that - primarily developer happiness ;-)"

**TODO**: Document the reasoning and trade-offs.

---

## Component Deep Dives

### [Virtual Hosts](./virtual-hosts.md)
**TODO**: How virtual hosting works in Kixx.

### [Plugin System](./plugin-system.md)
**TODO**: If Kixx has a plugin system, document it here.

### [Datastore](./datastore.md)
**TODO**: How the data layer works.

### [Logger](./logger.md)
**TODO**: How logging works and how to use it effectively.

---

## Performance

### [Caching Strategy](./caching.md)
**TODO**: How Kixx handles caching.

### [Asset Management](./assets.md)
**TODO**: How static assets are handled.

---

## Comparison with Other Frameworks

### [Kixx vs Next.js](./comparison-nextjs.md)
**TODO**: Key differences and when to choose each.

### [Kixx vs Express](./comparison-express.md)
**TODO**: Key differences and when to choose each.

### [Kixx vs Rails/Django](./comparison-rails-django.md)
**TODO**: Key differences and when to choose each.
