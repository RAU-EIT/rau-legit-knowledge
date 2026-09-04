---
name: develop-training
description: Help content authors translate learning designs into LeGIT markdown files ready for building
aliases: [develop, author, create-content, write-training]
whenToUse: You have a validated design and are ready to write content files
---

# Develop RAU Training Content

Ready to author your training content? This skill guides you through translating your design into complete, validated LeGIT markdown files.

## Quick Start

You'll author:
1. **Lectures**: Core instruction for each objective
2. **Knowledge Checks**: Embedded questions within lectures
3. **Labs**: Hands-on practice exercises
4. **Quizzes**: Assessment questions
5. **YAML Frontmatter**: Metadata for each file

By the end, you'll have:
- ✅ Complete file set ready for building
- ✅ Valid LeGIT markdown syntax
- ✅ All standards validated
- ✅ Ready for PDF/presentations/SCORM output

## Before You Start

**You need:**
- ✅ A validated learning design (from `/design-training`)
- ✅ File structure scaffolding (I can generate this)
- ✅ Content outline or SME input (what should be taught)

**I'll help with:**
- ✅ File structure and naming
- ✅ Lecture templates
- ✅ Knowledge check syntax
- ✅ Lab structure
- ✅ Quiz formatting
- ✅ YAML frontmatter
- ✅ Content block selection
- ✅ Validation against standards

## How It Works

### Phase 1: Setup
1. You provide your validated design
2. I generate file scaffolding
3. We create all the empty files

### Phase 2: Lecture Authoring
1. I guide you through each objective lecture
2. You provide content outline
3. I help structure with headings, lists, emphasis
4. I suggest content blocks (alerts, columns, images)
5. I validate against markdown standards

### Phase 3: Knowledge Checks
1. For each lecture, I help create embedded checks
2. Multiple choice, short answer, or matching
3. Integrated inline with lecture content

### Phase 4: Labs (if applicable)
1. For standalone objectives, I help design labs
2. Equipment needs, procedure steps, success criteria
3. Hands-on practice structure

### Phase 5: Quizzes
1. Aggregate quiz questions from all objectives
2. Question pool with multiple formats
3. Scoring and validation logic

### Phase 6: YAML & Validation
1. Generate complete YAML frontmatter
2. Run all validation rules
3. Fix any issues
4. Ready to build!

---

## Step-by-Step

### Step 1: Provide Your Design

To begin, share your learning design:

```
Option A: Share your design JSON
  (if you have the file from /design-training)

Option B: Share basic info
  - Skill name
  - Number of outcomes
  - Number of objectives
  - Modality (e-learning, classroom, blended)

Option C: Paste your design outline
  (if you have it in text format)
```

I'll use this to:
- Generate file scaffolding
- Show you the complete file list
- Help you organize content

---

### Step 2: File Scaffolding

Once I have your design, I'll show:

```
FILE STRUCTURE FOR: [Skill Name]

skills/[skill-slug]/
├── [outcome-1-slug]/
│   ├── objective-01/
│   │   ├── lecture.md              (always)
│   │   ├── knowledge-check.md      (always)
│   │   ├── quiz-questions.md       (always - feeds outcome quiz pool)
│   │   ├── lab.md                  (only if standalone)
│   │   └── media/
│   ├── objective-02/
│   │   └── ...
│   ├── outcome-01-lecture.md
│   ├── outcome-01-lab.md           (for non-standalone objectives)
│   ├── outcome-01-quiz.md
│   └── media/
├── assets/                         (supporting assets: VMs, project files)
└── media/

Total files to create: [X] files
Ready to start authoring?
```

---

### Step 3: Authoring Lectures

For each objective, I'll guide:

```
OBJECTIVE 1: Identify Basic Hydraulic Components

Content outline needed:
1. What components will learners identify?
2. How are they structured?
3. What functions do they serve?
4. Common variations or configurations?

Provide your outline (bullet points or sentences):
- Pump: creates flow
- Motor: converts flow to motion
- Valve: controls flow
- Filter: removes contamination
- ...

I'll help you:
✓ Structure with headings (H2, H3)
✓ Format lists and emphasis
✓ Suggest content blocks (alerts, columns)
✓ Add images/diagrams (if you have them)
✓ Validate against markdown standards
```

### Lecture Template

```markdown
---
title: [Objective Title]
docType: scorm  # or: print, revealjs
css: ../../../style-rau-base/rau-scorm.css
skill:
  id: SKL12345
  revisionDate: 2026-08
  classification: Public
---

# [Objective Title]

## Introduction

[2-3 sentences about what this objective covers]

## Main Concept 1

[Content here]

### Subpoint A
[Details]

### Subpoint B
[Details]

## Main Concept 2

[Content here]

## Summary

[Recap key learning points]
```

---

### Step 4: Knowledge Checks

After each lecture section, embed checks:

```markdown
## Main Concept 1

[Lecture content]

### Knowledge Check

What is the primary function of a [component]?

A) [Incorrect - why]
B) [Correct - explanation]
C) [Incorrect - why]
D) [Incorrect - why]

[Explanation: This is correct because...]

---
```

---

### Step 5: Labs (Standalone Objectives)

If objective is standalone, create a lab:

