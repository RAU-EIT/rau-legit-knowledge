---
title: LeGIT YAML Attributes and Configuration Rules
description: YAML attribute validation and best practices for LeGIT documents
category: configuration
---

# LeGIT YAML Attribute Rules

YAML metadata attributes define how LeGIT documents are processed, styled, and published.

## Required Attributes

### Rule: Every Document Must Have Required Frontmatter

**Required in every markdown file:**

```yaml
---
title: Document Title
docType: print  # or: revealjs, scorm
css: ../style-rau-base/rau-print.css
skill:
  id: SKL12345
  revisionDate: 2025-08
  classification: Public
---
```

**Required fields:**
- `title` — Document title (single line, sentence case)
- `docType` — Output type (print, revealjs, or scorm)
- `css` — Relative path to stylesheet
- `skill.id` — Unique skill identifier
- `skill.revisionDate` — Revision date in YYYY-MM format
- `skill.classification` — Public, Internal, or Confidential

## docType Rules

### Rule: Use Valid docType Values Only

**Valid values:**
- `print` — PDF output (lab manuals, guides)
- `revealjs` — HTML presentations (reveal.js)
- `scorm` — E-learning modules

**Good:**
```yaml
docType: print
```

**Bad:**
```yaml
docType: PDF
docType: web
docType: presentation
```

### Rule: Match CSS to docType

| docType | Recommended CSS |
|---------|-----------------|
| print | rau-print.css or rau-print-single.css |
| revealjs | rau-presentation-basic.css or rau-presentation-customer.css |
| scorm | rau-scorm.css |

## CSS Path Rules

### Rule: CSS Path Must Be Relative

**Good:**
```yaml
css: ../style-rau-base/rau-print.css
css: ../../shared-styles/rau-scorm.css
```

**Bad:**
```yaml
css: C:\Users\Name\rau-print.css
css: /absolute/path/rau-print.css
```

### Rule: Use Unix-Style Path Separators

**Good:** `css: ../path/to/file.css`  
**Bad:** `css: ..\path\to\file.css`

## Skill Metadata Rules

### Rule: Skill Must Have Three Sub-Attributes

```yaml
skill:
  id: SKL12345
  revisionDate: 2025-08
  classification: Public
```

All three sub-attributes are required.

### Rule: revisionDate Must Be YYYY-MM Format

**Good:**
```yaml
revisionDate: 2025-08
revisionDate: 2024-01
```

**Bad:**
```yaml
revisionDate: 08/2025
revisionDate: August 2025
```

### Rule: classification Must Be Valid

**Valid values:**
- `Public`
- `Internal`
- `Confidential`

## Variable Rules

### Rule: Use varsLocal for Document-Specific Variables

```yaml
varsLocal:
  lessonTitle: Basic Configuration
  estimatedTime: 45 minutes
```

### Rule: Use varsGlobal for Build-Wide Variables

Should be defined in build.yaml or frontmatter, not individual markdown files.

```yaml
varsGlobal:
  audience: customer
  productVersion: 32.0
```

## Common Mistakes

| Mistake | Good | Bad |
|---------|------|-----|
| Invalid docType | `revealjs` | `presentation` |
| Wrong CSS | Match to docType | Mismatched |
| Missing skill attributes | All 3 fields | 1 or 2 fields |
| Wrong date format | `2025-08` | `August 2025` |
| Bad classification | `Public` | `public` |
| Absolute CSS path | `../path/file.css` | `C:\path\file.css` |

## Quick Checklist

Before finalizing YAML:

- [ ] docType is valid (print, revealjs, scorm)
- [ ] CSS path is relative and file exists
- [ ] skill has id, revisionDate, classification
- [ ] revisionDate is YYYY-MM format
- [ ] classification is valid (Public, Internal, Confidential)
- [ ] title is single line, sentence case
