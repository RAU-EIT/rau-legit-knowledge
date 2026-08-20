# File Mapping Guide: From Design to Markdown

This guide shows how your content design translates into markdown files SMEs will author in LeGIT.

**Key Principle**: Content is authored at the **objective level** for modularity and reuse, then packaged at the **outcome level** for delivery.

## File Naming Convention

Follow this structure:

```
skills/[skill-name]/
├── [outcome-01-title]/                    (e.g., "analyze-hydraulic-components")
│   ├── objective-01/
│   │   ├── lecture.md               (Core instruction)
│   │   ├── knowledge-check.md       (Embedded checks within lecture)
│   │   ├── lab.md                   (Only if standalone)
│   │   ├── quiz-questions.md         (Assessment questions for quiz pool)
│   │   └── media/                   (Images, diagrams, etc.)
│   │
│   ├── objective-02/
│   │   ├── lecture.md               (Core instruction)
│   │   ├── knowledge-check.md       (Embedded checks within lecture)
│   │   ├── quiz-questions.md         (Assessment questions for quiz pool)
│   │   └── media/                   (Images, diagrams, etc.)
│   │
│   ├── outcome-01-lecture.md        (Aggregates objective lectures via !include)
│   └── outcome-01-quiz.md           (Outcome-level assessment)
│
├── [outcome-02-title]/                    (e.g., "interpret-pressure-readings")
│   └── [same structure as outcome-01]
│
├── skill-01-practical.md            (Skill-level capstone)
├── skill-01-presentation.md         (If classroom required)
└── media/                            (Shared images, videos, etc.)
```

**Folder Naming Convention for Outcomes**:
- Use outcome title in kebab-case (lowercase, hyphens)
- Example: "Analyze Hydraulic Components" → `analyze-hydraulic-components/`
- This makes the folder structure more readable and searchable

## File Creation Rules

| Content Type | File Path | When to Create |
|--------------|-----------|---|
| Objective lecture | `[outcome-title]/objective-##/lecture.md` | Always |
| Knowledge check | `[outcome-title]/objective-##/knowledge-check.md` | Always |
| Objective lab | `[outcome-title]/objective-##/lab.md` | Only if standalone |
| Quiz questions | `[outcome-title]/objective-##/quiz-questions.md` | Always (feeds outcome quiz) |
| Objective media | `[outcome-title]/objective-##/media/` | As needed |
| Outcome lecture | `[outcome-title]/outcome-##-lecture.md` | Always |
| Outcome quiz | `[outcome-title]/outcome-##-quiz.md` | For assessments |
| Presentation | `skill-##-presentation.md` | If classroom required |
| Practical/Project | `skill-##-practical.md` | For skill-level capstone |

**Note on outcome folders**: Replace `[outcome-title]` with the outcome title in kebab-case (e.g., `analyze-hydraulic-components`). Each outcome gets its own folder named after its title for clarity and readability.

## The Include Directive Pattern

Use `!include()` to aggregate objective lectures from their subfolders:

```markdown
---
title: "Outcome 01: Analyze Hydraulic Systems"
---

# Outcome Overview
[Introduction]

## Part 1: Component Identification
!include(objective-01/lecture.md)

## Part 2: Component Function
!include(objective-02/lecture.md)

## Outcome Synthesis
[How parts fit together]
```

**Include rules**:
- One include per line
- Use relative paths from the outcome folder: `!include(objective-##/lecture.md)`
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

### Knowledge Check Files

**Purpose**: Embedded, ungraded knowledge checks within objective lectures (1-2 per lecture)

**Must Include**:

- 1-2 simple check questions
- Answers and brief explanations
- Direct connection to lecture concepts
- Minimal cognitive load (quick pause point)
- ~50-100 words

### Quiz Question Files

**Purpose**: Pool of graded assessment questions for outcome-level quizzes

**Must Include**:

- 3-5 questions per objective
- Mix of question types (multiple choice, short answer, scenario-based)
- Clear connection to objective learning outcome
- Correct answer(s) and scoring rubric
- Rationale explaining why correct answer is right
- Difficulty level indicator (basic/intermediate/advanced)

## Tips for Success

1. Create objectives before files
2. Create an objective subfolder for each objective under its outcome
3. Keep objective media in `objective-##/media/` for better containment
4. Use relative paths consistently in include directives: `objective-##/lecture.md`
5. Provide start/finish files for sequential labs in the objective folder
6. Use consistent YAML `revisionDate` format
7. Test includes before pushing

For more details, see Section 4: File Mapping in CLAUDE.md
