---
title: LeGIT Presentation Structure Rules
description: Rules for creating presentations with reveal.js in LeGIT
category: presentation-standards
---

# LeGIT Presentation Rules

Rules and standards for creating reveal.js presentations in LeGIT.

## Presentation Setup

### Rule: Use Correct docType and CSS

**Good:**
```yaml
---
docType: revealjs
css: ../../style-rau-base/rau-presentation-customer.css
disableLayout: true
---
```

**Bad:**
```yaml
---
docType: presentation
css: rau-print.css
---
```

### Rule: Define Title in YAML, Not Markdown

**Good:**
```yaml
---
title: Presentation Title
subtitle: Optional subtitle
docType: revealjs
---
```

**Bad:**
```yaml
---
docType: revealjs
---

# Presentation Title
```

(Don't include H1 in markdown body)

## Slide Hierarchy Rules

### Rule: Use Proper Heading Levels

- **H2** (`##`): Main slides (horizontal transition)
- **H3** (`###`): Vertical slides (sub-slides)
- **H4+** (`####`): Content within a slide

**Good:**
```markdown
## Main Topic

### Sub-topic 1
Content

---

## Next Main Topic
```

### Rule: One Main Topic Per Slide Level

**Good structure:**
- H2: One major section
- Multiple H3s: Sub-topics within that section

## Slide Content Rules

### Rule: Keep Slides Visually Uncluttered

**Good:**
- 3-4 bullet points per slide maximum
- One main idea per slide
- Use images instead of text walls

**Bad:**
- Full paragraphs on slides
- 7+ bullet points
- Dense text content

### Rule: Use Bold for Emphasis

**Good:**
```markdown
## Important Concept

This is **critical** for understanding the next section.
```

**Bad:**
```markdown
## IMPORTANT CONCEPT

This is critical for understanding the next section.
```

## Speaker Notes and Scripts

### Rule: Include Speaker Notes

Use `::: notes` blocks for presenter guidance:

```markdown
## Slide Title

Visible slide content here.

::: notes

More detailed talking points visible only to the presenter.

:::
```

### Rule: Include Script for Video Output

For presentations that will be recorded as video:

```markdown
## Slide Title

Visible slide content.

::: notes

Talking points for presenter.

::: script

This is the exact script that will be read for the video.

:::

:::
```

## Image and Media Rules

### Rule: Use High-Quality Images

**Good:**
- Professional screenshots
- Clear diagrams
- Vector graphics (SVG preferred)

### Rule: Use SVG When Possible

**Good:** `![diagram](media/architecture.svg)`  
**Acceptable:** `![photo](media/installation.jpg)`

### Rule: Provide Meaningful Alt Text

**Good:**
```markdown
![Network architecture with three connected devices](media/network.svg)
```

**Bad:**
```markdown
![diagram](media/network.svg)
![](media/network.svg)
```

## Column Layouts

### Rule: Use Columns for Side-by-Side Content

```markdown
::: {.columns}

::: {.column width="50%"}
Left content
:::

::: {.column width="50%"}
Right content
:::

:::
```

## Validation Checklist

Before finalizing a presentation:

- [ ] docType is `revealjs`
- [ ] CSS is appropriate for audience
- [ ] disableLayout set correctly
- [ ] Title in YAML, not markdown
- [ ] Heading hierarchy is clean
- [ ] All images have alt text
- [ ] Speaker notes included
- [ ] Script included if recording video
- [ ] Slide content is concise
- [ ] No text walls or dense paragraphs
