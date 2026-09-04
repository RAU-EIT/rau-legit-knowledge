---
title: "LeGIT Knowledge Base - Reorganized Structure"
description: "RAU LeGIT Knowledge separated into Content Design and Content Development sections"
version: "2.0.0"
status: "Draft for review"
---

# RAU LeGIT Knowledge Base

Complete guide for **designing** and **developing** learning content in RAU LeGIT system.

---

## Choose Your Path

### 👨‍🏫 CONTENT DESIGN TRACK
**For**: Instructional Designers, Product Managers, Subject Matter Experts  
**Goal**: Turn skills + audiences into structured learning designs  
**Input**: Skills list, target roles, stakeholder requirements  
**Output**: Learning outcomes, objectives, delivery strategy, offerings, publications, activities, deliverables → Content Database

**Start Here**: [CONTENT DESIGN GUIDE](#content-design-guide)

---

### 📝 CONTENT DEVELOPMENT TRACK
**For**: Subject Matter Experts, Technical Writers, Content Developers  
**Goal**: Turn knowledge into formal LeGIT training content  
**Input**: Subject matter expertise (PPTs, docs, manuals, experiences)  
**Output**: Markdown files ready to publish (lectures, labs, quizzes, knowledge checks)

**Start Here**: [CONTENT DEVELOPMENT GUIDE](#content-development-guide)

---

# CONTENT DESIGN GUIDE

*Design your learning, then build it.*

## What Is Content Design?

Content design answers: **"What should learners be able to do?"** and **"How will we know they can do it?"**

Design happens FIRST, independent of how content will be delivered. This ensures your learning intent is clear before development begins.

## Design Derives Top-Down

Each decision determines the next:

```text
Delivery Strategy → Offering → Publication → Activity → Deliverable
    (Step 3)        (Step 4)    (Step 5)     (Step 6)    (Step 7)
```

The build system runs this chain in reverse: deliverables build into publications, which are packaged into offerings. Same chain, opposite directions. See [The Two Directions](../terminology-glossary.md#the-two-directions).

**Terminology**: [`docs/terminology-glossary.md`](../terminology-glossary.md) is the source of truth for every term used below.

## The Design Process (9 Steps)

**Complete Guide**: [`docs/design/content-design-process.md`](../design/content-design-process.md)

### Step 1: Define Learning Outcomes (ABCD)
**What**: Create 1-5 measurable outcomes for the skill  
**Use**: ABCD Method (Audience, Behavior, Condition, Degree)  
**Output**: Clear outcome titles + ABCD statements

**Example**:
- **Title**: "Analyze Hydraulic System Components"
- **ABCD**: "Given a schematic, the field technician will analyze components and explain function with 90% accuracy"

### Step 2: Define Learning Objectives
**What**: Break each outcome into 2-5 step-level objectives  
**Why**: 
- 1 objective = outcome too small
- 2-5 objectives = coherent, manageable
- 6-10 objectives = consider splitting outcome
- 10+ objectives = definitely split into multiple outcomes

**Designation**: Mark each as standalone or non-standalone (non-standalone is default for 90% of objectives)

### Step 3: Determine Delivery Strategy

**What**: Choose how the skill will be taught, the first link in the derivation chain
**Options**: E-learning, Classroom (ILT), Blended
**Decision Framework**: [Six-factor framework in the design process guide](../design/content-design-process.md#step-3-determine-delivery-strategy-rau-recommendation)
**Required**: Record the rationale, not just the label

### Step 4: Define Offerings

**What**: Define what students actually enroll in, purchase, or download
**Scope**: One or more per audience/role; the same design often serves roles differently

| Delivery Strategy | Typical Offerings |
| --- | --- |
| E-Learning | A self-paced online course enrolled in via the LMS |
| Classroom | An instructor-led course with a scheduled session |
| Blended | A self-paced course **plus** a classroom or regional practicum |

### Step 5: Define Publications

**What**: Determine what the build system must produce to support those offerings
**Each records**: name, audience/role, scope, publication type (`docType`), offering(s) it feeds
**Scoped by**: the design hierarchy: per skill, per outcome, or per standalone objective

| Delivery Strategy | Required Publications |
| --- | --- |
| E-Learning | SCORM module (`scorm1.2`) |
| Classroom | Presentation (`revealjs`/`pptx`), lab manual (`print`), practical (`print`) |
| Blended | All of the above, plus a learner handout (`print`) |

### Step 6: Determine Activities

**What**: Derive the activities the publications require, then validate coverage

**Coverage Types**:

1. **Passive** (Lecture) - Learner gains exposure
2. **Interactive** (Lab) - Learner applies with guidance
3. **Assessment** (Quiz/Practical) - Learner demonstrates mastery

**Validation**: Check at OUTCOME level (aggregated across objectives)
**Exception**: Standalone objectives must have P+I+A individually

### Step 7: Define Deliverables

**What**: Everything the SME must produce, the superset of activities plus supporting assets

```text
Deliverables = every Activity + supporting assets
```

**Content deliverables**: lectures, knowledge checks, quiz questions, labs, quizzes, presentations, handouts, practicals
**Supporting assets**: VMs and test environments, project files, lab start/finish files, externally produced media

Supporting assets are still tracked and exported to DevOps. A lab that depends on a VM is not deliverable without it.

### Step 8: Validate & Refine Design

**What**: Review the generated publications, activities, and deliverables; add, change, or remove
**How**: Validation checklist, or ask Claude Code "Validate my content design"

### Step 9: Load into Content Database

**What**: Load the validated design into the content database
**System of Record**: Content Database

**The database then**:

- Creates the authoring project structure (in LeGIT: folder structure + file shells with frontmatter)
- Exports the deliverables list to DevOps for work tracking

**Fields captured**:

- Skills list
- Outcomes (ABCD)
- Objectives with standalone flags
- Delivery strategy + rationale
- Offerings (per audience/role)
- Publications (audience, scope, type)
- Activities matrix + coverage validation
- Deliverables, including supporting assets

> **Transitional**: until the content database is live, the **CDD Workbook (Excel) remains the interim system of record**, and the design JSON carries the same fields.

---

## Design Validation Rules

**Rule 1: ABCD Outcome Completeness**
- Outcome must have title + all 4 ABCD elements

**Rule 2: Objective-to-Outcome Alignment**
- Every objective traces to an outcome
- No orphaned objectives

**Rule 3: Coverage Completeness (Outcome Level)**
- Every outcome has P+I+A (aggregated across objectives)
- Non-standalone objectives roll into outcome coverage

**Rule 4: Standalone Objective Designation**
- Standalone objectives have P+I+A individually
- Non-standalone objectives don't need individual coverage (only outcome-level)

**Rule 5: Delivery Strategy & Deliverable Alignment**

Validates the whole derivation chain, not just the strategy:

- A delivery strategy is selected, with a recorded rationale
- Every audience/role from intake is served by at least one offering
- Every offering is supported by at least one publication, and every publication feeds an offering
- Publication scope is consistent with the design hierarchy
- Every activity maps to a deliverable; supporting-asset dependencies are captured

**Rule 6: Deliverable File Completeness**

- Outcome folders use outcome titles (kebab-case)
- Objective folders created for each objective
- Every objective has `lecture.md` + `knowledge-check.md` + `quiz-questions.md`
- `lab.md` at objective level **only** for standalone objectives
- Outcome-level lab present when the outcome has non-standalone objectives
- Presentations and practicals are outcome level, not skill level

**Rule 7: Publication-to-Activity Derivation**: placeholder, **not enforced yet** (mapping rules TBD)

**Full Details**: [`.claude/rules/content-design-validation.md`](../../.claude/rules/content-design-validation.md)

---

## Key Concepts

### Outcomes vs. Objectives

| Outcome | Objective |
|---------|-----------|
| Measurable skill competency | Step toward outcome competency |
| 1-5 per skill | 2-5 per outcome |
| ABCD format | Observable behavior |
| Assessed as whole | Aggregated into outcome assessment |

### Standalone vs. Non-Standalone Objectives

**Non-Standalone (Default - 90% of cases)**:
- Lectures authored at objective level
- Labs/quizzes at outcome level
- Learners take whole outcome together

**Standalone (Exception)**:
- Lectures, labs, quizzes all at objective level
- Completely independent
- Separate SCORM packages possible

---

## Design Resources

| Resource | Purpose |
|----------|---------|
| [`docs/design/content-design-process.md`](../design/content-design-process.md) | Step-by-step design guide |
| [`.claude/rules/content-design-validation.md`](../../.claude/rules/content-design-validation.md) | 6 validation rules |
| [`docs/legit-fundamentals.md`](../development/legit-fundamentals.md) | LeGIT system overview |

---

# CONTENT DEVELOPMENT GUIDE

*Take your design and create the content.*

## What Is Content Development?

Content development answers: **"How do we teach this?"** and **"What does it look like in LeGIT?"**

Development takes the design (outcomes, objectives, activities) and creates actual markdown files formatted in LeGIT standards, ready to build and publish.

## The Development Process

### Prerequisites
- ✅ Design completed (outcomes, objectives, modalities defined)
- ✅ Folder structure scaffolded (outcome folders, objective folders)
- ✅ File structure ready (lecture.md, lab.md, quiz-questions.md, etc.)

### Content Types & Standards

**Lectures** (`objective-##/lecture.md`):
- Explain concepts and terminology
- Provide real-world examples
- Include demonstrations or walkthroughs
- Embedded knowledge checks (1-2 ungraded)
- ~800-1,200 words per objective
- Must reference `docs/legit-markdown-standards.md`

**Knowledge Checks** (`objective-##/knowledge-check.md`):
- 1-2 simple ungraded questions per objective
- Check understanding before moving on
- Embedded within or after lecture
- Brief explanations for answers
- ~50-100 words total

**Labs** (`objective-##/lab.md`):
- Hands-on or guided practice
- Step-by-step with screenshots
- Learning checkpoints
- Start/finish files if sequential
- Troubleshooting tips
- Reflection questions
- ~1,500-2,500 words

**Quiz Questions** (`objective-##/quiz-questions.md`):
- 3-5 graded questions per objective
- Mix of question types (multiple choice, short answer, scenario)
- Correct answers + scoring rubric
- Rationale for correct answers
- Difficulty level indicator
- Feeds into outcome-level quiz

**Outcome Lectures** (`outcome-##-lecture.md`):
- Aggregates all objective lectures via `!include` directives
- Adds connecting paragraphs between sections
- Outcome-level synthesis and key takeaways
- ~2,500-4,000 words

**Outcome Quizzes** (`outcome-##-quiz.md`):
- Aggregates quiz questions from all objectives
- Outcome-level assessment
- Grading rubric for the whole outcome

---

## LeGIT Technical Standards

### Markdown Standards
[`docs/legit-markdown-standards.md`](../development/markdown-standards.md)

**Key Rules**:
- Proper heading hierarchy (H1 → H2 → H3, no skipping)
- Bold for emphasis, not ALL CAPS
- Relative paths for images/links
- Descriptive alt text for all images
- Semantic markdown (use proper formatting, not inline HTML)
- DRY principle (use variables, includes, don't repeat)

### YAML Frontmatter
[`docs/legit-yaml.md`](../development/yaml-guide.md)

**Required in every file**:
```yaml
---
title: "Objective 01: [Name from design]"
docType: scorm          # or print, revealjs
css: ../style-rau-base/rau-scorm.css
skill:
  id: SKL12345
  revisionDate: 2026-08
  classification: Public
---
```

### Content Blocks
[`docs/content-blocks-reference.md`](../development/content-blocks-reference.md)

**Available Blocks** (by output type):

**All Outputs**:
- Alerts (Reference, Warning, Attention, Important, Tip)
- Column layouts (2-col, 3-col)
- Images (with captions, alt text, sizing)

**Print Outputs**:
- Page breaks
- Input fields (fillable forms)

**SCORM/E-Learning**:
- Accordion (expandable sections)
- Hero (banner/intro)
- Image hotspots (clickable regions)
- Flip cards (interactive cards)
- Steps (procedural sequences)
- Quiz blocks (interactive assessment)

**Presentations**:
- Slide layouts (title, content, two-column)
- Speaker notes
- Scripts (for video recording)

**Use Snippets**: Press `Ctrl+Space`, type `rau`, select block

### Presentation Standards
[`docs/legit-presentations.md`](../development/presentations.md)

**Key Rules**:
- H2 = main slides (horizontal transitions)
- H3 = vertical slides (sub-slides)
- Max 3-4 bullet points per slide
- Include speaker notes (`::: notes` blocks)
- Include scripts if recording video (`::: script` blocks)
- Use RevealJS CSS

---

## File Organization

**Folder Structure**:
```
skills/[skill-name]/
├── [outcome-title]/                          (e.g., analyze-hydraulic-components)
│   ├── objective-01/
│   │   ├── lecture.md                        (always)
│   │   ├── knowledge-check.md                (always)
│   │   ├── quiz-questions.md                 (always - feeds outcome quiz pool)
│   │   ├── lab.md                            (only if standalone)
│   │   └── media/                            (objective-specific images)
│   ├── objective-02/
│   │   └── [same structure]
│   ├── outcome-01-lecture.md                 (aggregates objectives via !include)
│   ├── outcome-01-lab.md                     (for non-standalone objectives)
│   └── outcome-01-quiz.md
├── [outcome-title-2]/
│   └── [same structure]
├── skill-01-presentation.md                  (if classroom delivery)
├── skill-01-practical.md                     (if capstone needed)
└── media/                                    (shared media across outcomes)
```

---

## Development Workflow

### 1. Start with Design
- Review outcomes and objectives from design phase
- Understand audience and modalities
- Know what activities are needed

### 2. Author Objective Lectures
- One lecture per objective
- 800-1,200 words each
- Include examples, demos, embedded knowledge checks
- Use LeGIT markdown standards

### 3. Create Knowledge Checks
- 1-2 ungraded questions per objective
- Brief explanations
- ~50-100 words

### 4. Create Labs (if needed)
- Guided walkthroughs
- Step-by-step with screenshots
- Troubleshooting tips
- Reflection questions

### 5. Create Quiz Questions
- 3-5 questions per objective
- Multiple question types
- Correct answers + rationale
- Difficulty levels

### 6. Aggregate into Outcome Files
- Create outcome lecture using `!include(objective-##/lecture.md)`
- Create outcome quiz from aggregated questions
- Add outcome-level synthesis

### 7. Validate and Build
- Check markdown standards
- Validate YAML frontmatter
- Run build system
- Test outputs (PDF, SCORM, etc.)

---

## Development Resources

| Resource | Purpose |
|----------|---------|
| [`docs/legit-markdown-standards.md`](../development/markdown-standards.md) | Markdown rules and best practices |
| [`docs/legit-yaml.md`](../development/yaml-guide.md) | YAML frontmatter guide |
| [`docs/content-blocks-reference.md`](../development/content-blocks-reference.md) | Content blocks syntax and usage |
| [`docs/legit-presentations.md`](../development/presentations.md) | Presentation-specific rules |
| [`.claude/rules/legit-blocks.md`](../../.claude/rules/legit-blocks.md) | Content block validation rules |
| [`.claude/rules/legit-markdown-standards.md`](../../.claude/rules/legit-markdown-standards.md) | Markdown validation rules |

---

## When to Ask Claude Code

### For Design Track
- "Validate my content design" → Checks against 6 rules
- "Generate learning objectives" → Suggests 2-5 from outcome
- "Map to deliverables" → Determines what needs to be built

### For Development Track
- "Check my markdown" → Validates against standards
- "Suggest content blocks" → Recommends blocks for content type
- "Validate my YAML" → Checks frontmatter completeness
- "Help me rewrite this" → Assist with clarity and conciseness

---

## FAQ

**Q: Can I skip design and go straight to development?**  
A: No. Design defines what learners need to accomplish. Without it, you build content without clear intent.

**Q: How do I know if something is an outcome or objective?**  
A: Outcomes are measurable skill competencies (what learners must do). Objectives are steps toward that competency.

**Q: Can I have just 1 objective per outcome?**  
A: Not recommended. 1 objective suggests the outcome is too small. Expand scope or reconsider whether it's an outcome.

**Q: How many outcomes per skill?**  
A: At least 1. Complex skills can have 2-5+. Ensure they're all part of the same skill domain.

**Q: What's the difference between knowledge check and quiz?**  
A: Knowledge checks are ungraded, embedded in lecture, check understanding. Quizzes are graded, formal assessment.

---

## Document Version

**Version**: 2.0.0  
**Status**: Draft for reorganization  
**Created**: 2026-08-20  
