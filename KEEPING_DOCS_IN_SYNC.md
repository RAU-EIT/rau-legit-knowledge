# Keeping Documentation in Sync with Claude Rules

**Problem**: When we update guides in `docs/`, the `.claude/rules/` files that Claude Code actually uses may not be updated, causing Claude to use outdated information.

**Example**: We just updated the delivery modality decision framework in `docs/content-design-process.md`, but the Claude Code skills use `.claude/rules/content-design-validation.md`. If they're not kept in sync, Claude will use old rules.

---

## The Sync Challenge

```
Two Versions of Truth Problem:

docs/design/
  └── content-design-process.md (UPDATED with new decision framework)
                ↓
          SME reads this ✅
          But Claude Code reads...
                ↓
.claude/rules/
  └── content-design-validation.md (OLD decision framework)
                ↓
          Claude Code uses this ❌ (OUTDATED!)
```

---

## Recommended Solution: Source of Truth Approach

### **Step 1: Establish `.claude/rules/` as the Primary Source**

**Principle**: `.claude/rules/` files are the source of truth for Claude Code skills.

- Claude Code reads `.claude/rules/` to validate work
- These are the actual rules that will be enforced
- These should be updated FIRST

### **Step 2: Create a One-Way Sync Process**

**Flow**:
```
UPDATE: Change .claude/rules/ file (the rule)
  ↓
DERIVE: Documentation in docs/ is based on the rule
  ↓
REFERENCE: docs/ file points back to .claude/rules/
```

### **Step 3: Create a Sync Checklist**

Every time you update a rule, follow this checklist:

**When updating `.claude/rules/content-design-validation.md`:**
- [ ] Update the rule in `.claude/rules/`
- [ ] Update the corresponding section in `docs/design/content-design-process.md` with the NEW framework
- [ ] Add a cross-reference link from docs back to the rule: "For the authoritative validation rules, see `./.claude/rules/content-design-validation.md`"
- [ ] Update any related files (e.g., CLAUDE_REORGANIZED.md if it references this)
- [ ] Create a git commit with message: "sync: update [rule name] and documentation"

---

## Three Implementation Options

### **Option A: Manual Sync with Checklist** (Simple, Low Cost)

**How it works**:
- Keep `.claude/rules/` and `docs/` separate
- Create a checklist that must be followed when updating
- Add a git hook that reminds you to check both places
- Document the sync process clearly

**Pros**:
- Simple to implement
- No tooling needed
- Clear responsibility (update rules first, then docs)

**Cons**:
- Relies on human discipline
- Can drift if checklist not followed
- Requires two edits per change

**Recommended for**: Near-term (now), while you have small documentation set

---

### **Option B: Include References** (Moderate Cost, Better Sync)

**How it works**:
- Keep `.claude/rules/` as the authoritative version
- In `docs/` files, use `!include` directives to pull content from `.claude/rules/`
- This way, docs always show what Claude Code sees

**Example**:
```markdown
# Step 3: Publication Strategy

For the authoritative decision framework, see below:

!include(./.claude/rules/content-design-validation.md#decision-framework)

[Additional context and examples specific to this guide]
```

**Pros**:
- Single source of truth
- Docs automatically sync with rules
- No duplicate maintenance

**Cons**:
- Requires include mechanism to work
- May change document readability
- Initial setup effort to refactor files

**Recommended for**: Medium-term (when you have more rules/docs)

---

### **Option C: Automated Sync Check** (Higher Cost, Best Safety)

**How it works**:
- Keep `.claude/rules/` and `docs/` separate
- Create a pre-commit hook that checks for sync
- The hook compares version numbers or checksums
- Commit fails if docs and rules don't match

**Example Hook Logic**:
```bash
# Pre-commit hook
# Check if .claude/rules/ was modified
# If yes, ensure corresponding docs/ was also modified
# If not, fail commit with message:
# "Error: You updated .claude/rules/ but didn't update docs/
#  Please update both and try again."
```

**Pros**:
- Prevents accidental drift
- Catches sync issues at commit time
- Works with separate files

**Cons**:
- Requires hook development
- Can be annoying if frequently editing
- Needs to be set up for developers

**Recommended for**: Long-term (as documentation scales)

---

## Recommended Approach: **Hybrid (A + B)**

**Start with Option A** (Manual Checklist):
- Simple to implement immediately
- Creates discipline around sync
- Document the process clearly

**Plan for Option B** (Includes):
- As documentation grows, migrate to include mechanism
- Reduce maintenance burden
- Future-proof against drift

**Add Option C** (Hook) when:
- Documentation set is large (50+ files)
- Multiple maintainers
- Risk of drift becomes real

---

## Immediate Action: Sync the Decision Framework

**Current situation**: We updated `docs/content-design-process.md` Step 3 with the new decision framework, but `.claude/rules/content-design-validation.md` Rule 5 may still have old information.

