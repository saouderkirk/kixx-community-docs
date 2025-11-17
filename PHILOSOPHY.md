# Kixx Philosophy & Design Principles

Understanding **why** Kixx exists and **how** it thinks will help you build better applications faster.

## Table of Contents

- [The Problem Kixx Solves](#the-problem-kixx-solves)
- [Core Principles](#core-principles)
- [Core Philosophy](#core-philosophy)
- [Design Principles](#design-principles)
- [AI Integration Strategy](#ai-integration-strategy)
- [Who Kixx Is For](#who-kixx-is-for)
- [Historical Context](#historical-context)

---

## The Problem Kixx Solves

### The Gap in Web Development Tools

Kixx was created to fill a specific gap in the web development ecosystem that emerged around 2008-2009:

**The Too-Simple Problem:**
- Simple brochure websites (5-10 pages, rarely updated)
- WordPress was overkill - too slow to set up, too heavy to maintain
- Writing HTML by hand was actually faster
- Static site builders (like Jekyll) eventually solved this

**The Too-Complex Problem:**
- Custom applications with complex data models
- WordPress's content model couldn't handle sophisticated use cases
- Example: A coupon website for local businesses didn't fit into "posts" and "pages"
- Rails/Django were powerful but heavy for small projects

**The Sweet Spot:**
- Nonprofit websites
- Hockey team schedulers
- Avalanche condition trackers
- Small business tools
- Projects you want to finish "on a weekend, realistically"

### Real-World Examples

From creator Kristoffer Walker:

> "There's really nifty little things like an avalanche reporting website that I work on for backcountry skiers. I work on stuff for some ice rinks, some scheduling systems for a hockey team... none of those are things that nonprofit organizations could spend $100,000 with developers to build."

These are sophisticated systems (user-generated content, video processing, email notifications, admin workflows) but built rapidly with minimal overhead.

---

## Core Principles

**From the official Kixx documentation** - These are the foundational principles that guide all decisions in the Kixx framework:

### 1. Optimize for productivity

When a decision needs to be made, we optimize for developer productivity. This is the north star of the Kixx framework, and most of the remaining principles derive from it.

### 2. Have opinions

Kixx has opinions about building great web applications. In our opinion, a good framework needs to have opinions, otherwise, what's the point of using a framework?

### 3. Software is a craft

Humans write software for humans, and even in the age of AI, building software is, and will always be a craft done by a craftsperson.

### 4. Remove complexity

There will always be complexity in the problems we choose to solve with our software. But, Kixx will seek out ways to bury incidental complexity and keep our brains concentrated on the real problems.

### 5. The World Wide Web is the best application platform ever invented

Nothing has ever been created that matches the accessibility, openness, power, and distribution of the Web. Kixx is fully committed to improving and contributing to the WWW.

### 6. AI tools can be built and used without negative moral, ethical, or environmental impacts

We can develop AI tools which are small, productive, focused on the craft of software development, and don't slurp up massive amounts of energy.

### 7. Favor monolithic applications over distributed architectures

Distributed microservices might be great for large engineering teams, but Kixx is made for solo developers and small, fast moving teams. Monolithic, hypermedia-driven applications, without the complexity of microservices and piles of client side JavaScript, give us a massive velocity boost over our distributed counterparts.

### 8. Convention over Configuration and Code

Wherever possible Kixx uses conventions over configuration and code for common web application logic. This dramatically reduces the amount of code that needs to be written, generated, and reasoned about to build a web app.

---

## Core Philosophy

### 1. Simplicity Over Complexity

**The Web Doesn't Need More JavaScript**

From the creator:
> "We download hundreds of megabytes of JavaScript to take 20 or 30 seconds to parse and interpret in the browser. Let's just write HTML. Most of the applications that we can work with can do that. We don't need these massive, complex JavaScript ecosystems that are really hard to work with."

**What This Means:**
- Server-side rendering by default
- Minimal client-side JavaScript
- Fast page loads
- Better accessibility
- Simpler debugging

### 2. Convention Over Configuration

**Make Decisions So Developers Don't Have To**

- File structure defines routing
- Standard naming conventions (`page.jsonc`, not `index.jsonc`)
- Sensible defaults that work out of the box
- Override when needed, but defaults handle 80% of cases

### 3. Progression Over Perfection

**Inspired by Mountain Biking Philosophy**

From the video:
> "There's a term in mountain biking called progression, where you pick a relatively easy obstacle and you work on that one until you get it, and then you pick a more difficult obstacle... Until the next thing you know, you can ride a whole trail full of medium to hard obstacles and get in that flow state."

**Applied to Kixx:**
- Start with simple projects, learn incrementally
- Each feature you build teaches you the next pattern
- Documentation for "small things" helps you build "big things"
- Get into flow state quickly (developer fun!)

### 4. Primitive Components

**Small, Composable Building Blocks**

Inspired by:
- **AWS** - S3, RDS, Lambda as discrete services
- **Firefox XP-COM** - Cross-platform component model
- **Rails MVC** - Models, Views, Controllers as primitives

**In Kixx:**
- Emailer component (sends emails)
- Job scheduler component (schedules tasks)
- Combine them → automated confirmation emails
- Each component is simple; power comes from composition

From the creator:
> "You don't have to read much or learn too much to be able to do that, but it feels much more advanced than when you first launched the application and didn't send emails."

### 5. Developer Fun

**Expand the Realm of Fun**

What makes development fun:
- **Being productive** - Actually shipping things
- **Flow state** - Challenging but not overwhelming
- **Weekend projects** - Finish something satisfying quickly
- **Learning** - Building mastery through progression

From the video:
> "What's the tipping point where it makes it worth it to be able to do that? If you have a framework that allows you to work really rapidly, then it becomes within the realm of possibility, within the realm of fun, to do it on a weekend."

---

## Design Principles

### 1. Minimal Context for Humans

**You Don't Need to Learn Everything to Build Something**

- Small, focused documentation
- Each primitive component fits in your head
- Work with 1-3 components at a time
- Example: "Email + Job Scheduler" is all you need for confirmations

### 2. Minimal Context for AI

**LLMs Work Better with Less**

Traditional approach:
```
LLM ingests 400,000 tokens of entire codebase
→ Slow processing
→ High energy consumption
→ Lower quality output
```

Kixx approach:
```
LLM ingests documentation for 2 components (emailer + scheduler)
→ Fast processing
→ Low energy consumption
→ High quality, deterministic output
```

### 3. Structured Over Unstructured

**More Structure = Better AI Results**

From the creator:
> "If you take it one level higher and you provide even more structure than code, it becomes even more deterministic and the output is better. Higher quality output with lower amounts of input, lower energy consumption, lower time."

**What this means:**
- Clear component boundaries
- Consistent naming conventions
- Documented interfaces
- Predictable patterns

### 4. Server-Side by Default

**Deliver HTML, Not Megabytes of JavaScript**

- Faster initial page loads
- Better SEO
- More accessible
- Simpler mental model
- Page reloads are fast enough

### 5. File-Based Everything

**Your Directory Structure IS Your Application**

- Pages defined by file structure
- Routes follow file conventions
- No separate routing configuration
- What you see (files) is what you get (URLs)

---

## AI Integration Strategy

### The Environmental & Quality Problem

From the creator:
> "We have enough garbage code out there today in the web. We don't need to be generating heaps more of it. Now we have machines that can 10X the amount of junk code that we deliver."

### The Kixx Solution

**1. Smaller Context Windows**

Instead of feeding the LLM your entire codebase:
- Give it documentation for specific components
- Example: "Emailer" + "Job Scheduler" docs only
- LLM focuses on connecting two things, not understanding everything

**2. MCP (Model Context Protocol) Servers**

A plugin for your local AI agent (Cursor, Claude Code, etc.) that:
- Lives on your laptop
- Answers LLM questions about Kixx components
- Provides just-in-time documentation
- Enables deterministic code generation

**Example flow:**
```
You: "Send an email when a new user signs up"
↓
LLM sees "Kixx framework" keyword
↓
LLM asks MCP: "How do I send email in Kixx?"
↓
MCP returns Emailer component docs
↓
LLM asks MCP: "How do I detect new users in Kixx?"
↓
MCP returns database listener docs
↓
LLM generates minimal code to connect them
```

**3. Human as Orchestrator**

You don't write thousands of lines of code. You:
- Decide which components to use
- Ask AI to connect them
- Review the (small) generated code
- Ship features rapidly

### Benefits

1. **Environmental** - Less compute = less energy
2. **Quality** - Smaller context = better output
3. **Speed** - Local processing possible
4. **Learning** - You understand the components, not just generated code

---

## Who Kixx Is For

### Perfect For

**Indie Hackers**
- Building multiple projects
- Need to ship quickly
- Want to learn progressively
- Value "developer fun"

**Small Teams (1-3 people)**
- Nonprofits with limited budgets
- Small businesses needing custom tools
- Consulting projects with tight timelines
- Side projects that need to actually ship

**Hobbyists**
- Weekend projects
- Personal websites
- Learning web development
- Experimenting with ideas

### Not Designed For

**Enterprise Scale**
- Applications with millions of users
- Complex compliance requirements
- Large teams needing extensive governance
- Projects requiring enterprise support contracts

**Heavy Client-Side Interactivity**
- Single Page Applications (SPAs)
- Real-time collaborative editing
- Complex client-side state management
- Apps that work offline

### The Polarizing Truth

From creator Kristoffer Walker:
> "We have to have a worldview and understand that means it's going to be polarizing. Some people are going to love it. And some people are going to say, just doesn't work for me. And I think that's okay."

**Kixx has strong opinions about:**
- Server-side rendering > Client-side rendering
- Convention > Configuration
- Simplicity > Features
- Small teams > Enterprise

If you disagree with these, that's fine! There are many other frameworks that might fit your needs better.

---

## Historical Context

### Timeline of Influences

**2008-2009: The First Kixx**
- Filling gaps in WordPress
- Need for rapid small project development
- Set aside when Rails emerged

**2010-2011: Ruby on Rails Era**
- Learned MVC patterns
- Understood convention-over-configuration
- Saw power of opinionated frameworks

**2012+: Static Site Builders**
- Jekyll and similar tools
- Markdown + HTML templates
- Solved the "too simple" problem

**Video Game Industry: Node.js at Scale**
- Millions of users
- Hundreds of thousands concurrent
- No good Node.js framework like Rails
- Express too minimal
- Developed custom solutions

**2024-2025: The Modern Web Problem**
- Pages getting heavier every year
- Hundreds of megabytes of JavaScript
- More bugs, slower sites
- Environmental concerns

**2024: AI/LLM Revolution**
- Cloud Code and specialized AI tools emerge
- Opportunity to rethink code generation
- Smaller context = better results
- Kixx as AI-optimized framework

### Pattern Recognition

From the creator:
> "I've seen that pattern play out multiple times where we kind of over-solve a problem in the technical world to the point where it's just too heavy to work with anymore, and then simplify it. And maybe that's the natural way of things."

**The Cycle:**
1. Simple solution (hand-coded HTML)
2. Add complexity (WordPress, SPAs, massive JavaScript)
3. Too complex, return to simplicity (Kixx, server-side rendering)
4. Repeat

---

## References & Further Reading

**Mentioned in the Videos:**

- **"Peak" by Anders Ericsson** - Expertise through deliberate practice and pattern recognition
- **"The Individual is the Platform" by Anu** - One person amplified by modern tools
- **Firefox XP-COM** - Cross-platform component model
- **Ruby on Rails** - Convention over configuration
- **Jekyll** - Static site builder philosophy

**Related Concepts:**

- **Progression** (mountain biking) - Incremental skill building
- **Flow State** - Optimal challenge level
- **Primitive Components** - Small, composable building blocks
- **Model Context Protocol (MCP)** - AI agent plugin system

---

## Conclusion

Kixx exists to make web development **fun** again by:

1. **Simplifying** what you need to learn
2. **Accelerating** how fast you can ship
3. **Reducing** environmental impact
4. **Enabling** small teams to build sophisticated things
5. **Leveraging** AI without sacrificing quality

If you want to build real projects quickly, understand how they work, and have fun doing it — Kixx might be for you.

If you need enterprise scale, complex client-side interactions, or don't agree with server-side rendering — that's okay! Kixx has strong opinions, and not every framework is for every project.

---

**"Fun for a developer, fun for a user, fun for the owner."** - Kristoffer Walker

---

**Kixx Framework Copyright**
Copyright: (c) 2017 - 2025 by Kris Walker ([www.kriswalker.me](https://www.kriswalker.me))

Unless otherwise indicated, all source code is licensed under the MIT license.

**This Documentation**
Built with ❤️ using [Kixx](https://www.kixx.dev) • Licensed under [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/)
