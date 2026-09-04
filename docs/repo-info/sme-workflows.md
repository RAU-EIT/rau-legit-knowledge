# SME Workflows for LeGIT Content Development

This guide describes the standard workflows for subject matter experts (SMEs) developing content in LeGIT.

## Content Development Lifecycle

LeGIT content development follows a design-first workflow:

1. **Design Phase**: SME completes instructional design
2. **Development Phase**: SME writes and structures content
3. **Review Phase**: Content review and validation
4. **Publication Phase**: Build and deliver to end users

## Design Phase Workflow

The design phase establishes learning outcomes, objectives, and everything they imply **before** any content is written.

Design **derives top-down**: each decision determines the next:

```text
Delivery Strategy → Offering → Publication → Activity → Deliverable
```

The build system runs this chain in reverse. See [The Two Directions](../terminology-glossary.md#the-two-directions).

**Output:** A validated design, loaded into the content database.

### Step 1: Define ABCD Outcomes

Using the ABCD method, define measurable learning outcomes:

- **Audience**: Who will take this training
- **Behavior**: What they will do (measurable verb)
- **Condition**: Under what circumstances
- **Degree**: How well (metric for success)

### Step 2: Design Objectives

Break each outcome into 2-5 specific learning objectives, and mark which are standalone.

Steps 1-2 together produce the **design hierarchy**, which scopes publications in Step 5.

### Step 3: Determine Delivery Strategy

Determine **how** this content will be taught:

- **E-Learning**: Self-paced, online, interactive delivery
- **Classroom (ILT)**: Instructor-led, hands-on labs delivery
- **Blended**: Mix of online and classroom delivery

Record the rationale. A strategy without one fails validation.

### Step 4: Define Offerings

Define what students actually enroll in, purchase, or download, one or more per audience/role.

| Delivery Strategy | Typical Offerings |
| --- | --- |
| E-Learning | A self-paced online course enrolled in via the LMS |
| Classroom | An instructor-led course with a scheduled session |
| Blended | A self-paced online course plus a classroom or regional practicum |

The same skill design often serves roles differently: a technician may get the blended offering while an engineer gets e-learning only.

### Step 5: Define Publications

Determine what the build system must produce to support those offerings.

| Delivery Strategy | Required Publications |
| --- | --- |
| E-Learning | SCORM module (`scorm1.2`) |
| Classroom | Presentation (`revealjs`/`pptx`) + lab manual (`print`) + practical (`print`) |
| Blended | All of the above, plus a learner handout (`print`) |

Each publication is tagged with its audience/role, scope, and publication type. Scope follows the design hierarchy: per skill, per outcome, or per standalone objective.

### Step 6: Determine Activities

Activities are derived from the publications. Every outcome must have:

- **Passive:** Lecture content
- **Interactive:** Lab/hands-on practice
- **Assessment:** Quiz or practical

### Step 7: Define Deliverables

Deliverables are the **superset**: every activity, plus everything else the SME must produce that LeGIT does not author.

| Delivery Strategy | Required LeGIT Deliverables |
| --- | --- |
| E-Learning | Lectures + knowledge checks + quiz questions + labs |
| Classroom | Lectures + presentations + lab manuals + practical assessments |
| Blended | All of the above, plus handouts |

**Plus supporting assets**: VMs and test environments, project files, lab start/finish files, externally produced media. These are still tracked and exported to DevOps: a lab that depends on a VM is not deliverable without it.

### Step 8: Validate & Refine Design

Review the generated publications, activities, and deliverables; add, change, or remove anything wrong. Then have the design reviewed by:

- Instructional designer (Aba Azeem)
- Subject matter expert (peer review)
- Stakeholder (management approval)

Ask Claude Code: **"Validate my content design"**

### Step 9: Load into Content Database

The content database creates the LeGIT project files and exports the deliverables to DevOps.

> **Transitional**: until the content database is live, the **CDD Workbook remains the interim system of record**. Document outcomes, objectives, delivery strategy, offerings, publications, coverage, and the deliverables list there.

## Development Phase Workflow

After the design is loaded into the content database, it creates the project structure and content is authored into those files.

### File Organization

The content database generates this structure; SMEs do not hand-create it.

```text
skills/[skill-name]/
├── [outcome-01-title]/                (e.g., "analyze-hydraulic-components")
│   ├── objective-01/                  (non-standalone)
│   │   ├── lecture.md                 (always)
│   │   ├── knowledge-check.md         (always)
│   │   ├── quiz-questions.md          (always - feeds outcome quiz pool)
│   │   └── media/
│   ├── objective-02/                  (standalone)
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── quiz-questions.md
│   │   ├── lab.md                     (only because it IS standalone)
│   │   └── media/
│   ├── outcome-01-lecture.md          (aggregates objective lectures)
│   ├── outcome-01-lab.md              (interactive activity for objective-01)
│   ├── outcome-01-quiz.md
│   ├── outcome-01-presentation.md     (classroom or blended)
│   └── outcome-01-handout.md          (blended)
├── [outcome-02-title]/                (repeat structure)
├── assets/                            (supporting assets + required README.md)
└── media/                             (shared images across outcomes)
```

**Key points:**

- Outcome folders use the outcome title in kebab-case (e.g., `analyze-hydraulic-components`)
- Each objective gets its own subfolder (`objective-01`, `objective-02`, etc.)
- `lecture.md`, `knowledge-check.md`, and `quiz-questions.md` exist for **every** objective
- `lab.md` at objective level **only** for standalone objectives; others share `outcome-##-lab.md`
- Presentations and practicals are **outcome** level, not skill level
- Objective lectures use `!include()` in outcome-level files
- Media can be at objective level (objective-specific) or skill level (shared)
- Supporting assets (VMs, project files) live in `assets/` with an inventory README

### Writing Objectives

Write at the **objective level** for reusability:

- Each objective file focuses on one learning objective
- Files are small (3-8 pages) for easier maintenance
- Content can be reused across multiple courses

### Composing at Outcome Level

For delivery, outcomes **aggregate** objectives using `!include()`. Paths are relative to the outcome folder and point into the objective subfolders:

```markdown
!include(objective-01/lecture.md)
!include(objective-02/lecture.md)
```

### Using Content Blocks

Use RAU markdown blocks to structure content:
- **Accordion**: Expand/collapse sections
- **Steps**: Procedure walkthrough
- **Quiz**: Knowledge assessment
- **Video**: Embed multimedia
- **Alerts**: Important notes

### Media Management

Store media files in a `media/` folder and use relative paths:

```markdown
![Network architecture](./media/network-diagram.svg)
```

## Review Phase Workflow

After drafting, content goes through review cycles.

### Self-Review Checklist

Before submitting for review:

- [ ] All outcomes have lecture + lab + quiz
- [ ] Every objective has lecture + knowledge check + quiz questions
- [ ] Any supporting assets (VMs, project files) are in place and listed in `assets/README.md`
- [ ] YAML frontmatter is complete
- [ ] All images have alt text
- [ ] All links are relative
- [ ] No linter warnings
- [ ] Content is in sentence case

### Peer Review

Content is reviewed by:
- Peer SME (technical accuracy)
- Instructional designer (pedagogical soundness)
- Quality assurance (format and style compliance)

## Publication Phase Workflow

Once review is complete, content is built for delivery.

### Build Process

```bash
make pdf          # Build PDF
make presentation # Build HTML presentation
make scorm        # Build SCORM module
```

### Quality Assurance Testing

Test the built output:
- [ ] All content renders correctly
- [ ] All images load properly
- [ ] All links work
- [ ] Interactive elements function
- [ ] PDFs print correctly
- [ ] SCORM modules launch in LMS

## Escalation Path

For questions or issues:

1. **First:** Check the LeGIT documentation in CLAUDE.md
2. **Design questions:** Contact Aba Azeem (instructional designer)
3. **Content questions:** Ask in the RAU learning team Slack channel
4. **Technical issues:** File an issue in the repository

---

For detailed information on specific topics, see:

- [Terminology Glossary](../terminology-glossary.md): the source of truth for all terms
- [Content Design Process](../design/content-design-process.md): the 9 design steps
- [File Mapping Guide](../design/file-mapping-guide.md): the deliverable file contract
- [Best Practices](../development/best-practices.md)
