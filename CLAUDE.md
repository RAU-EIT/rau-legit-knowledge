# RAU LeGIT Knowledge Base

Welcome to the RAU LeGIT Knowledge Base. This is your complete guide to **designing** and **developing** learning content from skills through publication.

---

## ✍️ Writing Style Rules (Mandatory)

These rules apply to **all** content in this repository and to **all** content Claude Code writes: documentation, rules, skills, lectures, labs, quizzes, presentations, and commit messages.

### Rule: Never use em dashes

**Do not use the em dash character (Unicode `U+2014`, HTML `&mdash;`). Ever.**

> This section deliberately refers to the banned characters by code point rather than
> printing them, so that the repository contains zero literal instances and the
> self-check below returns a clean result.

Replace it with the punctuation that actually fits the sentence:

| Context | Use instead | Example |
| --- | --- | --- |
| Label followed by its definition | Colon | `**E-Learning**: Self-paced, online, interactive` |
| Introducing an explanation | Colon | `The lab is not deliverable: the VM is missing.` |
| Two independent clauses | Semicolon | `Publications are planned in design; they are produced at build.` |
| A subordinate or aside clause | Comma | `Decided in design, which is why it drives authoring.` |
| A parenthetical aside | Parentheses or commas | `the delivery modality (ILT or e-learning)` |
| A clean break between thoughts | Period | `Record the rationale. A strategy without one fails validation.` |

**Also avoid these related smart-typography characters**, for the same reason:

| Character | Code point | Use instead |
| --- | --- | --- |
| En dash | `U+2013` | A plain hyphen: `Outcomes 1-3`, `2-5 objectives` |
| Ellipsis | `U+2026` | `etc.` or three plain periods |
| Curly double quotes | `U+201C` / `U+201D` | Straight quotes: `"` |
| Curly single quotes | `U+2018` / `U+2019` | Straight apostrophe: `'` |

**Why**: these characters are inconsistent with RAU house style, they are harder to read and search for, and they are a recognizable tell of machine-generated text. Choosing the correct punctuation also forces clearer sentence structure.

### Do not touch ASCII diagrams

The box-drawing characters used in folder trees and flow diagrams are in the `U+2500` range
(`U+2500`, `U+2502`, `U+251C`, `U+2514`) and only *look* like dashes. They are **not** em
dashes. Leave them alone. Arrows (`U+2190` to `U+2193`) are also fine.

A naive find-and-replace across dash-like characters will corrupt every diagram in this
repository. Target `U+2014` specifically.

### Self-check before finishing any edit

Run this from the repository root. It uses hex escapes rather than the literal characters,
so this file stays free of the very characters it bans.

```bash
# All four must return zero matches
grep -rnP '\x{2014}' --include="*.md" .                        # em dash
grep -rnP '\x{2013}' --include="*.md" .                        # en dash
grep -rnP '\x{2026}' --include="*.md" .                        # ellipsis
grep -rnP '[\x{2018}\x{2019}\x{201C}\x{201D}]' --include="*.md" .   # curly quotes
```

Two traps worth knowing:

- **`grep -E` does not interpret `\u` or `\x{...}` escapes.** Use `grep -P` as above, or the
  shell's `$'...'` quoting. With `-E` the check silently matches nothing and you will
  wrongly conclude the repository is clean.
- **Do not sed the whole repository at once.** Verify the replacement reads correctly in
  context. A blind swap to a hyphen produces comma splices and broken grammar; the right
  substitute depends on the sentence, per the table above.

---

## Quick Navigation

### 📖 Start Here: Terminology

**Before anything else**, read [`docs/terminology-glossary.md`](./docs/terminology-glossary.md). It is the **single source of truth** for what "offering," "publication," "activity," and "deliverable" mean, and how they relate. Every other document defers to it.

### 👨‍🏫 Content Design Track

