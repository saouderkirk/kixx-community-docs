# Architecture & Concepts

Understanding how Kixx works under the hood.

## Core Concepts

### Primitive Components Philosophy

**The Foundation of Kixx**

Kixx is built on the idea of **primitive components** - small, focused, composable building blocks that are simple on their own but powerful when combined.

**What are Primitive Components?**

A primitive component is a discrete module that:
- Has a single, clear purpose
- Can be understood independently
- Fits entirely in your head
- Becomes powerful when composed with other primitives

**Example: Email + Job Scheduler**

Individually:
- **Emailer** - Sends emails (does nothing else)
- **Job Scheduler** - Schedules tasks (does nothing else)

Combined:
- Schedule a job → Send confirmation email
- Sophisticated email automation with minimal code

**Historical Influences**

From creator Kristoffer Walker:

**AWS** - Infrastructure as primitive components
- S3 (storage), RDS (database), Lambda (compute)
- Each service is simple; power comes from composition

**Firefox XP-COM** - Cross-platform component model
- Networking component, observer model, plugin system
- Extensible through discrete modules

**Ruby on Rails** - MVC as primitives
- Models, Views, Controllers as building blocks
- Convention-based composition

**Benefits**

1. **Minimal Learning** - Learn one component at a time
2. **Small Context** - Think about 1-3 components simultaneously
3. **AI-Friendly** - LLMs can understand components in isolation
4. **Progressive** - Start simple, add complexity incrementally
5. **Composable** - Infinite possibilities from finite primitives

See [PHILOSOPHY.md](../PHILOSOPHY.md) for more on this approach.

### Hypermedia-Driven Applications

**Official Definition:**

Hypermedia-Driven Applications are web applications where hypermedia (HTML) serves as the engine of application state. Instead of relying on JavaScript to manage state and coordinate between client and server, the application state changes by following links and submitting forms embedded in the HTML responses.

**Key Characteristics:**

- **Server-side rendering** of all application state
- **State transitions** through hypermedia (links and forms)
- **Progressive enhancement** with minimal JavaScript
- **RESTful architecture** with HTML hypertext as the representation of state
- **Monolithic design** for simplicity and productivity

The Kixx framework embodies these principles, providing a productive environment for building web apps that are simple, fast, and maintainable.

**Further Reading:**
- [Hypermedia-Driven Applications by HTMX](https://htmx.org/essays/hypermedia-driven-applications/)
- [The Web's Grain by Frank Chimero](https://frankchimero.com/blog/2015/the-webs-grain/)
- REST: From Research to Practice

**TODO**: Document practical examples and patterns specific to Kixx.

### [Convention over Configuration](./convention-over-configuration.md)
How Kixx uses conventions to reduce boilerplate.

**What we know**:
- File structure defines routing
- Standard naming conventions reduce decisions
- Sensible defaults work for 80% of cases
- Override when needed, but conventions come first

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

### [Request Handlers](./handlers.md)
How handlers process HTTP requests in Kixx.

**What we know**:
- Handler signature: `async function(context, request, response, skip)`
- Built-in handlers: `StaticFileServer`, `PageHandler`, `PageErrorHandler`
- Handlers chain together - first to call `skip()` wins
- Error handlers catch exceptions from main handlers

See [handlers.md](./handlers.md) for complete documentation.

### MCP (Model Context Protocol) Server

**AI Integration for Kixx**

The MCP server is a plugin for local AI agents (Cursor, Claude Code, etc.) that provides Kixx-specific context to LLMs.

**How It Works:**

1. **Lives Locally** - Runs on your laptop alongside your AI agent
2. **Answers Questions** - Provides documentation when LLM needs it
3. **Just-in-Time Context** - Only loads relevant component docs
4. **Enables Deterministic Output** - Smaller context = better code generation

**Example Flow:**

```
Developer: "Send an email when a new user signs up"
    ↓
LLM detects "Kixx framework" keyword
    ↓
LLM asks MCP: "How do I send email in Kixx?"
    ↓
MCP returns Emailer component documentation
    ↓
LLM asks MCP: "How do I detect new users in Kixx?"
    ↓
MCP returns database listener documentation
    ↓
LLM generates minimal code to connect components
```

**Benefits:**

- **Environmental** - Less compute = less energy consumption
- **Speed** - Smaller context processes faster
- **Quality** - Focused context produces better code
- **Local** - Can run entirely on your machine

**TODO**: Document MCP server setup and configuration.

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