```markdown
# Lab: [Objective Title]

## Objective
Learners will [outcome behavior] by completing this lab.

## Time Required
[30 minutes | 1 hour | 2 hours]

## Equipment/Materials
- [Item 1]
- [Item 2]
- [Item 3]

## Procedure

### Step 1: [Title]
[Instructions with rationale]

### Step 2: [Title]
[Instructions]

## Success Criteria
Learner completes lab successfully when:
☐ [Criteria 1]
☐ [Criteria 2]
☐ [Criteria 3]

## Troubleshooting
**Problem**: [Common issue]
**Solution**: [How to fix]
```

---

### Step 6: Quizzes

Aggregate outcome-level quizzes:

```markdown
# Quiz: [Outcome Title]

## Instructions
This quiz assesses [outcome statement].
- [Number] questions
- [Pass percentage]%
- [Time limit] minutes

## Questions

### Question 1
[Question text]

A) [Option]
B) [Option]
C) [Option]
D) [Option]

**Correct answer**: [B]  
**Explanation**: [Why this is correct]

### Question 2
...
```

---

### Step 7: YAML Frontmatter

I'll generate complete YAML:

```yaml
---
title: Analyze Hydraulic System Components
docType: scorm  # or: print, revealjs
css: ../../../style-rau-base/rau-scorm.css
skill:
  id: SKL12345
  revisionDate: 2026-08
  classification: Public
varsLocal:
  skillName: Hydraulic Systems
  estimatedTime: 3 hours
---
```

**Required for every file:**
- `title`: File title
- `docType`: Publication type (scorm, print, revealjs)
- `css`: Path to stylesheet
- `skill.id`: Skill identifier
- `skill.revisionDate`: YYYY-MM format
- `skill.classification`: Public, Internal, Confidential

---

## Standards & Validation

I'll validate against:

### ✅ Markdown Standards
- Proper heading hierarchy (H1→H2→H3)
- Consistent list formatting
- Descriptive link text
- Alt text on images
- Code blocks with language specified

### ✅ LeGIT YAML Standards
- Valid docType (scorm, print, revealjs)
- Correct CSS path
- Complete skill metadata
- Proper date format (YYYY-MM)

### ✅ Content Block Standards
- Correct syntax (fenced divs)
- Valid block types
- Attribute formatting
- Media file references

### ✅ Content Quality
- Semantic structure
- Plain-text readability
- Clear emphasis (bold, italic)
- No ALL CAPS emphasis
- Relative image paths

---

## Content Blocks

Available blocks to use:

### Alerts (Any modality)
```markdown
::: {.rau-alert .warning}
Important safety information
:::
```

Types: `.reference`, `.warning`, `.attention`, `.important`, `.tip`

### Columns (for side-by-side content)
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

### For SCORM-Only
- `rau-accordion`: Expandable tabs
- `rau-quiz`: Interactive quizzes
- `rau-steps`: Numbered procedures
- `rau-image-hotspot`: Clickable images

### For Print
- `rau-page-break`: Start new page
- `important`: Important box

### For Presentations
- `rau-slide-*`: Slide-specific formatting
- `::: notes`: Speaker notes
- `::: script`: Recorded narration

---

## Real Example: From Design to Files

### Input (Design JSON)

```json
{
  "skill": {
    "name": "Configure CompactLogix Controllers",
    "audience": "technicians"
  },
  "outcomes": [
    {
      "id": "outcome-1",
      "title": "Configure Controller Parameters",
      "objectives": [
        {
          "id": "obj-1",
          "title": "Identify controller types",
          "standalone": false
        }
      ]
    }
  ]
}
```

### Output (Authored Files)

**File**: `skills/configure-compactlogix/configure-controller-parameters/objective-01/lecture.md`

```markdown
---
title: Identify Controller Types
docType: scorm
css: ../../../../../style-rau-base/rau-scorm.css
skill:
  id: SKL12345
  revisionDate: 2026-08
  classification: Public
---

# Identify Controller Types

## Overview

CompactLogix controllers come in different configurations...

## Controller Types

### Type L18
- 18 I/O points
- Ethernet port
- 16 MB memory

### Type L19
- 19 I/O points
- Dual Ethernet
- 32 MB memory

## Knowledge Check

Which controller type has dual Ethernet?

A) L18
B) L19  ✓
C) L20
D) L21

**Explanation**: The L19 includes dual Ethernet ports for redundancy...
```

---

## Workflow Options

### Option A: Guided Step-by-Step
I walk you through each file, one at a time.  
**Best for**: First-time authors, complex content

### Option B: Generate All Files at Once
I scaffold all files with templates, you fill in content.  
**Best for**: Experienced authors, rapid development

### Option C: Focus on One Outcome
Start with one outcome, complete it fully, then move to next.  
**Best for**: Large skills with many outcomes

---

## Ready to Start?

To begin, share:

```
1. Your design (JSON or outline)
2. Starting point (first outcome/objective)
3. Your content outline or SME input
4. Any files/images you want to include
```

Then I'll guide you through authoring, validating, and building your complete training content.

---

## Resources

- **Markdown Standards**: See `.claude/rules/legit-markdown-standards.md`
- **YAML Requirements**: See `.claude/rules/legit-yaml.md`
- **Content Blocks**: See `.claude/rules/legit-blocks.md`
- **Presentations**: See `.claude/rules/legit-presentations.md`
- **Best Practices**: See `docs/development/best-practices.md`

**Next Step**: After development is complete, run build to create:
- PDFs (for print modality)
- HTML presentations (for classroom)
- SCORM modules (for e-learning)
- Videos (if presentation-video modality)
