# RAU LeGIT Documentation Structure

**Purpose**: Clarify the organization of documentation files and their purposes.

---

## The Three Layers

### Layer 1: Entry Points (Root Files)

**Location**: Repository root  
**Purpose**: Help users find what they need

**Files**:
- **`CLAUDE.md`** - Main entry point, redirects to new two-track structure
- **`CLAUDE_REORGANIZED.md`** - The actual two-track knowledge base (Design + Development)
- **`QUICK_REFERENCE_CARD.md`** - One-page visual overview
- **`README.md`** - Project information and integration instructions

**When to use**:
- Start here if you're new
- These files guide you to the right folder

---

### Layer 2: Human-Readable Guides (`docs/` folder)

**Location**: `docs/` subdirectories  
**Purpose**: Comprehensive, instructional guides for learning how to design and develop content

#### **`docs/design/`** - Learning Design Guides
Guides for the DESIGN phase (deciding what learners will do)

**Files**:
- `content-design-process.md` - Step-by-step design process (7 steps + ABCD method)
- `file-mapping-guide.md` - How design maps to file structure

**When to read**: You're designing new training content

---

#### **`docs/development/`** - Content Development Guides
Guides for the DEVELOPMENT phase (how to author content in LeGIT)

**Files**:
- `content-blocks-reference.md` - What content blocks exist and how to use them
- `best-practices.md` - General best practices for writing and structure
- `yaml-guide.md` - How to write YAML frontmatter in files
- `markdown-standards.md` - Markdown formatting and structure standards
- `presentations.md` - Reveal.js presentation standards and structure

**When to read**: You're authoring content files

---

#### **`docs/` (Shared)** - Foundational References
Guides that apply to both design and development

**Files**:
- `legit-fundamentals.md` - System overview, architecture, project structure
- `sme-workflows.md` - Complete content development lifecycle

**When to read**: You need to understand the overall LeGIT system

---

### Layer 3: Validation Rules (`.claude/rules/` folder)

**Location**: `./.claude/rules/`  
**Purpose**: Machine-readable validation rules (used by automation, Claude Code, etc.)

**These are rules that:**
- Enforce quality standards
- Check designs against requirements
- Validate files during authoring
- Are referenced by Claude Code skills

**Files**:
- **`content-design-validation.md`** - The 6 design validation rules (ABCD completeness, objective alignment, coverage, etc.)
- **`legit-blocks.md`** - Content block syntax and validation rules
- **`legit-markdown-standards.md`** - Markdown formatting rules
- **`legit-yaml.md`** - YAML frontmatter requirements
- **`legit-presentations.md`** - Presentation-specific rules

**When to use**: 
- Claude Code references these for validation
- These are enforced by automation
- SMEs read these to understand requirements

---

## How They Work Together

```
START: User reads entry points
  ↓
CHOOSE PATH: Design or Development
  ↓
READ GUIDE: From docs/design/ or docs/development/
  ↓
UNDERSTAND RULES: Check .claude/rules/ for validation requirements
  ↓
CREATE/AUTHOR: Do the work
  ↓
VALIDATE: Run through .claude/rules/ checks
```

---

## The File Relationship

```
CLAUDE.md (entry)
  ↓
  ├─→ docs/repo-info/CLAUDE_REORGANIZED.md (two-track structure)
  ├─→ QUICK_REFERENCE_CARD.md (visual overview)
  ├─→ README.md (project info)
  └─→ docs/
        ├─→ design/
        │     ├─→ content-design-process.md
        │     └─→ file-mapping-guide.md
        ├─→ development/
        │     ├─→ content-blocks-reference.md
        │     ├─→ best-practices.md
        │     ├─→ yaml-guide.md
        │     ├─→ markdown-standards.md
        │     ├─→ presentations.md
        │     └─→ legit-fundamentals.md
        └─→ repo-info/
              ├─→ CLAUDE_REORGANIZED.md
              ├─→ DOCUMENTATION_STRUCTURE.md
              ├─→ REPO_STRUCTURE.md
              ├─→ KEEPING_DOCS_IN_SYNC.md
              └─→ sme-workflows.md

specifications/ (technical specs)
  └─→ CLAUDE_CODE_SKILLS_SPEC.md

archive/ (historical records)
  ├─→ PHASE_1_2_SUMMARY.md
  ├─→ PHASE_4_IMPLEMENTATION_STATUS.md
  ├─→ PHASE_4_SUMMARY.md
  ├─→ [and other phase/sync reports...]
  └─→ SYNC_REPORT_2026-08-20.md

.claude/rules/ (validation)
  ├─→ content-design-validation.md
  ├─→ legit-blocks.md
  ├─→ legit-markdown-standards.md
  ├─→ legit-yaml.md
  └─→ legit-presentations.md
```

