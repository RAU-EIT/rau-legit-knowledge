# RAU LeGIT Knowledge Base

This repository contains the shared knowledge base for RAU's LeGIT content development system. It includes comprehensive guidance for creating skills-based learning content using Markdown, from instructional design through publication.

## What is This?

RAU LeGIT is a **skills-based content development system** that enables SMEs to author atomic learning content in Markdown, store it in Git, and publish to multiple formats (PDFs, presentations, SCORM modules, videos).

This knowledge base provides:
- **Content Design Framework** — ABCD outcomes, objectives, modality selection, coverage planning
- **Content Development Guide** — Markdown authoring, content blocks, build configuration
- **Validation Rules** — Claude Code checks for instructional alignment and completeness
- **Integration Patterns** — How to use this in your content repositories

## Quick Start

**I'm creating new training content:**
1. Start with [`CLAUDE.md`](CLAUDE.md) — Read Section 0: Content Design Phase
2. Follow [`docs/content-design-process.md`](docs/content-design-process.md) — Complete design before development
3. Use [`docs/file-mapping-guide.md`](docs/file-mapping-guide.md) — Map design to markdown files
4. Ask Claude Code to validate: **"Validate my content design"**

**I'm integrating this into a content repository:**
1. Add as a git submodule: `git submodule add https://github.com/RAU-EIT/rau-legit-knowledge.git .claude/shared-knowledge`
2. Create symlink to rules: `ln -s ../shared-knowledge/.claude/rules/* .claude/rules/`
3. Import CLAUDE.md in your project's `.claude/CLAUDE.md`
4. Done — SMEs now have access to shared knowledge through Claude Code

## Key Files

| File | Purpose |
|------|---------|
| [`CLAUDE.md`](CLAUDE.md) | Main knowledge index (11 comprehensive sections) |
| [`docs/terminology-glossary.md`](docs/terminology-glossary.md) | Definitions of key terms: Offering, Publication, Deliverable, etc. |
| [`docs/content-design-process.md`](docs/content-design-process.md) | Step-by-step guide for defining outcomes, objectives, offering strategy, coverage |
| [`docs/file-mapping-guide.md`](docs/file-mapping-guide.md) | How design decisions translate to markdown file structure |
| [`.claude/rules/content-design-validation.md`](.claude/rules/content-design-validation.md) | Design validation rules Claude Code enforces |
| [`.claude/rules/legit-*.md`](.claude/rules/) | Rules for markdown, YAML, blocks, presentations (coming) |
| [`docs/legit-fundamentals.md`](docs/legit-fundamentals.md) | LeGIT system architecture (coming) |
| [`docs/content-blocks-reference.md`](docs/content-blocks-reference.md) | All 20+ content blocks with syntax and examples (coming) |

## The Content Design Phase

Before writing any markdown, content must be designed:

1. **Define Learning Outcomes** (ABCD method)
   - Audience: Who is learning?
   - Behavior: What observable action demonstrates learning?
   - Condition: Under what circumstances?
   - Degree: To what standard?

2. **Define Learning Objectives** (step-level actions supporting outcomes)
   - Break outcomes into 2-5 specific steps
   - Each objective needs core lecture content

3. **Determine Offering Strategy** (e-learning, classroom, or blended)
   - Decision tree: Can learner be competent without practice?
   - Recommendations by audience type and skill complexity

4. **Plan Activity Coverage** (passive + interactive + assessment)
   - Every outcome must have all three activity types
   - Coverage can be at outcome level or aggregated from objectives

5. **Validate Design** (Claude Code + Aba Azeem escalation)
   - Ask Claude Code: "Validate my content design"
   - Claude checks 6 validation rules
   - Escalates complex questions to Aba Azeem (RAU's global instructional design lead)

See [`docs/content-design-process.md`](docs/content-design-process.md) for complete step-by-step guide.

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

```
"Validate my content design"
```

Claude Code checks:
- ✅ ABCD outcome completeness
- ✅ Objective-to-outcome alignment
- ✅ Coverage matrix completeness (Passive + Interactive + Assessment)
- ✅ Standalone objective designation
- ✅ Offering strategy-deliverable alignment
- ✅ File mapping completeness

### Escalation to Aba Azeem

For complex design questions that Claude Code can't resolve:

**Contact**: aba.azeem@rockwellautomation.com  
**Response Time**: 1-2 business days

Claude Code will automatically escalate with context when needed.

## File Structure

```
rau-legit-knowledge/
├── CLAUDE.md                           # Main knowledge index
├── README.md                           # This file
├── .claude/
│   ├── settings.json                   # Context loading configuration
│   └── rules/
│       ├── content-design-validation.md   # Design validation rules
│       ├── legit-markdown-standards.md    # Markdown rules (coming)
│       ├── legit-blocks.md               # Content block rules (coming)
│       ├── legit-yaml.md                 # YAML rules (coming)
│       └── legit-presentations.md        # Presentation rules (coming)
└── docs/
    ├── content-design-process.md      # Step-by-step design guide
    ├── file-mapping-guide.md          # Design-to-files translation
    ├── legit-fundamentals.md          # System architecture (coming)
    ├── content-blocks-reference.md    # All blocks with examples (coming)
    ├── yaml-guide.md                  # YAML reference (coming)
    ├── sme-workflows.md               # Workflow tutorials (coming)
    └── best-practices.md              # Design philosophy (coming)
```

## Maintaining This Knowledge Base

To update shared knowledge:

1. Edit files in this repository
2. Commit and push to `main`
3. All projects with this as a submodule pull the updates via `git submodule update --remote`

## Questions?

- **Design questions** → See CLAUDE.md Section 0 or ask Claude Code
- **Content block questions** → See CLAUDE.md Section 6 or ask Claude Code
- **File structure questions** → See `docs/file-mapping-guide.md` or ask Claude Code
- **Complex design questions** → Ask Claude Code to escalate to Aba Azeem

---

**Repository**: https://github.com/RAU-EIT/rau-legit-knowledge  
**Maintained by**: pfranci@rockwellautomation.com  
**Last Updated**: 2026-08-14
