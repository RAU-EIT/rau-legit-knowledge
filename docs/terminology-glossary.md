# RAU Terminology Glossary

This glossary is the **single source of truth** for RAU terminology. Other documents should link here rather than restating definitions.

It covers the chain from design decisions through student delivery, and, importantly, the fact that this chain is traversed in **two opposite directions** depending on which phase you are in.

> **These terms are tool-agnostic.** They describe RAU's design and development model, not LeGIT specifically. Everything here applies equally to content authored in LeGIT (markdown), in Word or PowerPoint, in RISE, or in any future tool. Where a term *is* LeGIT-specific, it says so explicitly.

---

## The Two Directions

This is the most important idea in this glossary. The same five terms form one chain, but design and build travel it in opposite directions.

### Design derives top-down

During design, you start with strategy and work *down* to files. Each decision determines the next:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
(how it's taught)     (what the    (what must     (what        (what the SME
                       student      be built)      learners     must produce)
                       enrolls in)                 must do)
```

Read as: *"We chose blended. That means a self-paced course plus an ILT course for customer training. To deliver those we need a SCORM module, an instructor deck, a TSL (Training Summary/Lesson Book), and a TLB (Training Lab Book). Those require lecture, lab, and quiz activities. Those activities convert to deliverables the SME must create, plus a VM environment for the instructors and students to use for the labs."*

### Build produces bottom-up

During build and delivery, the dependency runs the other way. Files are consumed to produce outputs, which are packaged for students:

```text
Deliverable  →  Publication  →  Offering
(the source)    (the built      (the packaged
                 output)         product)
```

Read as: *"The authored lecture content, knowledge check, and quiz build into a SCORM package. That SCORM package is loaded as the e-learning course a student enrolls in."*

### Why this matters

Both directions are correct. They are the same chain, not two different models.

If you only ever see the build direction, publications and offerings look like things that happen *after* authoring, which makes it unclear why you would decide them during design. **You decide them during design precisely because they determine what gets authored.** Deciding publications late means discovering late that you authored the wrong deliverables.

---

## The Full Lifecycle

```text
INTAKE            DESIGN PHASE                       BUILD / EXPORT
──────            (derives top-down)                 ──────────────
audiences    →    Delivery Strategy
roles                     ↓
                     Offerings          ← what the student enrolls in
                          ↓
                    Publications        ← what must be built, per audience/role,
                          ↓                scoped by the design hierarchy
                     Activities         ← what learners must do
                          ↓
                    Deliverables        ← content deliverables + supporting
                          ↓                assets (VMs, project files, etc.)
                    Content Database    →  creates the authoring project files
                                        →  exports deliverables to DevOps
                                                    ↓
                                        SMEs author  →  build  →  publications
                                                                       ↓
                                                                   offerings
                                                                   delivered
