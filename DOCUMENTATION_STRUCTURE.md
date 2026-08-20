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
- (Missing files that should be here): legit-markdown-standards.md, legit-presentations.md

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
  ├─→ CLAUDE_REORGANIZED.md (two-track structure)
  ├─→ QUICK_REFERENCE_CARD.md (visual overview)
  └─→ docs/
        ├─→ design/
        │     ├─→ content-design-process.md
        │     └─→ file-mapping-guide.md
        ├─→ development/
        │     ├─→ content-blocks-reference.md
        │     ├─→ best-practices.md
        │     └─→ yaml-guide.md
        └─→ legit-fundamentals.md

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
| System overview | `docs/legit-fundamentals.md` | Understanding the big picture |
| Design process + steps | `docs/design/content-design-process.md` | Learning how to design |
| How design maps to files | `docs/design/file-mapping-guide.md` | Understanding file structure |
| Content block syntax | `docs/development/content-blocks-reference.md` | Using blocks in content |
| Markdown writing rules | `.claude/rules/legit-markdown-standards.md` | Validation + enforcing standards |
| YAML frontmatter | `docs/development/yaml-guide.md` + `.claude/rules/legit-yaml.md` | Writing + validation |
| Presentation rules | `.claude/rules/legit-presentations.md` | Validation for presentations |
| Design validation rules | `.claude/rules/content-design-validation.md` | Validating designs |
| Content block validation | `.claude/rules/legit-blocks.md` | Validating block syntax |
| General best practices | `docs/development/best-practices.md` | Overall quality principles |

---

## Current Issues & Recommendations

### Issue 1: Duplicate Files
Some content appears in BOTH `docs/` (human guide) AND `.claude/rules/` (validation rule):
- **`legit-markdown-standards.md`**: Only in `.claude/rules/` (missing from `docs/development/`)
- **`legit-presentations.md`**: Only in `.claude/rules/` (missing from `docs/development/`)

**Recommendation**: Copy these to `docs/development/` so they're discoverable in the "development guides" section.

---

### Issue 2: Unclear Separation
The files in `.claude/rules/` serve two purposes:
1. **Validation rules** (machine-readable for enforcement)
2. **Reference guides** (human-readable for understanding requirements)

**Examples**:
- `content-design-validation.md` explains the 6 rules AND is used for validation
- `legit-yaml.md` explains YAML requirements AND enforces them

**Recommendation**: Keep them where they are (.claude/rules/) since they serve both purposes. The key is that `docs/development/` should REFERENCE these rules, not duplicate them.

---

### Issue 3: Missing Development Guides
Some standards that should have guides in `docs/development/` are only in `.claude/rules/`:
- Markdown standards guide (only in rules, not in docs)
- Presentation standards guide (only in rules, not in docs)

**Recommendation**: These should be copied/linked from `docs/development/` so new developers can find them naturally.

---

## Proposed Structure (Post-Consolidation)

```
docs/
├── design/
│   ├── content-design-process.md          (learning design, 7 steps)
│   └── file-mapping-guide.md              (design to file mapping)
├── development/
│   ├── content-blocks-reference.md        (block syntax + usage)
│   ├── best-practices.md                  (general practices)
│   ├── yaml-guide.md                      (YAML frontmatter)
│   ├── markdown-standards.md               ⭐ (markdown rules - COPY from .claude/rules/)
│   └── presentations.md                    ⭐ (presentation rules - COPY from .claude/rules/)
├── legit-fundamentals.md                  (shared - system overview)
└── sme-workflows.md                       (shared - full lifecycle)

.claude/rules/                             (validation rules - source of truth)
├── content-design-validation.md           (design rules)
├── legit-blocks.md                        (block validation)
├── legit-markdown-standards.md            (markdown validation)
├── legit-yaml.md                          (YAML validation)
└── legit-presentations.md                 (presentation validation)
```

**Notes**:
- `.claude/rules/` remains the source of truth for validation
- `docs/development/` gets copies so they're discoverable as guides
- Both can exist; they serve different purposes (guide vs. validation)

---

## Summary

| Folder | Purpose | Used By | Examples |
|--------|---------|---------|----------|
| Root files | Entry points | Users choosing path | CLAUDE.md, QUICK_REFERENCE_CARD.md |
| `docs/design/` | Design guides | Instructional designers | content-design-process.md |
| `docs/development/` | Development guides | Content authors | content-blocks-reference.md |
| `docs/` (shared) | System fundamentals | Everyone | legit-fundamentals.md |
| `.claude/rules/` | Validation rules | Claude Code, automation | content-design-validation.md |

---

**Status**: Documentation reorganized (2026-08-20)  
**Next Step**: Consolidate missing files from `.claude/rules/` into `docs/development/`
