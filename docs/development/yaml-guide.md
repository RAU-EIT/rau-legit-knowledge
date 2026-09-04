# YAML Configuration Guide

This guide documents the YAML metadata attributes used in LeGIT markdown documents. YAML frontmatter defines document properties, styling, variables, and publishing options.

## Table of Contents

- [Where YAML Goes](#where-yaml-goes)
- [Common Attributes](#common-attributes)
- [Presentation-Only Attributes](#presentation-only-attributes)
- [Print-Only Attributes](#print-only-attributes)
- [SCORM-Only Attributes](#scorm-only-attributes)
- [Variables and Conditionals](#variables-and-conditionals)

---

## Where YAML Goes

YAML metadata can be provided in three places:

1. **`build.yaml`**: Build configuration file
2. **Frontmatter YAML files**: Standalone YAML documents (typically for cover pages)
3. **Markdown file headers**: YAML block at the top of markdown files (most common)

---

## Common Attributes

### docType
Sets the publication type for the document.

**Required:** Yes  
**Valid Values:** `print`, `revealjs`, `scorm`

### title
The document title shown in output.

**Required:** Yes

### subtitle
Subtitle shown on presentation title slides.

**Required:** No

### author
Author information for documents.

**Required:** No (but recommended for multi-file builds)

### css
CSS stylesheet for styling the document.

**Required:** Yes

Available stylesheets:
- `rau-print.css`: Lab manuals and print documents
- `rau-scorm.css`: E-learning content
- `rau-presentation-basic.css`: Internal/informal presentations
- `rau-presentation-customer.css`: Customer-facing presentations

### skill
Skill metadata used in footers and publication information.

**Required:** Yes

**Subattributes:**
- **id**: Skill identifier (e.g., ITM55555)
- **revisionDate**: Last revision date (YYYY-MM format)
- **classification**: Public, Internal, Confidential, or Restricted

### vars
Custom variables for use in document content.

**Recommended Structure:**
```yaml
varsLocal:
  lessonTitle: Identifying a Controller
varsGlobal:
  audience: customer
  productVersion: 32.0
```

---

## Presentation-Only Attributes

### title-slide-attributes
Configure the title slide layout and background.

### disableLayout
Disable RevealJS default centered layout (set `true` for customer presentations).

### titleNotes
Speaker notes for the title slide.

### titleScript
Script text for video narration or screen readers.

---

## Print-Only Attributes

### toc-title
Title for the table of contents.

### tagline
Three lines of text on the document cover page.

---

## SCORM-Only Attributes

### scorm-title
Title for the SCORM module launch page.

### scorm-subtitle
Subtitle for the SCORM module launch page.

### scorm-desc
Description text for the SCORM module.

---

## Variables and Conditionals

### Using Variables in Content

Variables can be inserted using the `%variable%` syntax:

```markdown
# %varsLocal.lessonTitle%

You are working with **%varsGlobal.productVersion%**.
```

### Conditional Content Filtering

Content can be included or excluded based on YAML variables.

**Inline syntax:**
```markdown
[This shows for customers]{if="varsGlobal.audience==customer"}
```

**Block syntax:**
```markdown
::: {if="varsGlobal.audience!=internal"}

This content is hidden for internal audiences.

:::
```

**Supported Operators:** `==` (equals), `!=` (not equals)

---

For complete YAML attribute reference, see the rau-start-here documentation.
