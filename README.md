# Kixx Community Documentation

> **For AI Coding Assistants**: Start with [AGENT_REFERENCE.md](./AGENT_REFERENCE.md)

Comprehensive, community-created documentation for the [Kixx web framework](https://www.kixx.dev). Created through real-world usage while building production applications.

## What is This?

Official Kixx documentation is great, but some conventions and gotchas only become clear when building real applications. This documentation fills those gaps with:

- **Critical conventions** that aren't obvious from exploring the code
- **Common errors** with detailed solutions
- **Real-world examples** from production applications
- **AI-friendly reference** for coding assistants like Claude, GitHub Copilot, etc.
- **Video tutorials** from the framework creator

## Quick Navigation

### 🤖 For AI Agents
**[Agent Reference](./AGENT_REFERENCE.md)** - Quick reference covering critical gotchas and non-obvious conventions

### 📖 For Developers

**Core Documentation:**
- **[Conventions](./conventions/README.md)** - File naming, routing, path mapping, configuration
- **[Troubleshooting](./troubleshooting/README.md)** - Common errors with detailed solutions
- **[How-To Guides](./how-to-guides/README.md)** - Step-by-step guides for common tasks
- **[Philosophy & Design](./PHILOSOPHY.md)** - Why Kixx exists and how it thinks

**Additional Resources:**
- **[API Reference](./api-reference/README.md)** - Complete API documentation (WIP)
- **[Architecture](./architecture/README.md)** - Primitive components and how Kixx works
- **[Examples](./examples/README.md)** - Real-world code examples (WIP)

### 🎥 Video Resources

Watch the **Kixxcast** series with creator Kristoffer Walker on YouTube:

**[@essayohh Channel](https://www.youtube.com/@essayohh)**

**Episode 1: Origin Story & Philosophy**
- Why Kixx was created (filling the gap between WordPress and complex frameworks)
- Server-side rendering philosophy vs. heavy JavaScript
- AI/LLM integration strategy (smaller context = better results)
- Environmental considerations in web development

**Episode 2: Technical Architecture**
- Primitive components and building blocks
- Model-View-Controller (MVC) pattern in Kixx
- MCP (Model Context Protocol) for AI integration
- Target audience: indie hackers, hobbyists, small teams (1-3 people)

## Most Common Questions

### Why won't my page content render?

**Problem**: Page loads with header/footer but main content is empty.

**Solution**: Non-root pages MUST use subdirectory structure:
```
❌ pages/about.jsonc + pages/about.md        (Won't work!)
✅ pages/about/page.jsonc + pages/about/page.md
```

See: [Conventions → File Naming](./conventions/README.md#pages-directory)

### Why does my homepage return 404?

**Problem**: Using `pages/index.jsonc` for the homepage.

**Solution**: Use `pages/page.jsonc` (not index.jsonc):
```
❌ pages/index.jsonc
✅ pages/page.jsonc
```

See: [Troubleshooting → Page Not Found](./troubleshooting/README.md)

### How do I deploy to production?

See the comprehensive [Deployment Guide](./how-to-guides/03-deploying-to-production.md) covering:
- Railway (recommended)
- Render
- VPS with nginx
- Custom domains & SSL

## About Kixx

[Kixx](https://www.kixx.dev) is a convention-over-configuration web framework for Node.js that emphasizes:

- **Minimal boilerplate** - Start building features, not configuration
- **Sensible defaults** - Works out of the box for most use cases
- **File-based routing** - Your file structure defines your routes
- **Server-side rendering** - Fast, lightweight pages without heavy JavaScript
- **Primitive components** - Small, composable building blocks
- **AI-optimized** - Designed to work exceptionally well with LLM code generation

### Why Kixx Exists

Kixx was created to solve a specific problem: **the gap between simple static sites and complex enterprise frameworks**.

- **Too simple?** WordPress is overkill for a 5-page brochure site
- **Too complex?** Custom solutions don't fit into WordPress's content model
- **Just right:** Kixx lets you build rapidly for real-world projects

From the creator:
> "It would be really great if we could build that nonprofit website, that hockey team scheduler, that avalanche conditions tracker — and do it on a weekend, realistically."

## Who Is Kixx For?

**Perfect for:**
- 🛠️ **Indie hackers** building multiple projects
- 👥 **Small teams** (1-3 people) shipping quickly
- 🎨 **Hobbyists** creating personal projects
- 💼 **Small businesses** needing custom solutions
- 🚀 **Developers** who want to be productive fast

**Not designed for:**
- Large enterprise applications requiring massive scale
- Teams that need extensive corporate compliance features
- Projects requiring heavy client-side interactivity (SPAs)

As creator Kristoffer Walker says:
> "We have to have a worldview and understand that means it's going to be polarizing. Some people are going to love it. And some people are going to say, just doesn't work for me."

## Contributing

This is community documentation - contributions are welcome!

### How to Contribute

1. **Found an error?** Open an issue
2. **Have a better explanation?** Submit a PR
3. **Discovered a new gotcha?** Add it to the docs
4. **Built something cool?** Share an example

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines.

### What We Need

- More real-world examples
- Additional how-to guides
- API documentation completion
- Architecture deep-dives
- Video tutorials / screencasts

## Origin Story

This documentation was created while building [Consulting for Normal People](https://consultingfornormalpeople.com), a production Kixx application. Every convention documented here was discovered through actual development work.

The docs were created collaboratively between human developers and AI coding assistants (primarily Claude), demonstrating best practices for AI-assisted documentation.

## Related Projects

- **Official Kixx Website**: https://www.kixx.dev
- **Kixx on npm**: https://www.npmjs.com/package/kixx
- **Kixx GitHub**: https://github.com/kixxauth/kixx
- **Example Application**: [CFNP Website](https://github.com/saouderkirk/cfnp) (built with these docs)
- **Creator's Website**: https://chriswalker.me
- **Video Tutorials**: [@essayohh on YouTube](https://www.youtube.com/@essayohh)

## Maintainers

Created and maintained by [@saouderkirk](https://github.com/saouderkirk) with contributions from the community.

Framework created by [Kristoffer Walker](https://chriswalker.me) ([@kixxauth](https://github.com/kixxauth))

## License

This documentation is licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/) - free to use, share, and adapt with attribution.

## Support

- **Questions?** Open a [GitHub Discussion](../../discussions)
- **Bug in docs?** Open an [Issue](../../issues)
- **Want to help?** See [Contributing](#contributing)

---

**Keywords**: Kixx, Node.js, web framework, documentation, conventions, routing, templates, Handlebars, server-side rendering, deployment, Railway, community docs, AI-assisted development, primitive components, MVC, indie hackers

Built with ❤️ using [Kixx](https://www.kixx.dev) • Documentation created with [Claude Code](https://claude.com/claude-code)
