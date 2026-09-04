# Presentation Standards

Standards and best practices for creating reveal.js presentations in LeGIT.

## Overview

Presentations in LeGIT use reveal.js for interactive, web-based slideshows. This guide covers the standards you must follow for:
- **Consistency** - Uniform presentation structure
- **Interactivity** - Proper use of reveal.js features
- **Speaker Support** - Integrating speaker notes and scripts
- **Accessibility** - Ensuring presentations are accessible to all learners

## Getting Started

### Required YAML Metadata

Every presentation file must include:
```yaml
---
title: Your Presentation Title
subtitle: Optional subtitle
docType: revealjs
css: ../../style-rau-base/rau-presentation-customer.css
disableLayout: true
---
```

**Key points:**
- Define the title in YAML, not as an H1 in the document body
- Use `revealjs` as the docType (not "presentation")
- Point to the correct CSS file for your audience
- Set `disableLayout: true` to control slide structure

## Slide Structure

### Heading Hierarchy
Reveal.js uses heading levels to create navigation:
- **H2** (`##`): Main slides (horizontal navigation)
- **H3** (`###`): Vertical sub-slides (stack beneath main slide)
- **H4+** (`####`): Content within a slide

### Example Structure
```markdown
## Main Topic 1
This is a main slide.

### Sub-topic 1.1
This sub-slide appears below the main topic.

### Sub-topic 1.2
Another sub-slide.

---

## Main Topic 2
Next main topic starts fresh horizontally.
```

## Slide Content Standards

### Keep It Simple
- Maximum 3-4 bullet points per slide
- One main idea per slide
- Use images instead of text walls
- Avoid dense paragraphs

### Emphasis
- Use **bold** for emphasis, not ALL CAPS
- Use italics sparingly for special terms
- Let visual hierarchy do the work

### Images and Media
- Use high-quality, professional visuals
- Prefer SVG over raster formats
- Always include descriptive alt text
- Ensure images are properly sized for slides

### Column Layouts
For side-by-side content:
```markdown
::: {.columns}

::: {.column width="50%"}
Left column content
:::

::: {.column width="50%"}
Right column content
:::

:::
```

## Speaker Support

### Speaker Notes
Include presenter-only notes visible only to the presenter:

```markdown
## Slide Title

Visible content here.

::: notes

Detailed talking points for the presenter.

:::
```

### Video Scripts
If recording the presentation as video, include the exact script:

```markdown
## Slide Title

Visible content.

::: notes

Talking points for presenter.

::: script

This is the exact script to read for the recorded video.

:::

:::
```

## Quality Checklist

Before finalizing your presentation:

- [ ] Title is defined in YAML, not as H1
- [ ] docType is `revealjs`
- [ ] CSS is appropriate for your audience
- [ ] Heading hierarchy is clean and logical
- [ ] All images have descriptive alt text
- [ ] Slide content is concise (3-4 bullet points max)
- [ ] Speaker notes are included for complex topics
- [ ] Script is included if recording as video
- [ ] No text walls or dense paragraphs
- [ ] Column layouts are properly structured

## Templates and Examples

Starter templates for common presentation types:
- **Instructor-led**: Include detailed speaker notes
- **Self-paced e-learning**: Include comprehensive speaker notes and scripts
- **Short update**: Minimal notes, focused content

## Complete Standards Reference

For the authoritative, detailed validation rules including all specific requirements and examples, see:

**→ [`.claude/rules/legit-presentations.md`](../../.claude/rules/legit-presentations.md)**

This file contains:
- Detailed rules with code examples
- Common mistakes to avoid
- Complete validation checklist

## Related Guides

- [Content Blocks Reference](./content-blocks-reference.md): Using special LeGIT blocks in presentations
- [Markdown Standards](./markdown-standards.md): Basic markdown syntax standards
- [YAML Guide](./yaml-guide.md): YAML frontmatter reference
- [Best Practices](./best-practices.md): General authoring best practices
