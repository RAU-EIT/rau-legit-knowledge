---
title: LeGIT Markdown Standards
description: Markdown syntax rules and standards for RAU learning content
category: content-standards
---

# LeGIT Markdown Writing Standards

Standards for writing semantic, accessible markdown content in LeGIT.

## Heading Rules

### Rule: Use Proper Heading Hierarchy

- One H1 (`#`) per document
- H2 (`##`) for major sections
- H3 (`###`) for subsections
- H4+ for content within subsections

**Good:**
```markdown
# Main Document Title

## Section A

### Subsection A1

Content

## Section B
```

**Bad:**
```markdown
# Title
#### Skips levels
## Main section
```

### Rule: Don't Skip Heading Levels

**Good:** H1 → H2 → H3 progression  
**Bad:** H1 → H4 (skipped H2 and H3)

### Rule: Use Sentence Case for Headings

**Good:** `## Getting started with markdown`  
**Bad:** `## GETTING STARTED WITH MARKDOWN`

## Text Formatting Rules

### Rule: Use Appropriate Emphasis

- **Bold** (`**text**`): Important terms, emphasis
- *Italic* (`*text*`): Foreign words, variable names
- `Code` (`` `text` ``): File names, commands

**Good:**
```markdown
Press the **Create** button to make a new file named `my-file.md`.
```

**Bad:**
```markdown
Press the Create button to make a new file named my-file.md.
```

### Rule: Avoid ALL CAPS Emphasis

**Good:** **Important** message  
**Bad:** IMPORTANT message

## List Rules

### Rule: Use Lists, Not Paragraphs, for Multiple Items

**Good:**
```markdown
Required software:
- Python 3.8+
- VSCode
- Git
```

**Bad:**
```markdown
You need Python 3.8 or higher, VSCode, and Git.
```

### Rule: Consistent List Formatting

- Use `-` for bullet lists
- Use `1.`, `2.`, etc. for numbered lists
- Use proper indentation for nested lists

## Code Block Rules

### Rule: Use Code Fences with Language Specification

**Good:**
````markdown
```python
def hello():
    print("Hello world")
```
````

**Bad:**
````markdown
```
def hello():
    print("Hello world")
```
````

(Missing language specification)

## Link Rules

### Rule: Use Relative Paths for Internal Links

**Good:**
```markdown
[Link text](./media/image.png)
[Reference](../../shared/file.md)
```

**Bad:**
```markdown
[Link text](https://example.com/image.png)
```

### Rule: Use Descriptive Link Text

**Good:**
```markdown
[Read about markdown syntax](./markdown-guide.md)
```

**Bad:**
```markdown
[Click here](./markdown-guide.md)
[Link](./markdown-guide.md)
```

## Image Rules

### Rule: Always Provide Descriptive Alt Text

**Good:**
```markdown
![A person installing a controller into a cabinet](media/installation.jpg)
```

**Bad:**
```markdown
![image](media/installation.jpg)
![](media/installation.jpg)
```

### Rule: Use Relative Paths for Images

**Good:**
```markdown
![alt text](./media/diagram.svg)
```

**Bad:**
```markdown
![alt text](C:\Users\Pictures\image.png)
```

### Rule: Prefer SVG Over Raster Formats

**Good:** `![alt](media/diagram.svg)`  
**Acceptable:** `![alt](media/photo.jpg)`  
**Avoid:** `![alt](media/diagram.png)` when SVG is available

## Semantic Markdown Rules

### Rule: Content Must Be Readable as Plain Text

**Good:** Clear structure visible without styles
```markdown
# Main Title

## Section 1

Key concepts:
- Concept A
- Concept B
```

### Rule: Use Proper Semantic Elements

- Headings for document structure
- Lists for multiple items
- Code for technical terms
- Emphasis for importance

**Don't:**
- Use bold to make text look like a heading
- Use inline HTML for formatting
- Use ALL CAPS for emphasis

## DRY (Don't Repeat Yourself) Rules

### Rule: Use YAML Variables for Repeated Values

**Good:**
```yaml
varsLocal:
  productName: CompactLogix L20
  version: 32.0
```

Then in markdown:
```markdown
The %varsLocal.productName% runs version %varsLocal.version%.
```

### Rule: Use Includes for Reusable Sections

**Good:**
```markdown
!include(../../shared/common-setup.md)
```

**Bad:** Copying the same content into multiple files

## File Structure Rules

### Rule: Use Consistent File Organization

```
lesson/
├── media/
│   ├── diagram.svg
│   ├── photo.jpg
├── lecture.md
├── lab.md
└── quiz.md
```

### Rule: Use Descriptive File Names

**Good:**
- `network-configuration.md`
- `installing-firmware.md`

**Bad:**
- `lesson01.md`
- `section2.md`

## Validation Rules

### Rule: Run Markdown Linter

The markdown linter should show no errors:
- Check spacing and formatting
- Verify heading structure
- Validate list indentation

### Rule: Verify Links and References

- All relative links are correct
- Images exist at referenced paths
- External links are valid

## Common Mistakes to Avoid

| Mistake | Good | Bad |
|---------|------|-----|
| Skipped headings | H1→H2→H3 | H1→H4 |
| Empty alt text | `![description](file)` | `![](file)` |
| ALL CAPS | **important** | IMPORTANT |
| Inline HTML | Native markdown | `<div>...</div>` |
| Inconsistent lists | All bullets or all numbered | Mixed styles |

## Quick Checklist

Before finalizing content:

- [ ] Heading hierarchy is correct and logical
- [ ] All images have descriptive alt text
- [ ] Links use relative paths (except external)
- [ ] No inline HTML or styles
- [ ] No hard-coded repeated values
- [ ] Lists are properly formatted
- [ ] Code blocks have language specified
- [ ] No linter warnings
