# File Mapping Guide: The Deliverable File Structure

This guide is the **structural contract** for LeGIT deliverables. It specifies the folder layout, file names, and content requirements that deliverables must follow.

**Who uses this guide**:

- **The content database** generates this structure from a validated design (design [Step 9](./content-design-process.md#step-9-load-into-content-database)). SMEs do not hand-create it.
- **SMEs** use it to navigate the generated structure and understand what belongs in each file.
- **Reviewers** use it to confirm a generated structure is correct during design [Step 8](./content-design-process.md#step-8-validate--refine-design).

**Key Principle**: Content is authored at the **objective level** for modularity and reuse, then packaged at the **outcome level** for delivery.

For which deliverables a given design requires, see [Content Design Process Step 7](./content-design-process.md#step-7-define-deliverables). This guide covers where they go and what goes in them.

## File Naming Convention

```text
skills/[skill-name]/
├── [outcome-01-title]/                  (e.g., "analyze-hydraulic-components")
│   ├── objective-01/
│   │   ├── lecture.md                   (Core instruction)
│   │   ├── knowledge-check.md           (Embedded checks within lecture)
│   │   ├── quiz-questions.md            (Questions for the outcome quiz pool)
│   │   ├── lab.md                       (Only if objective is standalone)
│   │   └── media/                       (Objective-specific images, diagrams)
│   │
│   ├── objective-02/
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── quiz-questions.md
│   │   └── media/
│   │
│   ├── outcome-01-lecture.md            (Aggregates objective lectures via !include)
│   ├── outcome-01-lab.md                (Outcome-level interactive activity)
│   ├── outcome-01-quiz.md               (Outcome-level assessment)
│   ├── outcome-01-presentation.md       (If classroom or blended)
│   ├── outcome-01-practical.md          (If assessment is hands-on)
│   └── outcome-01-handout.md            (If blended, or a learner reference is planned)
│
├── [outcome-02-title]/                  (e.g., "interpret-pressure-readings")
│   └── [same structure as outcome-01]
│
├── skill-01-practical.md                (Optional skill-level capstone)
├── assets/                              (Supporting assets, see below)
└── media/                               (Shared images, videos across outcomes)
```

**Folder Naming Convention for Outcomes**:

- Use the outcome title in kebab-case (lowercase, hyphens)
- Example: "Analyze Hydraulic Components" → `analyze-hydraulic-components/`
- This makes the structure readable and searchable, and it appears in build outputs

## File Creation Rules

### Objective Level

| Deliverable | File Path | When to Create |
| --- | --- | --- |
| Objective lecture | `[outcome-title]/objective-##/lecture.md` | **Always** |
| Knowledge check | `[outcome-title]/objective-##/knowledge-check.md` | **Always** |
| Quiz questions | `[outcome-title]/objective-##/quiz-questions.md` | **Always** (feeds outcome quiz) |
| Objective lab | `[outcome-title]/objective-##/lab.md` | **Only if standalone** |
| Objective media | `[outcome-title]/objective-##/media/` | As needed |

**Why `quiz-questions.md` is always created but `lab.md` is not**: quiz questions are authored per objective so the outcome quiz can draw a pool traceable to each objective. Labs live at the outcome level because a non-standalone objective shares its interactive activity with the rest of the outcome. A standalone objective must be independently completable, so it needs its own lab.

### Outcome Level

| Deliverable | File Path | When to Create |
| --- | --- | --- |
| Outcome lecture | `[outcome-title]/outcome-##-lecture.md` | **Always** |
| Outcome lab | `[outcome-title]/outcome-##-lab.md` | When the outcome has non-standalone objectives (the usual case) |
| Outcome quiz | `[outcome-title]/outcome-##-quiz.md` | **Always** |
| Outcome presentation | `[outcome-title]/outcome-##-presentation.md` | Classroom or blended delivery |
| Outcome practical | `[outcome-title]/outcome-##-practical.md` | When assessment is hands-on rather than quiz-based |
| Outcome handout | `[outcome-title]/outcome-##-handout.md` | Blended delivery, or when a learner reference is planned |

### Skill Level

| Deliverable | File Path | When to Create |
| --- | --- | --- |
| Shared media | `media/` | As needed across outcomes |
| Supporting assets | `assets/` | When the skill requires VMs, project files, etc. |
| Skill capstone practical | `skill-##-practical.md` | **Optional**: only for a genuine cross-outcome capstone |

**Note on scope**: presentations and practicals live at the **outcome** level, not the skill level. Coverage is validated per outcome ([Rule 3](/.claude/rules/content-design-validation.md#rule-3-coverage-completeness-outcome-level)), so an outcome-level practical traces to the outcome it assesses. A skill-level practical is an optional extra for a capstone that deliberately spans multiple outcomes; it does not satisfy any outcome's assessment coverage on its own.

## Supporting Assets

Some deliverables are not authored or built by LeGIT but are still required for the skill to be deliverable. These are planned in design [Step 7 Part B](./content-design-process.md#part-b-non-legit-deliverables) and tracked alongside the markdown.

### Where They Live

```text
skills/[skill-name]/
└── assets/
    ├── vm/                       (VM images or links to their storage location)
    ├── project-files/            (Studio 5000 .ACD files, config exports)
    ├── lab-files/
    │   ├── objective-01-start/   (Starting state for a sequential lab)
    │   └── objective-01-finish/  (Expected end state, for verification)
    └── README.md                 (Inventory: what each asset is, who owns it)
```

### Reference Rules

- **Large binaries do not belong in Git.** Store VMs and large project files in their designated location (SharePoint, artifact storage, OnCourse) and put a pointer in `assets/README.md` with the path, version, and owner.
- **Small project files and lab start/finish files** can live in the repo under `assets/`.
- **Reference them from labs by relative path** where they are in-repo:

  ```markdown
  Download the starting project: [objective-01 start files](../assets/lab-files/objective-01-start/)
  ```

- **`assets/README.md` is required** whenever `assets/` exists. It is the inventory reviewers check during design Step 8 and the list exported to DevOps.

### What to Record Per Asset

| Field | Why |
| --- | --- |
| What it is | So a reviewer can tell whether it is complete |
| Which deliverable depends on it | A lab is not deliverable without its VM |
| Owner | Supporting assets often need someone outside the authoring team |
| Location and version | Especially for anything stored outside Git |

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
- No nested includes (included files cannot themselves have includes)

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
- **Any supporting-asset dependencies** (VM, project file, equipment) called out up front
- Step-by-step guided walkthrough with screenshots
- Learning checkpoints
- Start files and/or finish files (if sequential)
- Troubleshooting tips
- Reflection questions
- ~1,500-2,500 words

### Knowledge Check Files

**Purpose**: Embedded, ungraded knowledge checks within objective lectures (1-2 per lecture)

**Must Include**:

- 1-2 simple check questions
- Answers and brief explanations
- Direct connection to lecture concepts
- Minimal cognitive load (quick pause point)
- ~50-100 words

### Quiz Question Files

**Purpose**: Pool of graded assessment questions that feed outcome-level quizzes

**Must Include**:

- 3-5 questions per objective
- Mix of question types (multiple choice, short answer, scenario-based)
- Clear connection to the objective
- Correct answer(s) and scoring rubric
- Rationale explaining why the correct answer is right
- Difficulty level indicator (basic/intermediate/advanced)

### Outcome Quiz Files

**Must Include**:

- Clear outcome(s) being assessed
- Questions drawn from the objective `quiz-questions.md` pools
- Coverage of every objective in the outcome
- Quiz configuration (`enableRetry`, `passPercent`)
- Correct answers and scoring rubric

### Practical Files

**Must Include**:

- The outcome being assessed
- Scenario or task description
- Required equipment or environment (including supporting-asset dependencies)
- Observable performance criteria
- Scoring rubric with pass thresholds
- Instructor guidance for evaluation

## Tips for Success

1. Confirm the design is validated before expecting the structure to be generated
2. Keep objective media in `objective-##/media/` for better containment
3. Use relative paths consistently in include directives: `objective-##/lecture.md`
4. Provide start/finish files for sequential labs under `assets/lab-files/`
5. Keep `assets/README.md` current; it is what gets exported to DevOps
6. Use a consistent YAML `revisionDate` format (`YYYY-MM`)
7. Test includes before pushing

## Related Documents

- [Content Design Process](./content-design-process.md): which deliverables a design requires
- [Terminology Glossary](../terminology-glossary.md): what "deliverable" covers
- [Content Design Validation](/.claude/rules/content-design-validation.md): Rule 6, deliverable completeness

