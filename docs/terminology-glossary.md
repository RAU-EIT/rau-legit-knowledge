# RAU Terminology Glossary

This glossary defines key terms used throughout the RAU development system, from design through publication to student delivery.

---

## The Three Tiers: Design → Development → Delivery

RAU has a clear hierarchy from design decisions through student delivery:

```
DESIGN PHASE                DEVELOPMENT PHASE           BUILD & DELIVERY PHASE
─────────────────           ─────────────────           ─────────────────

Design Activities       →    Deliverables (Files)   →   Publications (Outputs)   →   Offerings (Packages)
(What learners need)        (What SMEs author)          (What system builds)         (What students get)

Lecture activity        →    lecture.md              →   SCORM, PDF, HTML        →   E-learning course
Quiz activity           →    quiz-questions.md      →   Assessment tools        →   Online assessment
Lab activity            →    lab.md                 →   Lab manual PDF          →   Practical exercise
```

---

## Core Terminology

### Offering
**Definition**: A student-facing, packaged learning product that a student enrolls in, purchases, or downloads.

**Examples**:
- An e-learning module enrolled in an LMS
- A classroom training course with instructor
- A blended learning experience (online + classroom)

**Characteristics**:
- Student-facing (what they experience)
- Branded and packaged for delivery
- May combine multiple publications
- May include supporting materials (certificates, references, job aids)

**Used in**: Marketing, stakeholder communication, student delivery, LMS enrollment

---

### Publication
**Definition**: A concrete output produced by the LeGIT build system from authored deliverables. Publications are the intermediate products that either get deployed directly to students or packaged into student-facing offerings.

**Examples of Publications**:
- SCORM 1.2 module (.zip file)
- PDF document (printable guide, lab manual)
- HTML presentation (RevealJS)
- Video recording (MP4 with narration)
- Reference materials (handouts, job aids)
- TLB (Training Lab Book) — Lab-focused publication
- TSL (Training Summary/Lesson Book) — Reference-focused publication

**Characteristics**:
- System-generated from deliverables via build process
- Format-specific (PDF, SCORM, HTML, video, etc.)
- Not student-branded yet (may be rebranded into offerings)
- Versioned and tracked in source control
- May be intermediate (combined into offerings) or final (deployed as-is)

**Used in**: Build system, technical documentation, content management, version control

---

### Deliverable
**Definition**: The concrete work product that a subject matter expert (SME) creates during the development phase. Deliverables are the actual markdown files and supporting assets that become the source content for publications.

**Examples of Deliverables**:
- Lecture content
- Lab/hands-on exercise content
- Assessment questions
- Knowledge checks
- Presentation slides
- Practical assessment
- Supporting media files (images, diagrams, videos)
- Virtual machines (VMs) or test environments
- Supporting documentation

**Characteristics**:
- Created by SMEs (technical writers, instructional designers, subject matter experts)
- Markdown-based + YAML frontmatter
- Maps 1:1 to design-phase activities
- Tracked in Git version control
- Feeds into build system to create publications

**Used in**: Content authoring, development workflows, version control, peer review

---

### Delivery Strategy
**Definition**: The strategic decision about how a skill will be delivered to students. It answers: "What modality and packaging will best serve this learning?"

**Options**:
- **E-Learning** — Self-paced, online, interactive offering
- **Classroom (ILT)** — Instructor-led training in a physical or virtual classroom
- **Blended** — Combination of online self-paced and classroom/synchronous components

**Characteristics**:
- Determined during design phase (Step 3)
- Drives which deliverables must be created
- Drives which publications will be generated
- Drives what offerings are packaged for students

**Used in**: Design phase, content planning, team communication

---

## Related Terminology

### Activity (Design-Phase)
**Definition**: A learning activity defined during the design phase that describes what learners will do to achieve an objective.

