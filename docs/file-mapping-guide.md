# File Mapping Guide: From Design to Markdown

This guide shows how your content design translates into markdown files SMEs will author in LeGIT.

**Key Principle**: Content is authored at the **objective level** for modularity and reuse, then packaged at the **outcome level** for delivery.

## File Naming Convention

Follow this structure:

```
skills/[skill-name]/
├── [outcome-01]/
│   ├── objective-01-lecture.md      (Always created)
│   ├── objective-02-lecture.md      (Always created)
│   ├── outcome-01-lecture.md        (Always created; uses !include)
│   ├── objective-01-lab.md          (Only if objective 1 is standalone)
│   └── outcome-01-quiz.md           (Outcome-level assessment)
│
├── skill-01-practical.md            (Skill-level capstone)
├── skill-01-presentation.md         (If classroom required)
└── media/                            (Images, videos, etc.)
```

## File Creation Rules

| Content Type | File Name | When to Create |
|--------------|-----------|---|
| Objective lecture | `objective-##-lecture.md` | Always |
| Outcome lecture | `outcome-##-lecture.md` | Always |
| Objective lab | `objective-##-lab.md` | Only if standalone |
| Objective quiz | `objective-##-quiz.md` | Only if standalone |
| Outcome quiz | `outcome-##-quiz.md` | For assessments |
| Presentation | `skill-##-presentation.md` | If classroom required |
| Practical/Project | `skill-##-practical.md` | For skill-level capstone |

## The Include Directive Pattern

Use `!include()` to aggregate objective lectures:

```markdown
---
title: "Outcome 01: Analyze Hydraulic Systems"
---

# Outcome Overview
[Introduction]

## Part 1: Component Identification
!include(objective-01-lecture.md)

## Part 2: Component Function
!include(objective-02-lecture.md)

## Outcome Synthesis
[How parts fit together]
```

**Include rules**:
- One include per line
- Use relative paths: `!include(objective-01-lecture.md)`
- No nested includes (included files cannot have includes)

## Content Requirements

### Objective Lecture Files
**Must Include**:
- YAML frontmatter (title, docType, skill metadata)
- Learning objective statement
- Explanation of key concepts and terminology
- Real-world examples or use cases
- Demonstrations or step-by-step walkthroughs
- Embedded knowledge checks (1-2 ungraded questions)
- Visuals (diagrams, screenshots, videos)
- ~800-1,200 words per objective

### Outcome Lecture Files
**Must Include**:
- YAML frontmatter
- Clear outcome statement
- All objective lectures via include directives
- Connecting paragraphs between sections
- Outcome-level synthesis
- Key takeaways or summary
- ~2,500-4,000 words

### Lab Files
**Must Include**:
- Clear objective(s) being practiced
- Prerequisites and setup instructions
- Step-by-step guided walkthrough with screenshots
- Learning checkpoints
- Start files and/or finish files (if sequential)
- Troubleshooting tips
- Reflection questions
- ~1,500-2,500 words

### Assessment Files
**Must Include**:
- Clear outcome(s) being assessed
- Mix of question types
- 5-8 questions per objective assessment
- Correct answers and scoring rubric
- Rationale for each correct answer

## Tips for Success

1. Create objectives before files
2. Use relative paths consistently
3. Provide start/finish files for sequential labs
4. Keep media in `media/` folder
5. Use consistent YAML `revisionDate` format
6. Test includes before pushing

For more details, see Section 4: File Mapping in CLAUDE.md
