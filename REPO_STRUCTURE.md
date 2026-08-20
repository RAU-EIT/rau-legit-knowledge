# RAU LeGIT Knowledge Base - Repository Structure

## Clean Repository (Post-Cleanup)

**Cleaned**: 2026-08-20  
**Status**: Ready for use  
**Focus**: Two Claude Code skills (Design + Development)

---

## Root Files

```
rau-legit-knowledge/
├── CLAUDE.md                          (Main entry point - redirects to new structure)
├── CLAUDE_REORGANIZED.md              (⭐ NEW: Two-track knowledge base)
├── CLAUDE_CODE_SKILLS_SPEC.md         (⭐ NEW: Skill specifications)
├── PHASE_1_2_SUMMARY.md               (⭐ NEW: Leadership summary)
├── QUICK_REFERENCE_CARD.md            (⭐ NEW: One-page overview)
├── README.md                          (Project README)
├── POPULATION_SUMMARY.md              (Historical: how docs were populated)
└── .claude/
    └── rules/                         (Validation rules)
```

---

## What Each Root File Does

### Entry Points

**`CLAUDE.md`** - Main entry point  
→ Directs users to new two-track structure  
→ Links to CLAUDE_REORGANIZED.md, QUICK_REFERENCE_CARD.md

**`QUICK_REFERENCE_CARD.md`** - One-page overview  
→ Visual diagram of workflow  
→ Benefits summary  
→ Links to full documentation

**`README.md`** - Project information  
→ How to integrate this repo as submodule  
→ Link to GitHub repository

---

### Reorganized Knowledge Base

**`CLAUDE_REORGANIZED.md`** ⭐ NEW - Complete reorganization  
→ **Content Design Track**
  - 7-step design process
  - 6 validation rules
  - Key concepts & resources
  
→ **Content Development Track**
  - Content types & standards
  - LeGIT technical standards
  - File organization
  - Development workflow

→ **FAQ & Resources**
  - When to use which track
  - Links to all documentation

**Size**: ~3,500 words  
**Status**: Complete, ready to replace old CLAUDE.md

---

### Implementation Plans

**`CLAUDE_CODE_SKILLS_SPEC.md`** ⭐ NEW - Skill specifications  
→ **Skill 1: /design-training**
  - Interactive design workflow
  - Design JSON schema
  - Example outputs
  
→ **Skill 2: /develop-training**
  - Interactive authoring workflow
  - Real translation examples (PPT, documentation)
  - Validation checklist
  
→ **Integration & Timeline**
  - How skills work together
  - 6-8 week implementation roadmap

**Size**: ~5,000 words  
**Status**: Complete, ready for development

---

**`PHASE_1_2_SUMMARY.md`** ⭐ NEW - Leadership summary  
→ What was completed in Phase 1 & 2
→ QA checklist (validation)
→ Next steps decision matrix
→ Resource estimates & timeline
→ 5 questions for leadership review

**Size**: ~2,500 words  
**Status**: Complete, ready to present

---

## Documentation Directories

### `/docs/` - Comprehensive Guides

**Design-Related**:
- `content-design-process.md` - Step-by-step design guide
- `file-mapping-guide.md` - File structure conventions

**Development-Related**:
- `legit-markdown-standards.md` - Markdown writing rules
- `legit-yaml.md` - YAML frontmatter guide
- `content-blocks-reference.md` - Content block syntax
- `legit-presentations.md` - Presentation standards
- `best-practices.md` - General best practices

**Fundamentals**:
- `legit-fundamentals.md` - System overview

**Total**: 8 comprehensive guides

---

### `/.claude/rules/` - Validation Rules

**Design Rules**:
- `content-design-validation.md` - 6 design validation rules

**Development Rules**:
- `legit-blocks.md` - Content block validation
- `legit-markdown-standards.md` - Markdown validation
- `legit-yaml.md` - YAML validation
- `legit-presentations.md` - Presentation validation

**Total**: 5 validation rule files

---

## Removed Files

The following files have been **removed** as they're no longer part of the new direction:

- ❌ `AUTOMATION_ROADMAP.md` - Old 5-phase plan (superseded)
- ❌ `AUTOMATION_SUMMARY.md` - Old summary (superseded)
- ❌ `GAP_ANALYSIS.md` - Old gap analysis (superseded)
- ❌ `WORKFLOW_COMPARISON.md` - Old workflow diagrams (superseded)

**Reason**: Replaced by focused two-skill approach  
**New Direction**: CLAUDE_CODE_SKILLS_SPEC.md defines the path forward

---

## Repository Overview

### Purpose
RAU's shared knowledge base for designing and developing skills-based learning content using the LeGIT system.

### Key Sections
1. **Content Design** - How to design learning outcomes, objectives, and activities
2. **Content Development** - How to author content in LeGIT format
3. **Validation Rules** - Automated checks for quality assurance
4. **LeGIT Fundamentals** - System overview and architecture

### Use Cases
- SMEs designing new training programs
- Content developers authoring skills content
- Teams validating their work against standards
- Organizations implementing the LeGIT system

### Integration
This repo is typically integrated as a **git submodule** in content projects:
```bash
git submodule add https://github.com/RAU-EIT/rau-legit-knowledge.git .claude/shared-knowledge
```

---

## Quick Links

| Need | Resource |
|------|----------|
| Quick overview | [QUICK_REFERENCE_CARD.md](./QUICK_REFERENCE_CARD.md) |
| Design guidance | [CLAUDE_REORGANIZED.md - Design Track](./CLAUDE_REORGANIZED.md#content-design-guide) |
| Development guidance | [CLAUDE_REORGANIZED.md - Development Track](./CLAUDE_REORGANIZED.md#content-development-guide) |
| Skill specs | [CLAUDE_CODE_SKILLS_SPEC.md](./CLAUDE_CODE_SKILLS_SPEC.md) |
| Leadership summary | [PHASE_1_2_SUMMARY.md](./PHASE_1_2_SUMMARY.md) |
| Design process | [docs/content-design-process.md](./docs/content-design-process.md) |
| Markdown standards | [docs/legit-markdown-standards.md](./docs/legit-markdown-standards.md) |
| Content blocks | [docs/content-blocks-reference.md](./docs/content-blocks-reference.md) |
| System overview | [docs/legit-fundamentals.md](./docs/legit-fundamentals.md) |

---

## File Statistics

**Root Files**: 7 files
- Entry points: 2
- New documentation: 4
- Reference: 1

**Documentation Guides**: 8 files  
**Validation Rules**: 5 files  
**Total Markdown**: 20 files

**Total Size**: ~120 KB (highly efficient, text-based)

---

## Status

✅ **Repository Cleaned**  
✅ **New Structure Ready**  
✅ **Documentation Complete**  
✅ **Specifications Finalized**  
✅ **Ready for Phase 3 Implementation**

---

**Last Updated**: 2026-08-20  
**Repository**: RAU LeGIT Knowledge Base  
**Status**: Clean, organized, ready for use
