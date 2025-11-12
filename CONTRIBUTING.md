# Contributing to Kixx Community Documentation

Thank you for your interest in improving Kixx documentation! This guide will help you contribute effectively.

## How You Can Help

### 1. Report Issues

Found something wrong or confusing?

- **Incorrect information**: Open an issue with details
- **Broken links**: Let us know which links need fixing
- **Unclear explanations**: Suggest improvements
- **Missing topics**: Request documentation for uncovered areas

[Open an Issue](../../issues/new)

### 2. Improve Existing Documentation

- Fix typos and grammar
- Clarify confusing explanations
- Add missing details
- Update outdated information
- Improve code examples

### 3. Add New Content

We especially need:

- **Real-world examples** from your projects
- **How-to guides** for common tasks
- **Troubleshooting tips** for errors you've encountered
- **Performance tips** and best practices
- **API documentation** completion
- **Architecture explanations**

### 4. Share Your Experience

- Add your use case to examples
- Document gotchas you discovered
- Share deployment experiences
- Contribute error solutions

## Documentation Standards

### Writing Style

**For Humans:**
- Clear, concise language
- Active voice
- Short paragraphs
- Practical examples
- Real-world context

**For AI Agents:**
- Structured format (headings, lists, tables)
- Code examples with clear markers (❌ Wrong, ✅ Correct)
- Explicit "Why" explanations
- Machine-readable patterns

### File Organization

```
├── README.md                    # Main documentation index
├── AGENT_REFERENCE.md           # Quick reference for AI
├── CONTRIBUTING.md              # This file
├── conventions/                 # Conventions and patterns
│   └── README.md
├── troubleshooting/             # Error solutions
│   └── README.md
├── how-to-guides/               # Step-by-step guides
│   ├── README.md
│   └── 01-guide-name.md
├── api-reference/               # API documentation
├── architecture/                # How Kixx works
└── examples/                    # Code examples
```

### Markdown Format

**Headers:**
```markdown
# H1 for page title
## H2 for main sections
### H3 for subsections
```

**Code Examples:**
````markdown
```javascript
// Always include language identifier
const example = "code here";
```
````

**Wrong vs. Correct:**
```markdown
❌ **WRONG**: `pages/index.jsonc`
✅ **CORRECT**: `pages/page.jsonc`
```

**File Paths:**
```markdown
[filename.ts:42](path/to/file.ts#L42)  # Links to specific line
```

### TODO Markers

Use TODO markers for incomplete sections:

```markdown
**TODO**: Document this feature when we encounter it in a real project.
```

This helps track what needs to be filled in.

## Pull Request Process

### Before You Start

1. **Check existing issues/PRs** to avoid duplicate work
2. **Open an issue first** for major changes (optional but recommended)
3. **Keep PRs focused** - one topic per PR

### Creating a PR

1. **Fork the repository**
2. **Create a feature branch**: `git checkout -b docs/your-topic-name`
3. **Make your changes**
4. **Test your links** - ensure all internal links work
5. **Commit with clear messages**:
   ```
   docs: add deployment guide for Heroku

   - Add step-by-step Heroku deployment instructions
   - Include environment variable configuration
   - Add troubleshooting section for common errors
   ```
6. **Push to your fork**: `git push origin docs/your-topic-name`
7. **Open a Pull Request** with:
   - Clear description of changes
   - Why the change is useful
   - Any related issues

### PR Review Process

- Maintainers will review within a few days
- May request changes or clarifications
- Once approved, your PR will be merged
- Your contribution will be credited in the commit

## Documentation from Experience

**The best contributions come from real usage:**

✅ **Good**: "When deploying to Railway, I got error X. Here's how I fixed it..."

✅ **Good**: "I discovered that pages need subdirectory structure after spending 2 hours debugging..."

❌ **Less helpful**: "The official docs say..." (we can link to official docs instead)

## Style Guide

### Language

- **US English** for consistency
- **Present tense**: "Kixx uses..." not "Kixx will use..."
- **Second person**: "You can..." not "We can..."
- **Active voice**: "Run the command" not "The command should be run"

### Code Examples

**Always include:**
- Context (what problem this solves)
- Full, working examples
- Expected output (if relevant)
- Common mistakes to avoid

**Example structure:**
```markdown
### Creating a New Page

**Problem**: You need to add an About page to your site.

**Solution**:

1. Create the subdirectory:
   ```bash
   mkdir -p pages/about
   ```

2. Add metadata file `pages/about/page.jsonc`:
   ```jsonc
   {
       "title": "About Us",
       "baseTemplateId": "base.html"
   }
   ```

3. Add content file `pages/about/page.md`:
   ```markdown
   # About Us

   Content here...
   ```

**Common Mistakes**:
❌ Using `pages/about.jsonc` (flat file - won't render content)
✅ Using `pages/about/page.jsonc` (subdirectory - works correctly)
```

## Questions?

- **Unclear about something?** Open an [issue](../../issues)
- **Want to discuss an idea?** Start a [discussion](../../discussions)
- **Need help with PR?** Tag maintainers in your PR

## Recognition

All contributors will be:
- Listed in commit history
- Credited in documentation (if significant contribution)
- Thanked in release notes (for major improvements)

## Code of Conduct

**Be kind and respectful:**
- Assume good intentions
- Provide constructive feedback
- Help newcomers
- Focus on improving documentation

**Unacceptable behavior:**
- Harassment or discrimination
- Trolling or inflammatory comments
- Personal attacks
- Publishing others' private information

---

Thank you for helping make Kixx documentation better! 🎉