---

## What Goes Where: Quick Reference

| Content | Location | Purpose |
|---------|----------|---------|
| System overview | `docs/development/legit-fundamentals.md` | Understanding the big picture |
| Design process + steps | `docs/design/content-design-process.md` | Learning how to design |
| How design maps to files | `docs/design/file-mapping-guide.md` | Understanding file structure |
| Content block syntax | `docs/development/content-blocks-reference.md` | Using blocks in content |
| Markdown writing standards | `docs/development/markdown-standards.md` (guide) → `.claude/rules/legit-markdown-standards.md` (rules) | Learning + validation |
| YAML frontmatter | `docs/development/yaml-guide.md` (guide) → `.claude/rules/legit-yaml.md` (rules) | Learning + validation |
| Presentation standards | `docs/development/presentations.md` (guide) → `.claude/rules/legit-presentations.md` (rules) | Learning + validation |
| SME workflows | `docs/repo-info/sme-workflows.md` | Content development lifecycle |
| Design validation rules | `.claude/rules/content-design-validation.md` | Validating designs |
| Content block validation | `.claude/rules/legit-blocks.md` | Validating block syntax |
| General best practices | `docs/development/best-practices.md` | Overall quality principles |
| Repository structure info | `docs/repo-info/REPO_STRUCTURE.md` | Understanding project layout |
| Documentation sync | `docs/repo-info/KEEPING_DOCS_IN_SYNC.md` | Maintaining docs-to-rules sync |

---

## Current Issues & Recommendations

### ✅ RESOLVED: Missing Development Guides

**Status**: Fixed (2026-08-25)

Guide files created in `docs/development/`:

- **`markdown-standards.md`** — Guide to markdown standards with reference link to validation rules
- **`presentations.md`** — Guide to presentation standards with reference link to validation rules

**Approach**:

- Guides serve as discoverable entry points in `docs/development/`
- Authoritative validation rules remain in `.claude/rules/` (source of truth)
- Guides reference the detailed rules files to avoid duplication
- Developers can find standards naturally by browsing `docs/development/`
- Single source of truth reduces maintenance burden

---

## Final Structure (Current)

```
docs/
├── design/
│   ├── content-design-process.md          (learning design, 7 steps)
│   └── file-mapping-guide.md              (design to file mapping)
├── development/
│   ├── content-blocks-reference.md        (block syntax + usage)
│   ├── best-practices.md                  (general practices)
│   ├── yaml-guide.md                      (YAML frontmatter)
│   ├── markdown-standards.md              (markdown guide → links to rules)
│   └── presentations.md                   (presentation guide → links to rules)
├── legit-fundamentals.md                  (shared - system overview)
└── sme-workflows.md                       (shared - full lifecycle)

.claude/rules/                             (validation rules - source of truth)
├── content-design-validation.md           (design rules)
├── legit-blocks.md                        (block validation)
├── legit-markdown-standards.md            (markdown validation)
├── legit-yaml.md                          (YAML validation)
└── legit-presentations.md                 (presentation validation)
```

**Approach**:

- `.claude/rules/` remains the **authoritative source of truth** for all validation
- `docs/development/` provides **discoverable guide entry points** that link to rules
- Avoids duplication while maintaining single source of truth
- Developers find guides naturally when browsing `docs/development/`

---

## Summary

| Location                | Purpose                          | Used By                        | Examples                                                             |
| ----------------------- | -------------------------------- | ------------------------------ | -------------------------------------------------------------------- |
| Root (3 files)          | Entry points                     | Users choosing path            | CLAUDE.md, README.md, QUICK_REFERENCE_CARD.md                        |
| `docs/design/`          | Design guides                    | Instructional designers        | content-design-process.md, file-mapping-guide.md                     |
| `docs/development/`     | Development guides & references  | Content authors                | markdown-standards.md, presentations.md, yaml-guide.md               |
| `docs/repo-info/`       | Repository documentation         | Everyone                       | CLAUDE_REORGANIZED.md, DOCUMENTATION_STRUCTURE.md, sme-workflows.md  |
| `specifications/`       | Technical specifications         | Developers                     | CLAUDE_CODE_SKILLS_SPEC.md                                           |
| `archive/`              | Historical project records       | Project history                | Phase summaries, sync reports                                        |
| `.claude/rules/`        | Validation rules (source truth)  | Claude Code, automation        | content-design-validation.md, legit-markdown-standards.md            |

---

**Status**: Documentation reorganized (2026-08-20)  
**Next Step**: Consolidate missing files from `.claude/rules/` into `docs/development/`