**For**: Instructional Designers, Product Managers, SMEs
**Goal**: Design learning (outcomes, objectives, delivery strategy, offerings, publications, activities, deliverables)
**Go to**: [Content Design Guide](./docs/repo-info/CLAUDE_REORGANIZED.md#content-design-guide)

### 📝 Content Development Track

**For**: SMEs, Technical Writers, Content Developers
**Goal**: Author training content (lectures, labs, quizzes in LeGIT format)
**Go to**: [Content Development Guide](./docs/repo-info/CLAUDE_REORGANIZED.md#content-development-guide)

---

## Choose Your Path

**Starting a new training project?**
→ [Section 0: Content Design Phase](#section-0-content-design-phase)

**Already designed, ready to author?**
→ [Section 3: Deliverable File Structure](#section-3-deliverable-file-structure)

**Need help?**
→ [Section 7: Getting Started](#section-7-getting-started)

**Want a quick overview?**
→ [Quick Reference Card](./QUICK_REFERENCE_CARD.md)

---

## 🤖 Documentation Maintenance: Automatic Sync Validation

**Important**: When you commit changes to `docs/` files, Claude automatically validates that `.claude/rules/` files stay in sync.

### How It Works

When you commit changes to documentation files (anything in `docs/` folder):

1. **Git hook triggers** → Claude Code runs automatic review
2. **Comparison happens** → Current rules vs. updated documentation
3. **Claude provides guidance** → Shows what needs updating in rules
4. **Team makes decision** → You review Claude's recommendations and approve changes
5. **Rules auto-update** → Once approved, `.claude/rules/` files update to match docs

### Claude's Role: Advisory, Not Enforcement

Claude will:

- ✅ **Identify differences** between docs and rules
- ✅ **Suggest updates** to keep them in sync
- ✅ **Provide rationale** for why updates matter
- ✅ **Recommend changes** for team review and approval

Claude will NOT:

- ❌ Enforce rules rigidly without context
- ❌ Auto-update rules without team review
- ❌ Replace team judgment with automation

### Example Workflow

```text
You: Commit updated decision framework to docs/design/content-design-process.md
  ↓
Git hook: Triggers Claude sync validation
  ↓
Claude: "I see you updated the delivery strategy decision framework.
         The version in .claude/rules/content-design-validation.md is outdated.
         Here's what should change:
         [Shows side-by-side comparison]

         Recommendation: Update Rule 5 Part A to match the new criteria.
         Rationale: This ensures Claude Code skills use the latest guidance."
  ↓
Team: Reviews Claude's analysis
  ↓
Team: Approves and applies the recommended changes to rules
  ↓
Rules auto-update to match documentation
```

### For More Details

See: [`KEEPING_DOCS_IN_SYNC.md`](./docs/repo-info/KEEPING_DOCS_IN_SYNC.md)

---

## What is LeGIT?

LeGIT is **RAU's skills-based content development system** using Markdown, Git, and cloud-native publishing. It enables SMEs to author **atomic learning content** that automatically builds into PDFs, presentations, SCORM modules, and videos.

---

## Publication Types

A **publication** is a specific output the build system produces. Its **publication type** determines the format, set by the `docType` field in YAML frontmatter.

<!-- VERIFY: The valid docType enum is inconsistent across this repo. This table lists five
     values; .claude/rules/legit-yaml.md permits only print, revealjs, and scorm (note:
     `scorm`, not `scorm1.2`). This repo contains no build code, so the true enum must be
     confirmed against the actual LeGIT Pandoc/Lua pipeline before SMEs rely on it. -->

| Publication type (`docType`) | Format | Best For |
| --- | --- | --- |
| **print** | `.pdf` | Printable documents, lab manuals, handouts |
| **revealjs** | `.html` | Web-based presentations |
| **pptx** | `.pptx` | Standalone PowerPoint |
| **scorm1.2** | `.zip` | LMS-compatible e-learning |
| **presentation-video** | `.mp4` | Recorded video with narration |

---

# SECTION 0: CONTENT DESIGN PHASE

**This section is critical.** Design happens first; development follows. Before any markdown is written, learning intent must be defined, and so must everything that intent implies.

## Design Derives Top-Down

Design works **downward**. Each decision determines the next:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
    (Step 3)          (Step 4)      (Step 5)       (Step 6)      (Step 7)
```

The build system later runs this chain **in reverse**: deliverables build into publications, which are packaged into offerings. Both directions describe the same chain, traversed for different purposes.

**Critical Principle**: decide offerings and publications *during* design, not after. They are what tell you which activities and deliverables are actually required.

See [Terminology Glossary: The Two Directions](./docs/terminology-glossary.md#the-two-directions).

## Step 1: Define Learning Outcomes Using ABCD

Every skill must have at least one **learning outcome**: a measurable statement of what learners will demonstrate.

Use the **ABCD Method**:

- **A (Audience)**: Who is learning?
- **B (Behavior)**: What observable action demonstrates learning?
- **C (Condition)**: Under what circumstances?
- **D (Degree)**: To what standard?

**Example**:
> "Given a schematic diagram of a hydraulic system, the field technician will analyze the components and explain the function of each **with 90% accuracy**."

## Steps 2-9: Complete Content Design

See the detailed step-by-step guide in [`docs/design/content-design-process.md`](./docs/design/content-design-process.md):

- Step 2: Define Learning Objectives (2-5 per outcome)
- Step 3: Determine Delivery Strategy (e-learning, classroom, blended)
- Step 4: Define Offerings (student-facing packages, per audience/role)
- Step 5: Define Publications (what the build system must produce)
- Step 6: Determine Activities (Passive + Interactive + Assessment)
- Step 7: Define Deliverables (activities + supporting assets)
- Step 8: Validate & Refine Design
- Step 9: Load into Content Database

---

# SECTION 1: QUICK REFERENCE

## I Want to Create...

### "...a skill lesson"

**Use**: Complete the design phase → the content database generates the structure → author markdown

### "...a presentation"

- **Interactive HTML**: `revealjs` publication type
- **PowerPoint file**: `pptx` publication type
- **Presentation video**: `presentation-video` publication type

### "...a lab/hands-on exercise"

**Use**: Lab deliverable → publication type `print` OR `scorm1.2` → design with labs → author content

### "...an interactive course (SCORM)"

**Use**: Multiple objectives → publication type `scorm1.2` → add interactive blocks

---

# SECTION 2: VALIDATION & RULES

Claude Code enforces these rules:

1. ABCD outcome completeness
2. Objective-to-outcome alignment
3. Coverage completeness (Passive + Interactive + Assessment)
4. Standalone objective designation
5. **Delivery Strategy & Deliverable Alignment**: the whole chain: strategy → offerings → publications → activities → deliverables
6. Deliverable file completeness

Rule 7 (publication-to-activity derivation) is a **placeholder and is not enforced yet**.

**Link**: [`.claude/rules/content-design-validation.md`](./.claude/rules/content-design-validation.md)

---

# SECTION 3: DELIVERABLE FILE STRUCTURE

**Key principle**: Content is authored at the **objective level** for reuse, then packaged at the **outcome level** for delivery.

## File Naming Convention

```text
skills/[skill-name]/
├── [outcome-title]/                  (e.g., analyze-hydraulic-components)
│   ├── objective-##/
│   │   ├── lecture.md                (Always; core instruction)
│   │   ├── knowledge-check.md        (Always; embedded checks)
│   │   ├── quiz-questions.md         (Always; feeds outcome quiz pool)
│   │   ├── lab.md                    (Only if the objective is standalone)
│   │   └── media/                    (Objective-specific images/files)
│   ├── outcome-##-lecture.md         (Always; uses !include)
│   ├── outcome-##-lab.md             (Interactive activity for non-standalone objectives)
│   ├── outcome-##-quiz.md            (Always; outcome-level assessment)
│   ├── outcome-##-presentation.md    (Classroom or blended)
│   ├── outcome-##-practical.md       (If assessment is hands-on)
│   └── outcome-##-handout.md         (Blended, or planned learner reference)
├── assets/                           (Supporting assets + required README.md)
└── media/                            (Shared media across outcomes)
```

**Outcome Titles**: Use the outcome title in kebab-case as the folder name.

**Why `quiz-questions.md` is always authored but `lab.md` is not**: quiz questions live per objective so the outcome quiz can draw a pool traceable to each objective. Labs live at the outcome level because non-standalone objectives share one interactive activity. A standalone objective must be independently completable, so it needs its own lab.

**Supporting assets** (VMs, project files, lab start/finish files) are first-class deliverables. They live under `assets/` with a required inventory in `assets/README.md`.

**Who creates this structure**: the **content database**, from the validated design. SMEs do not hand-create it, and Claude Code does not scaffold it.

**Detailed guide**: [`docs/design/file-mapping-guide.md`](./docs/design/file-mapping-guide.md)

---

# SECTION 4: CONTENT BLOCKS QUICK SELECT

Available in ANY publication type:

- **Alerts** (Reference, Warning, Attention, Important, Tip)
- **Columns** (2-col or 3-col)
- **Images**: With captions and sizing
- **Video/Audio embeds**: Local or remote

For PRINT: **important**, **rau-input-\***, **rau-page-break**

For SCORM: **rau-accordion**, **rau-hero**, **rau-image-hotspot**, **rau-flip-card**, **rau-steps**, **rau-quiz**

For PRESENTATIONS: **rau-slide-\***, **Speaker notes**

**Full reference**: [`docs/development/content-blocks-reference.md`](./docs/development/content-blocks-reference.md)

---

# SECTION 5: WHEN TO ASK CLAUDE CODE

## ALWAYS Use Claude Code For

- "Validate my content design"
- "Create a lecture for [objective]"
- "How do I create a quiz?"
- "What does [term] mean?" (or read the glossary)

## DO NOT Use Claude Code For

- Strategic decisions → Talk to your project manager
- Subject matter → Talk to your SME
- Instructional design → Ask Aba Azeem
- Creating the project file structure → That is the content database's job

---

# SECTION 6: FULL DOCUMENTATION

**Terminology** (read first):

- [Terminology Glossary](./docs/terminology-glossary.md): the source of truth for all terms

**Design Phase**:

- [Content Design Process Guide](./docs/design/content-design-process.md): the 9 steps in detail
- [File Mapping Guide](./docs/design/file-mapping-guide.md): the deliverable file contract

**Development Phase**:

- [LeGIT Fundamentals](./docs/development/legit-fundamentals.md)
- [Markdown Standards](./docs/development/markdown-standards.md)
- [YAML Guide](./docs/development/yaml-guide.md)
- [Content Blocks Reference](./docs/development/content-blocks-reference.md)
- [Presentations](./docs/development/presentations.md)
- [Best Practices](./docs/development/best-practices.md)

**Validation**:

- [Content Design Validation](./.claude/rules/content-design-validation.md): rules Claude Code enforces

---

# SECTION 7: GETTING STARTED

**You're a new SME. Follow these steps:**

1. **Learn the terminology** (15 min)
   - Read [`docs/terminology-glossary.md`](./docs/terminology-glossary.md), especially "The Two Directions"

2. **Understand the design phase** (30 min)
   - Read Section 0 above
   - Review the publication types

3. **Complete the design** (1-2 weeks)
   - Follow [`docs/design/content-design-process.md`](./docs/design/content-design-process.md)
   - Ask Claude Code: "Validate my content design"

4. **Load into the content database** (1 day)
   - The database creates the LeGIT project files and exports deliverables to DevOps
   - *Transitional*: until the database is live, use the CDD Workbook and create the structure from the deliverable manifest

5. **Author content** (ongoing)
   - Write lectures, labs, and assessments into the generated files
   - Ask Claude Code for syntax help

6. **Build and publish** (1 day)
   - Create/update `build.yaml`
   - Run the build task

---

**For Questions**:

- Terminology: See [`docs/terminology-glossary.md`](./docs/terminology-glossary.md)
- Design: See Section 0 or ask Claude Code
- Complex: Contact Aba Azeem (<aba.azeem@rockwellautomation.com>)

**Last Updated**: 2026-09-03
**Repository**: <https://github.com/RAU-EIT/rau-legit-knowledge>
