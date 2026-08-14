---
title: LeGIT Content Blocks Rules
description: Validation and best practice rules for RAU markdown content blocks
category: content-validation
---

# LeGIT Content Blocks Validation Rules

Rules for using RAU markdown content blocks correctly and effectively.

## Block Structure Rules

### Rule: Use Fenced Divs with Proper Syntax

**Standard block:**
```markdown
::: block-name
content here
:::
```

**Block with attributes:**
```markdown
::: {.block-name .additional-class attribute="value"}
content here
:::
```

### Rule: Validate Closing Markers

**Good:** Matching opening/closing divs
```markdown
::: rau-accordion
content
:::
```

## Block-Specific Rules

### Alert Blocks

**Rule: Use correct alert type classes**

Valid types: `.reference`, `.warning`, `.attention`, `.important`, `.tip`

**Good:**
```markdown
::: {.rau-alert .warning}
Important safety information
:::
```

### Accordion

**Rule: Use consistent tab structure**

Each tab must have:
- Opening `::: tab` marker
- H3 heading as first element
- Content
- Closing `:::` marker

**Good:**
```markdown
::: rau-accordion
::: tab
### Tab Title
Content here
:::
:::
```

### Quiz

**Rule: Set configuration options correctly**

```yaml
---
enableRetry: true/false
passPercent: 0-100
---
```

## General Best Practices

### Rule: Use Blocks Intentionally

**Good use:** Structure and functionality
- Accordion for FAQs
- Steps for procedures
- Quiz for assessment

**Bad use:** Styling only

### Rule: Validate Media References

All images/videos in blocks should:
- Use relative paths: `./media/image.png`
- Include descriptive alt text
- Reference existing files
- Use SVG when possible

### Rule: Test Interactive Blocks

Before publishing, test:
- Accordion tabs open/close
- Steps navigation works
- Quiz scoring calculates
- Video/audio playback works

## Using VSCode Snippets

**Rule: Always use snippets for accuracy**

1. Press `Ctrl+Space`
2. Type `rau`
3. Select block from suggestions

This ensures correct syntax and structure.

## Validation Checklist

Before considering a block valid:

- [ ] Block syntax is correct (opening/closing markers)
- [ ] All required attributes are present
- [ ] Attribute values are valid
- [ ] Media files exist and referenced correctly
- [ ] Alt text is present and descriptive
- [ ] Block serves a functional purpose
- [ ] Tested in target output format
