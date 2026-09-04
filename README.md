# RAU LeGIT Knowledge Base

This repository contains the shared knowledge base for RAU's LeGIT content development system. It includes comprehensive guidance for creating skills-based learning content using Markdown, from instructional design through publication.

## What is This?

RAU LeGIT is a **skills-based content development system** that enables SMEs to author atomic learning content in Markdown, store it in Git, and publish to multiple formats (PDFs, presentations, SCORM modules, videos).

This knowledge base provides:
- **Content Design Framework**: ABCD outcomes, objectives, delivery strategy, coverage planning
- **Content Development Guide**: Markdown authoring, content blocks, build configuration
- **Validation Rules**: Claude Code checks for instructional alignment and completeness
- **Integration Patterns**: How to use this in your content repositories

## Quick Start

**I'm creating new training content:**
1. Start with [`CLAUDE.md`](CLAUDE.md): Read Section 0: Content Design Phase
2. Follow [`docs/design/content-design-process.md`](docs/design/content-design-process.md): Complete design before development
3. Use [`docs/design/file-mapping-guide.md`](docs/design/file-mapping-guide.md): Map design to markdown files
4. Ask Claude Code to validate: **"Validate my content design"**

**I'm integrating this into a content repository:**
1. Add as a git submodule: `git submodule add https://github.com/RAU-EIT/rau-legit-knowledge.git .claude/shared-knowledge`
2. Create symlink to rules: `ln -s ../shared-knowledge/.claude/rules/* .claude/rules/`
3. Import CLAUDE.md in your project's `.claude/CLAUDE.md`
4. Done. SMEs now have access to shared knowledge through Claude Code

## Key Files

| File | Purpose |
|------|---------|
| [`CLAUDE.md`](CLAUDE.md) | Main knowledge index (Sections 0-7) |
| [`docs/terminology-glossary.md`](docs/terminology-glossary.md) | Definitions of key terms: Offering, Publication, Deliverable, etc. |
| [`docs/design/content-design-process.md`](docs/design/content-design-process.md) | Step-by-step guide for defining outcomes, objectives, delivery strategy, coverage |
| [`docs/design/file-mapping-guide.md`](docs/design/file-mapping-guide.md) | How design decisions translate to markdown file structure |
| [`.claude/rules/content-design-validation.md`](.claude/rules/content-design-validation.md) | Design validation rules Claude Code enforces |
| [`.claude/rules/legit-*.md`](.claude/rules/) | Rules for markdown, YAML, blocks, presentations (coming) |
| [`docs/development/legit-fundamentals.md`](docs/development/legit-fundamentals.md) | LeGIT system architecture (coming) |
| [`docs/development/content-blocks-reference.md`](docs/development/content-blocks-reference.md) | All 20+ content blocks with syntax and examples (coming) |

## The Content Design Phase

