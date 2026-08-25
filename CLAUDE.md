---
title: "LeGIT Knowledge Base - Reorganized Index"
description: "RAU LeGIT Knowledge: Content Design & Content Development tracks"
version: "2.0.0"
last_updated: "2026-08-20"
maintained_by: "pfranci@rockwellautomation.com"
repo_url: "https://github.com/RAU-EIT/rau-legit-knowledge"
type: "knowledge-index"
---

# RAU LeGIT Knowledge Base

Welcome to the RAU LeGIT Knowledge Base. This is your complete guide to **designing** and **developing** learning content from skills through publication.

⭐ **NEW**: This knowledge base is now organized into two clear tracks. Choose your path below.

## Quick Navigation

### 👨‍🏫 Content Design Track
**For**: Instructional Designers, Product Managers, SMEs  
**Goal**: Design learning (outcomes, objectives, modalities, activities)  
**Go to**: [`CLAUDE_REORGANIZED.md#content-design-guide`](./docs/repo-info/CLAUDE_REORGANIZED.md#content-design-guide)

### 📝 Content Development Track  
**For**: SMEs, Technical Writers, Content Developers  
**Goal**: Author training content (lectures, labs, quizzes in LeGIT format)  
**Go to**: [`CLAUDE_REORGANIZED.md#content-development-guide`](./docs/repo-info/CLAUDE_REORGANIZED.md#content-development-guide)

---

## Choose Your Path

**Starting a new training project?**  
→ [Content Design Guide](./docs/repo-info/CLAUDE_REORGANIZED.md#content-design-guide)

**Ready to author content?**  
→ [Content Development Guide](./docs/repo-info/CLAUDE_REORGANIZED.md#content-development-guide)

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

```
You: Commit updated decision framework to docs/content-design-process.md
  ↓
Git hook: Triggers Claude sync validation
  ↓
Claude: "I see you updated the publication strategy decision framework.
         The version in .claude/rules/content-design-validation.md is outdated.
         Here's what should change:
         [Shows side-by-side comparison]
         
         Recommendation: Update Rule 5 to match the new criteria.
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

## Quick Navigation

- **Starting a new project?** → [Section 0: Content Design Phase](#section-0-content-design-phase)
- **Already designed, ready to author?** → [Section 4: File Mapping](#section-4-file-mapping)
- **Need help?** → [Section 8: Getting Started](#section-8-getting-started)

---

## What is LeGIT?

LeGIT is **RAU's skills-based content development system** using Markdown, Git, and cloud-native publishing. It enables SMEs to author **atomic learning content** that automatically builds into PDFs, presentations, SCORM modules, and videos.

---

## The Four Output Types

| Output Type | Format | Best For |
|------------|--------|----------|
| **print** | `.pdf` | Printable documents, lab manuals |
| **revealjs** | `.html` | Web-based presentations |
| **pptx** | `.pptx` | Standalone PowerPoint |
| **scorm1.2** | `.zip` | LMS-compatible e-learning |
| **presentation-video** | `.mp4` | Recorded video with narration |

---

# SECTION 0: CONTENT DESIGN PHASE

**This section is critical.** Design happens first; development follows. Before any markdown is written, learning intent must be defined.

## Overview: Design Before Development

LeGIT distinguishes between **design** (what learners must do) and **delivery** (how content is packaged).

**Critical Principle**: Design decisions MUST be made before delivery decisions.

## Step 1: Define Learning Outcomes Using ABCD

Every skill must have at least one **learning outcome** — a measurable statement of what learners will demonstrate.

Use the **ABCD Method**:

- **A (Audience)**: Who is learning?
- **B (Behavior)**: What observable action demonstrates learning?
- **C (Condition)**: Under what circumstances?
- **D (Degree)**: To what standard?

**Example**:
> "Given a schematic diagram of a hydraulic system, the field technician will analyze the components and explain the function of each **with 90% accuracy**."

For detailed guide: See [`docs/content-design-process.md`](/docs/content-design-process.md)

## Steps 2-7: Complete Content Design

See detailed step-by-step guide in [`docs/content-design-process.md`](/docs/content-design-process.md):

- Step 2: Define Learning Objectives (2-5 per outcome)
- Step 3: Determine Publication Strategy (e-learning, classroom, blended)
- Step 4: Map to Required Deliverables
- Step 5: Plan Activity Coverage (Passive + Interactive + Assessment)
- Step 6: Enter Design into CDD Workbook
- Step 7: Validate Your Design (Claude Code + Aba Azeem)

---

# SECTION 1: QUICK REFERENCE

## I Want to Create...

### "...a skill lesson"
**Use**: Skill-based folder → Choose output type → Complete design phase → Author markdown

### "...a presentation"
- **Interactive HTML**: Use `revealjs` output type
- **PowerPoint file**: Use `pptx` output type
- **Presentation video**: Use `presentation-video` output type

### "...a lab/hands-on exercise"
**Use**: Lab template → Output type: `print` OR `scorm` → Design with labs → Author content

### "...an interactive course (SCORM)"
**Use**: Multiple objectives → Build with `scorm1.2` → Add interactive blocks

---

# SECTION 2: VALIDATION & RULES

Claude Code enforces these rules:

- ABCD outcome completeness
- Objective-to-outcome alignment
- Coverage completeness (Passive + Interactive + Assessment)
- Standalone objective designation
- Modality-deliverable alignment
- File mapping completeness

**Link**: [`.claude/rules/content-design-validation.md`](/.claude/rules/content-design-validation.md)

---

# SECTION 3: FILE MAPPING

**Key principle**: Content is authored at the **objective level** for reuse, then packaged at the **outcome level** for delivery.

## File Naming Convention

```
skills/[skill-name]/
├── [outcome-title]/                    (e.g., analyze-hydraulic-components)
│   ├── objective-##/
│   │   ├── lecture.md           (Always; core instruction)
│   │   ├── knowledge-check.md   (Always; embedded checks)
│   │   ├── lab.md               (Only if standalone)
│   │   ├── quiz-questions.md    (Always; feeds outcome quiz)
│   │   └── media/               (Objective-specific images/files)
│   ├── outcome-##-lecture.md    (Always; uses !include)
│   └── outcome-##-quiz.md       (Outcome-level assessment)
└── media/                        (Shared media across outcomes)
```

**Outcome Titles**: Use the outcome title in kebab-case as the folder name for clarity in build outputs and file navigation.

**Detailed guide**: [`docs/file-mapping-guide.md`](/docs/file-mapping-guide.md)

---

# SECTION 4: CONTENT BLOCKS QUICK SELECT

Available in ANY output type:
- **Alerts** (Reference, Warning, Attention, Important, Tip)
- **Columns** (2-col or 3-col)
- **Images** — With captions and sizing
- **Video/Audio embeds** — Local or remote

For PRINT: **important**, **rau-input-***,  **rau-page-break**

For SCORM: **rau-accordion**, **rau-hero**, **rau-image-hotspot**, **rau-flip-card**, **rau-steps**, **rau-quiz**

For PRESENTATIONS: **rau-slide-***, **Speaker notes**

**Full reference**: [`docs/content-blocks-reference.md`](/docs/content-blocks-reference.md) (coming)

---

# SECTION 5: WHEN TO ASK CLAUDE CODE

### ALWAYS Use Claude Code For:
- "Validate my content design"
- "Create a lecture for [objective]"
- "How do I create a quiz?"
- "Create file structure for this skill"

### DO NOT Use Claude Code For:
- Strategic decisions → Talk to project manager
- Subject matter → Talk to your SME
- Instructional design → Ask Aba Azeem

---

# SECTION 6: FULL DOCUMENTATION

**Design Phase**:
- [Content Design Process Guide](/docs/content-design-process.md) — Step-by-step ABCD, objectives, modality, coverage
- [File Mapping Guide](/docs/file-mapping-guide.md) — Design to files with examples

**Validation**:
- [Content Design Validation](/.claude/rules/content-design-validation.md) — Rules Claude Code enforces

---

# SECTION 7: GETTING STARTED

**You're a new SME. Follow these steps:**

1. **Understand Design Phase** (30 min)
   - Read Section 0 above
   - Review the four output types

2. **Complete Design** (1-2 weeks)
   - Follow [`docs/content-design-process.md`](/docs/content-design-process.md)
   - Ask Claude Code: "Validate my content design"

3. **Plan Files** (1 day)
   - Use [`docs/file-mapping-guide.md`](/docs/file-mapping-guide.md)
   - Ask Claude Code to scaffold structure

4. **Author Content** (ongoing)
   - Write lectures, labs, assessments
   - Ask Claude Code for syntax help

5. **Build and Publish** (1 day)
   - Create/update build.yaml
   - Run build task

---

**For Questions**:
- Design: See Section 0 or ask Claude Code
- Complex: Contact Aba Azeem (aba.azeem@rockwellautomation.com)

**Last Updated**: 2026-08-14  
**Repository**: https://github.com/RAU-EIT/rau-legit-knowledge