**Types**:
- **Passive Activity** (Lecture) — Exposure to concepts and information
- **Interactive Activity** (Lab/Practice) — Guided practice with feedback
- **Assessment Activity** (Quiz/Practical) — Demonstration of mastery

**Relationship**: Design-phase activities map directly to development-phase deliverables. If you define a "lecture activity," an SME creates a `lecture.md` deliverable.

---

### Publication Type
**Definition**: The technical format that publications are built into.

**Options**:
- **print** → PDF documents
- **revealjs** → HTML presentations (reveal.js framework)
- **pptx** → PowerPoint presentations
- **scorm1.2** → SCORM 1.2 e-learning modules
- **presentation-video** → MP4 video recordings

**Relationship**: Output types are determined by delivery strategy. E-learning offerings use scorm1.2 output type. Classroom offerings use revealjs or pptx output types.

---

### Content Block / LeGIT Block
**Definition**: Pre-built structural and interactive elements available in LeGIT markdown that enable special formatting, interactivity, or presentation.

**Examples**:
- Alert blocks (reference, warning, important, tip)
- Accordion blocks
- Steps blocks
- Quiz blocks
- Image hotspot blocks
- Columns blocks

**Used in**: Content authoring, markdown files (deliverables)

---

## Workflow Mapping

### From Design to Student

```
DESIGN PHASE (CDD Created)
├─ Outcomes defined (ABCD)
├─ Objectives defined (2-5 per outcome)
├─ Delivery strategy chosen (e-learning/classroom/blended)
├─ Activities mapped (lecture, lab, quiz, etc.)
└─ Deliverables list created

        ↓

DEVELOPMENT PHASE (Deliverables Authored)
├─ SMEs create markdown files
├─ lecture.md authored
├─ lab.md authored  
├─ quiz-questions.md authored
├─ Media created (images, videos)
└─ Files committed to Git

        ↓

BUILD PHASE (Publications Generated)
├─ Build system processes deliverables
├─ Applies delivery strategy rules
├─ Creates output formats (PDF, SCORM, HTML, MP4)
└─ Generates publications (final outputs)

        ↓

DELIVERY PHASE (Offerings Packaged)
├─ Publications branded/packaged
├─ Student offerings created
├─ Deployed to LMS, websites, or distribution channels
└─ Students enroll/access offerings
```

---

## Quick Reference Table

| Term | Layer | Creator | Format | Purpose |
|------|-------|---------|--------|---------|
| **Activity** | Design | Instructional Designer | Abstract | Define what learners will do |
| **Deliverable** | Development | SME/Technical Writer | Markdown + YAML | Author source content |
| **Publication** | Build | Build System | PDF/SCORM/HTML/MP4 | Create technical outputs |
| **Offering** | Delivery | Product/Marketing | Packaged Product | Present to students |
| **Offering Strategy** | Design | Instructional Designer | Decision/Rationale | Guide deliverable and publication decisions |


---

## For Different Audiences

### Instructional Designers
- Focus on: Activities, Delivery Strategy, CDD
- Create: Design documents defining activities and delivery strategy
- Outcome: Deliverables list that drives development

### Content Developers / SMEs
- Focus on: Deliverables, File structure, Authoring standards
- Create: Markdown files (lecture.md, lab.md, quiz-questions.md) that become deliverables
- Input: Deliverables list from CDD
- Outcome: Authored files committed to Git

### Build System / Operations
- Focus on: Deliverables, Publications, Output formats
- Consume: Authored deliverables (markdown files)
- Produce: Publications (PDF, SCORM, HTML, MP4)
- Outcome: Publishable outputs ready for delivery

### Student Training / PODs
- Focus on: Offerings, Student packaging, Branding
- Consume: Publications from build system
- Package: Publications into student-facing offerings
- Outcome: Student offerings deployed to LMS, websites, etc.

---

**Last Updated**: 2026-09-02  
**Maintained by**: Peter Francis (pfranci@rockwellautomation.com)