Before writing any markdown, content must be designed. Design **derives top-down**: each decision determines the next:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
```

The build system later runs this chain in reverse. See [The Two Directions](docs/terminology-glossary.md#the-two-directions) for why both matter.

**The nine steps:**

1. **Define Learning Outcomes** (ABCD method)
   - Audience: Who is learning?
   - Behavior: What observable action demonstrates learning?
   - Condition: Under what circumstances?
   - Degree: To what standard?

2. **Define Learning Objectives** (step-level actions supporting outcomes)
   - Break outcomes into 2-5 specific steps
   - Mark which objectives are standalone

3. **Determine Delivery Strategy** (e-learning, classroom, or blended)
   - Six-factor decision framework
   - The first link in the derivation chain

4. **Define Offerings** (what students enroll in, per audience/role)
   - The same skill often serves different roles differently

5. **Define Publications** (what the build system must produce)
   - Tagged with audience/role, scope, and publication type
   - Scoped by the design hierarchy

6. **Determine Activities** (passive + interactive + assessment)
   - Derived from the publications
   - Every outcome must have all three activity types

7. **Define Deliverables** (activities + supporting assets)
   - Most are authored content
   - Some (VMs, project files) are not authored by LeGIT but are still required

8. **Validate & Refine Design** (Claude Code + Aba Azeem escalation)
   - Ask Claude Code: "Validate my content design"
   - The team reviews the generated set and can add, change, or remove
   - Escalates complex questions to Aba Azeem (RAU's global instructional design lead)

9. **Load into Content Database**
   - The database creates the LeGIT project files and exports deliverables to DevOps
   - *Transitional*: until the database is live, the CDD Workbook remains the interim record

See [`docs/design/content-design-process.md`](docs/design/content-design-process.md) for the complete step-by-step guide, and [`docs/terminology-glossary.md`](docs/terminology-glossary.md) for what each term means.

## Integration with Your Content Repository

### Option 1: Add as Git Submodule (Recommended)

```bash
cd your-content-repo
git submodule add https://github.com/RAU-EIT/rau-legit-knowledge.git .claude/shared-knowledge
cd .claude
ln -s shared-knowledge/.claude/rules/* rules/  # Symlink rules for auto-loading
```

Then in your `.claude/CLAUDE.md`:
```markdown
# Your Project Name

[Project-specific context]

## Shared RAU Standards

@.claude/shared-knowledge/CLAUDE.md
```

### Option 2: Clone and Copy (If Submodules Not Ideal)

```bash
git clone https://github.com/RAU-EIT/rau-legit-knowledge.git
cp -r rau-legit-knowledge/docs your-repo/.claude/shared-docs
cp -r rau-legit-knowledge/.claude/rules your-repo/.claude/shared-rules
```

## Validation & Escalation

### Claude Code Validation

SMEs can ask Claude Code to validate their designs:

```text
"Validate my content design"
```

Claude Code checks:

1. ✅ ABCD outcome completeness
2. ✅ Objective-to-outcome alignment
3. ✅ Coverage completeness (Passive + Interactive + Assessment)
4. ✅ Standalone objective designation
5. ✅ **Delivery Strategy & Deliverable Alignment**: the whole chain: strategy → offerings → publications → activities → deliverables
6. ✅ Deliverable file completeness

Rule 7 (publication-to-activity derivation) is a placeholder and is **not enforced yet**.

### Escalation to Aba Azeem

For complex design questions that Claude Code can't resolve:

**Contact**: <aba.azeem@rockwellautomation.com>
**Response Time**: 1-2 business days

Claude Code will automatically escalate with context when needed.

## File Structure

```text
rau-legit-knowledge/
├── CLAUDE.md                                # Main knowledge index
├── README.md                                # This file
├── QUICK_REFERENCE_CARD.md                  # One-page overview
├── .claude/
│   ├── rules/
│   │   ├── content-design-validation.md     # Design validation rules
│   │   ├── legit-markdown-standards.md      # Markdown rules
│   │   ├── legit-blocks.md                  # Content block rules
│   │   ├── legit-yaml.md                    # YAML rules
│   │   └── legit-presentations.md           # Presentation rules
│   └── skills/                              # /design-training, /develop-training, sync
├── docs/
│   ├── terminology-glossary.md              # ★ Source of truth for all terms
│   ├── design/
│   │   ├── content-design-process.md        # The 9 design steps
│   │   └── file-mapping-guide.md            # Deliverable file contract
│   ├── development/
│   │   ├── legit-fundamentals.md            # System architecture
│   │   ├── markdown-standards.md            # Markdown authoring
│   │   ├── yaml-guide.md                    # YAML reference
│   │   ├── content-blocks-reference.md      # All blocks with examples
│   │   ├── presentations.md                 # Presentation authoring
│   │   └── best-practices.md                # Design philosophy
│   └── repo-info/
│       ├── CLAUDE_REORGANIZED.md            # Design + Development tracks
│       ├── DOCUMENTATION_STRUCTURE.md
│       ├── KEEPING_DOCS_IN_SYNC.md          # docs ↔ rules sync process
│       ├── REPO_STRUCTURE.md
│       └── sme-workflows.md                 # Workflow tutorials
├── specifications/
│   └── CLAUDE_CODE_SKILLS_SPEC.md           # Skill specifications
└── archive/                                 # Historical phase reports
```

## Maintaining This Knowledge Base

To update shared knowledge:

1. Edit files in this repository
2. Commit and push to `main`
3. All projects with this as a submodule pull the updates via `git submodule update --remote`

## Questions?

- **Design questions** → See CLAUDE.md Section 0 or ask Claude Code
- **Content block questions** → See CLAUDE.md Section 6 or ask Claude Code
- **File structure questions** → See `docs/design/file-mapping-guide.md` or ask Claude Code
- **Complex design questions** → Ask Claude Code to escalate to Aba Azeem

---

**Repository**: https://github.com/RAU-EIT/rau-legit-knowledge  
**Maintained by**: pfranci@rockwellautomation.com  
**Last Updated**: 2026-08-14