```

---

## Core Terminology

### Delivery Strategy

**Definition**: The design-phase decision about which modality will be used to teach a skill. It answers: *"How will this be taught?"*

**Options**:

- **E-Learning**: Self-paced, online, interactive
- **Classroom (ILT)**: Instructor-led, in a physical or virtual classroom
- **Blended**: Combination of self-paced online and classroom/synchronous components

**Characteristics**:

- Decided in design Step 3, using the six-factor decision framework
- The **first** link in the design derivation chain; everything downstream follows from it
- Determines the offerings, which determine the publications, which determine the activities, which determine the deliverables
- Recorded with an explicit rationale, not just a label

**Used in**: Design phase, content planning, stakeholder communication

---

### Offering

**Definition**: A student-facing, packaged learning product that a student enrolls in, purchases, or downloads.

**Offering is a two-state term**:

| State | Phase | Meaning |
| --- | --- | --- |
| **Defined** | Design (Step 4) | The decision about what students will receive, made once the delivery strategy is confirmed |
| **Delivered** | Delivery | The actual branded, packaged product deployed to an LMS or distribution channel |

These are the same offering at two points in its life, not two different things.

**Examples**:

- An e-learning module a student enrolls in via an LMS
- A classroom training course with an instructor
- A blended experience (self-paced online + classroom practicum)

**Characteristics**:

- Student-facing: this is what they experience and enroll in
- Scoped per audience/role: the same skill design may produce different offerings for a technician and an engineer
- May combine multiple publications
- May include supporting materials (certificates, job aids, reference guides)

**Used in**: Design Step 4, marketing, stakeholder communication, LMS enrollment

---

### Publication

**Definition**: A specific output produced from authored deliverables, planned in design and produced later. In LeGIT the build system produces it; in classic development it is the exported PDF, deck, or published RISE module.

**Publication is a two-state term**:

| State | Phase | Meaning |
| --- | --- | --- |
| **Decided** | Design (Step 5) | The plan: which publications are needed, for which audience/role, at what scope |
| **Produced** | Build | The actual built artifact: a `.zip`, `.pdf`, `.html`, `.mp4` |

A publication is *decided* during design because that decision is what tells you which activities and deliverables are required. It is *produced* at build time from those deliverables.

**Examples of publications**:

- SCORM 1.2 module (`.zip`) for the Technician role, covering Outcomes 1-3
- PDF lab manual for the classroom practicum
- HTML presentation (RevealJS) for instructor delivery
- Video recording (MP4 with narration)
- **TLB** (Training Lab Book): a lab-focused publication
- **TSL** (Training Summary/Lesson Book): a reference-focused publication

**Characteristics**:

- Planned in design, tagged with its audience/role, scope, and publication type
- Scoped by the **design hierarchy**: a publication may cover a whole skill, one outcome, or a single standalone objective
- Produced from deliverables, not hand-assembled
- Not student-branded yet; publications get packaged into offerings
- Versioned and tracked

> A single publication is **not** an offering on its own. A lab manual, for example, is a publication. It only becomes an offering if it is deliberately delivered to students standalone, for instance as tailored training. Otherwise it is one component of a larger offering.

**Used in**: Design Step 5, build system, content management, version control

---

### Activity

**Definition**: A learning activity that describes what learners will *do*. Activities are derived from the publications that need to be built.

**The three types.** Every outcome needs all three:

| Type | Example | Learner is... | Typically becomes |
| --- | --- | --- | --- |
| **Passive** | Lecture | Gaining exposure to concepts | Lecture content, knowledge checks |
| **Interactive** | Lab / Practice | Applying knowledge with guidance | A lab or guided practice exercise |
| **Assessment** | Quiz / Practical | Demonstrating mastery, graded | Quiz questions, or a practical + rubric |

**Characteristics**:

- Derived in design Step 6 from the publication set, not authored freehand
- Coverage (Passive + Interactive + Assessment) is validated at the **outcome** level, and additionally at the objective level for standalone objectives only
- Each activity maps to one or more deliverables

**Relationship**: Design-phase activities map directly to development-phase **deliverables**. If you define a lecture activity, an SME authors a lecture deliverable.

---

### Deliverable

**Definition**: Anything an SME must produce for a skill. Deliverables are the **superset** of every activity, plus everything else the skill needs in order to be delivered.

```text
Deliverables  =  every Activity  +  supporting assets
```

This is the key point: a deliverable is *not* only a document. Most deliverables are authored instructional content, but some are not, and those still have to be planned, assigned, and tracked.

#### Content deliverables

The authored instructional content. Each corresponds to an activity:

| Deliverable | Purpose |
| --- | --- |
| **Lecture** | Core instruction |
| **Knowledge check** | Embedded, ungraded comprehension checks |
| **Lab** | Hands-on guided practice |
| **Quiz questions** | Graded assessment question pool |
| **Quiz** | Assembled graded assessment |
| **Presentation** | Instructor-facing delivery material |
| **Handout** | Learner reference material |
| **Practical** | Hands-on assessment plus rubric |
| **Media** | Images, diagrams, and video referenced by the above |

**Authoring tool is a separate question.** The same deliverable can be authored in LeGIT (markdown), Word, PowerPoint, RISE, or another tool. The deliverable is the *lecture*, not the `.md` or `.docx` that holds it. For the LeGIT file structure specifically, see the [File Mapping Guide](./design/file-mapping-guide.md).

#### Supporting assets

Required by the skill and produced by the SME, but **not authored content**: no content-authoring tool produces these. They are still tracked as deliverables and exported for work tracking:

- Virtual machines (VMs) and test environments
- Project files (e.g. Studio 5000 `.ACD` files, configuration exports)
- Lab start files and finish files
- Externally produced media
- Supporting documentation (instructor setup guides, equipment lists)

**Why these count as deliverables**: a lab that depends on a VM is not deliverable without the VM. Omitting it from the deliverables list means the gap surfaces during development instead of during design.

**Characteristics**:

- Defined in design Step 7, after activities are known
- Created by SMEs, technical writers, and instructional designers
- Version-controlled, or referenced with a location and owner when stored elsewhere
- Feed the publishing process to produce publications

**Used in**: Content authoring, development workflow, version control, peer review, work tracking

---

### Content Database

**Definition**: The system of record for design data. It receives the validated design and drives the two downstream handoffs.

**What it does**:

- Creates the authoring project structure from the design (in LeGIT: the folder structure and file shells)
- Exports the deliverables list to DevOps for work tracking

**Characteristics**:

- Loaded in design Step 9, after the design is validated and refined
- Replaces the CDD Workbook as the system of record

> **Transitional note**: the content database is still being built. Until it is live, the **CDD Workbook remains the interim system of record**, and its entry is still required. Treat the CDD Workbook as a fallback, not the target state.

---

## Related Terminology

### Publication Type

**Definition**: The technical file type a publication is produced as. Distinct from the publication itself.

- A **publication** is a planned output: *"a SCORM module for the Technician role covering Outcomes 1-3."*
- A **publication type** is what it is produced as: `scorm1.2`.

> **Do not call this "format."** In RAU usage, *format* has historically meant the delivery modality (ILT or e-learning), which is now called [delivery strategy](#delivery-strategy). Using "format" for the file type collides with that. The two are different decisions at different levels:
>
> - **Delivery strategy** = ILT / e-learning / blended (design Step 3)
> - **Publication type** = `print` / `scorm1.2` / `revealjs`, etc. (design Step 5)

In LeGIT, the publication type is set by the **`docType`** field in YAML frontmatter. `docType` is the LeGIT field name; "publication type" is the tool-agnostic concept.

<!-- VERIFY: The valid docType enum is inconsistent across this repo. CLAUDE.md and this
     file list five values; .claude/rules/legit-yaml.md permits only print, revealjs, and
     scorm (note: `scorm`, not `scorm1.2`). This repo contains no build code, so the true
     enum must be confirmed against the actual LeGIT Pandoc/Lua pipeline before SMEs rely
     on it. Do not treat the list below as authoritative until verified. -->

| Publication type (`docType`) | Produces |
| --- | --- |
| `print` | PDF documents |
| `revealjs` | HTML presentations (reveal.js) |
| `pptx` | PowerPoint presentations |
| `scorm1.2` | SCORM 1.2 e-learning modules |
| `presentation-video` | MP4 video recordings |

**Relationship**: Publication types follow from the delivery strategy. E-learning offerings use `scorm1.2`; classroom offerings use `revealjs` or `pptx`.

---

### Design Hierarchy

**Definition**: The structure of outcomes and objectives for a skill: how many outcomes, how many objectives each has, and which objectives are marked standalone.

**Why it matters**: the design hierarchy **scopes** publications. It determines whether a publication covers an entire skill, a single outcome, or one standalone objective, and therefore how many publications the skill needs.

**Used in**: Design Step 5 (as an input to publication scoping), Step 7 (deliverable generation)

---

### Standalone Objective

**Definition**: An objective designed as an independent learning unit, completable on its own.

- **Non-standalone** (default): rolls into its parent outcome for delivery. Contributes lecture content; shares interactive and assessment activities at the outcome level.
- **Standalone** (exception): must have its own complete Passive + Interactive + Assessment coverage, and may generate its own publication.

**Used in**: Design Step 2, coverage validation (Rules 3 and 4), publication scoping

---

### Content Block / LeGIT Block

> **LeGIT-specific.** This is the one term here tied to a particular tool. Content authored in Word, PowerPoint, or RISE uses that tool's own constructs instead.

**Definition**: Pre-built structural and interactive elements available in LeGIT markdown that provide special formatting, interactivity, or presentation.

**Examples**: alerts (reference, warning, important, tip), accordions, steps, quizzes, image hotspots, flip cards, columns.

**Used in**: Content authoring, inside LeGIT-authored content deliverables

---

## Quick Reference Table

| Term | Decided in | Realized in | Owner | Form |
| --- | --- | --- | --- | --- |
| **Delivery Strategy** | Design (Step 3) | - | Instructional Designer | Decision + rationale |
| **Offering** | Design (Step 4) | Delivery | Product / Instructional Designer | Packaged student product |
| **Publication** | Design (Step 5) | Build | Instructional Designer / Build System | PDF, SCORM, HTML, MP4 |
| **Activity** | Design (Step 6) | Development | Instructional Designer | Abstract learning activity |
| **Deliverable** | Design (Step 7) | Development | SME / Technical Writer | Authored content, or a supporting asset |
| **Content Database** | Design (Step 9) | Build / Export | Content Ops | System of record |

---

## For Different Audiences

### Instructional Designers

- **Focus on**: Delivery strategy, offerings, publications, activities
- **Work through**: Design Steps 1-8, top-down
- **Produce**: A validated design, including the deliverables list that drives development

### Content Developers / SMEs

- **Focus on**: Deliverables, file structure, authoring standards
- **Consume**: The deliverables list and generated file shells
- **Produce**: Authored content deliverables, plus supporting assets (VMs, project files)

### Build System / Operations

- **Focus on**: Deliverables in, publications out
- **Consume**: Authored deliverables
- **Produce**: Publications (PDF, SCORM, HTML, MP4)

### Student Training / PODs

- **Focus on**: Offerings, packaging, branding
- **Consume**: Publications from the build system
- **Produce**: Offerings deployed to LMS, websites, and distribution channels

---

## Related Documents

- [Content Design Process](./design/content-design-process.md): the 9 design steps in detail
- [File Mapping Guide](./design/file-mapping-guide.md): the file structure deliverables take
- [Content Design Validation](../.claude/rules/content-design-validation.md): rules Claude Code checks

---

**Last Updated**: 2026-09-03
**Maintained by**: Peter Francis (pfranci@rockwellautomation.com)