**Action items**:
1. Review `.claude/rules/content-design-validation.md` Rule 5 (Modality-Deliverable Alignment)
2. Compare with new framework in `docs/content-design-process.md` Step 3
3. Update `.claude/rules/content-design-validation.md` to match the new framework
4. Add note in both files: "Last synced: 2026-08-20"
5. Create commit: "sync: update publication strategy decision framework (docs + rules)"

---

## Sync Process Template

**Create this checklist in your project workflow:**

### When Updating Rules

```
BEFORE EDITING:
☐ Identify which .claude/rules/ file needs updating
☐ Identify which docs/ files reference this rule
☐ Get the list of both

WHILE EDITING:
☐ Update .claude/rules/ version first (source of truth)
☐ Update docs/ versions second (reference/guide)
☐ Add sync date to both: "Last synced: YYYY-MM-DD"
☐ Review both for consistency

BEFORE COMMITTING:
☐ Read both versions side-by-side
☐ Verify docs explain the rule from .claude/rules/
☐ Check for links between them
☐ Create commit with pattern: "sync: update [topic] (docs + rules)"

TESTING:
☐ Run `/design-training` skill manually
☐ Verify it uses the new framework
☐ If creating new skills, test they reference updated rules
```

---

## File Mapping: Which Docs Link to Which Rules

Keep this up-to-date as you add new rules:

| `.claude/rules/` File | Referenced in `docs/` Files | Last Sync |
|---|---|---|
| `content-design-validation.md` | `docs/design/content-design-process.md` (Step 7) | TBD |
| `legit-yaml.md` | `docs/development/yaml-guide.md` | TBD |
| `legit-markdown-standards.md` | `docs/development/best-practices.md` | TBD |
| `legit-presentations.md` | `docs/development/presentations.md` (missing) | TBD |
| `legit-blocks.md` | `docs/development/content-blocks-reference.md` | TBD |

---

## For Claude Code Skills

**When Claude Code reads a rule**, it should get the single authoritative version:

```
/design-training skill
  ├─→ Reads: .claude/rules/content-design-validation.md ✅ (source of truth)
  └─→ Uses: 6 validation rules from this file

/develop-training skill
  ├─→ Reads: .claude/rules/legit-markdown-standards.md ✅ (source of truth)
  ├─→ Reads: .claude/rules/legit-yaml.md ✅ (source of truth)
  └─→ Uses: Standards from these files
```

**No reason for Claude Code to read `docs/` files directly** — those are for human guidance, not machine enforcement.

---

## Implementation: Pick Your Path

### **Path 1: Start Simple (Recommended for now)**

1. **Create a sync checklist** in your project workflow
2. **Add sync dates** to both `.claude/rules/` and `docs/` files
3. **Link from docs to rules**: "For the authoritative version, see `./.claude/rules/[filename]`"
4. **Document the sync process** in this file
5. **Follow the checklist** every time you update a rule

**Time to implement**: 1-2 hours  
**Maintenance burden**: Low (if checklist followed)  
**When to upgrade**: When you have 20+ rules/docs or multiple maintainers

### **Path 2: Automated (Better long-term)**

1. Create a pre-commit hook that validates sync
2. Hook checks: "If `.claude/rules/` changed, did docs also change?"
3. Developers must update both or commit fails

**Time to implement**: 4-6 hours  
**Maintenance burden**: Zero (automated)  
**When to implement**: After Path 1 proves stable

### **Path 3: Include-Based (Most elegant)**

1. Refactor `.claude/rules/` files to have marked sections
2. In `docs/` files, use `!include` to pull from `.claude/rules/`
3. Single source of truth, automatically in sync

**Time to implement**: 8-12 hours (refactoring work)  
**Maintenance burden**: None (automatic)  
**When to implement**: After documentation stabilizes

---

## Recommended Next Step

**Implement Path 1 immediately** (1-2 hours):

1. [ ] Create `DOCS_SYNC_PROCESS.md` with the checklist above
2. [ ] Review `.claude/rules/content-design-validation.md` vs. `docs/content-design-process.md` Step 3
3. [ ] Sync the decision framework (update both files)
4. [ ] Add sync date stamps to both
5. [ ] Create commit: "sync: update publication strategy decision framework"
6. [ ] Add this file to project workflow as required reading

**Plan Path 2** (low-priority, when you have time):

Document how pre-commit hooks would work, get it ready to implement later.

---

## Summary

**The principle**: `.claude/rules/` is the source of truth. `docs/` is the guide that explains it.

**The process**: 
1. Update `.claude/rules/` first
2. Update `docs/` second
3. Link them together
4. Test Claude Code uses the new version
5. Follow a checklist to ensure both are updated

**The benefit**: Claude Code stays in sync with your documentation, SMEs always see what Claude Code sees, no confusing gaps.

---

**Status**: Proposed sync strategy  
**Action**: Implement Path 1 immediately  
**Next**: Monitor sync discipline, plan Path 2/3 when ready
