# Markdown Writing Standards

Standards and best practices for writing semantic, accessible markdown content in LeGIT.

## Overview

All markdown content in LeGIT must follow consistent formatting and structure standards to ensure:
- **Consistency** - Uniform look and feel across all content
- **Accessibility** - Content readable by screen readers and accessible to all learners
- **Maintainability** - Clear structure makes content easier to update and reuse
- **Semantic Correctness** - Proper use of markdown elements for their intended purpose

## Key Standards

### Heading Hierarchy
Use proper heading levels to structure your content:
- **H1** (`#`): One per document for the main title
- **H2** (`##`): Major sections
- **H3** (`###`): Subsections
- **H4+**: Content within subsections

Never skip heading levels (e.g., don't jump from H1 to H4).

### Text Formatting
Use formatting intentionally:
- **Bold** for important terms and emphasis
- *Italic* for foreign words and variable names
- `Code` for file names, commands, and technical terms

Avoid using ALL CAPS for emphasis; use **bold** instead.

### Images and Media
- Always provide descriptive alt text
- Use relative paths (e.g., `./media/image.png`)
- Prefer SVG over raster formats when possible
- Include meaningful captions when appropriate

### Links
- Use relative paths for internal links
- Use descriptive link text (not "Click here")
- For external links, ensure they're current and relevant

### Lists
- Use lists (not paragraphs) when presenting multiple items
- Use `-` for bullet lists
- Use `1.`, `2.`, etc. for numbered lists
- Maintain consistent indentation for nested lists

### Code Blocks
- Always specify the language in code fences
- Use proper indentation and spacing
- Include comments for non-obvious code

## Reusability: DRY Principles

### Variables
Use YAML variables for values you'll repeat:
```yaml
varsLocal:
  productName: CompactLogix L20
  version: 32.0
```

Then reference them in markdown: `%varsLocal.productName%`

### Includes
Reuse common sections with includes:
```markdown
!include(../../shared/common-setup.md)
```

Don't copy the same content into multiple files.

## Validation

Before finalizing content, check:

- [ ] Heading hierarchy is correct and logical
- [ ] All images have descriptive alt text
- [ ] Links use relative paths (except external)
- [ ] No inline HTML or styles
- [ ] No hard-coded repeated values
- [ ] Lists are properly formatted
- [ ] Code blocks have language specified
- [ ] Content is readable as plain text

---

## Complete Standards Reference

For the authoritative, detailed validation rules including all specific requirements and examples, see:

**→ [`.claude/rules/legit-markdown-standards.md`](../../.claude/rules/legit-markdown-standards.md)**

This file contains:
- Detailed rules with examples
- Common mistakes to avoid
- Quick reference tables
- Complete validation checklist

## Related Guides

- [Content Blocks Reference](./content-blocks-reference.md): Using special LeGIT blocks
- [YAML Guide](./yaml-guide.md): Writing YAML frontmatter
- [Best Practices](./best-practices.md): General authoring best practices
