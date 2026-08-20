---
name: sync-docs-to-rules
description: Validate and synchronize documentation files with validation rules, providing advisory recommendations
aliases: [sync, validate-sync, review-sync]
whenToUse: You've updated documentation and want to ensure .claude/rules/ stays in sync
---

# Sync Documentation to Rules

This skill keeps your documentation and validation rules synchronized, ensuring Claude Code always uses the latest guidance.

## What This Does

When you update files in `docs/`, the rules in `.claude/rules/` might become outdated. This skill:

1. **Detects changes** — Identifies which docs/ files were modified
2. **Compares versions** — Reads the updated docs and corresponding rules
3. **Identifies gaps** — Shows what's different
4. **Recommends updates** — Suggests specific changes to rules
5. **Provides rationale** — Explains why sync matters
6. **Awaits approval** — Team reviews and approves before applying

## Key Principle: Advisory, Not Enforcement

**What this skill DOES:**
- ✅ Show side-by-side comparisons (old vs new)
- ✅ Recommend specific rule updates
- ✅ Explain why each update matters
- ✅ Highlight conflicts needing team judgment
- ✅ Support multiple update strategies

**What this skill DOES NOT:**
- ❌ Auto-update rules without approval
- ❌ Enforce rigid interpretation
- ❌ Make strategic decisions for you
- ❌ Bypass team review process
- ❌ Assume rules should always match docs

---

## How to Use

### Automatic (On Git Commit)

When you commit changes to `docs/`:

```bash
git commit -m "Update decision framework in docs/"
# Git pre-commit hook runs automatically
# /sync-docs-to-rules detects changes
# Shows recommendations for review
```

### Manual (Audit Current State)

To check sync status anytime:

```
/sync-docs-to-rules
```

This will:
- Compare all docs/ with corresponding .claude/rules/
- Show overall sync status
- List any discrepancies

### Manual (With Focus)

To focus on specific rules:

```
/sync-docs-to-rules --focus content-design-validation.md
```

This will:
- Compare only docs related to this rule
- Show detailed differences
- Recommend specific updates

### Apply Approved Changes

After team reviews recommendations:

```
/sync-docs-to-rules --apply
```

This will:
- Apply approved changes to .claude/rules/
- Add sync date stamps
- Create commit documenting changes

---

## Example: Decision Framework Sync

### Scenario

You updated `docs/design/content-design-process.md` Step 3 with a new publication strategy decision framework. Now you want to sync the corresponding rule.

### Run Skill

```
/sync-docs-to-rules --focus content-design-validation.md
```

### Output

```
═══════════════════════════════════════════════════════════════
  DOCUMENTATION SYNC ANALYSIS
═══════════════════════════════════════════════════════════════

Changed Files:
  • docs/design/content-design-process.md (Step 3: Publication Strategy)

Comparison Results:

FILE: docs/design/content-design-process.md
SECTION: Step 3 - Publication Strategy Decision Framework

CHANGE DETECTED:
  Updated decision criteria from "Can learner become competent without practice?"
  to structured evaluation of 6 factors:
    1. Performance Type (Recall, Procedure, Problem-Solving, Complex)
    2. Instructor Involvement (None, Minimal, Moderate, Required)
    3. Audience Scale (Small, Medium, Large)
    4. Speed & Accessibility (Immediate, 1-2 weeks, Later)
    5. Hardware Requirements (Equipment, Simulation, None)
    6. Content Complexity (Simple, Moderate, Complex)

IMPACT ON RULES:
  File: .claude/rules/content-design-validation.md
  Rule: Rule 5 (Modality-Deliverable Alignment)
  Issue: Based on outdated decision framework

RECOMMENDED CHANGES:

1. UPDATE: Rule 5 - Modality-Deliverable Alignment
   
   FROM (old):
   "Binary yes/no questions determine if classroom or e-learning is appropriate."
   
   TO (new):
   "Six-factor evaluation determines publication modality:
    - Performance Type
    - Instructor Involvement
    - Audience Scale
    - Speed & Accessibility
    - Hardware Requirements
    - Content Complexity"
   
   Detailed Recommendation:
   The new framework in docs is more comprehensive and addresses real-world
   constraints. Updating this rule ensures /design-training skill uses the
   latest guidance when helping SMEs choose modality.

2. UPDATE: Examples in Rule 5
   
   Old examples reference binary decision tree
   New examples should demonstrate 6-factor evaluation
   
   Example:
   "Given large audience (100+ learners), complex content (simulations),
    hardware requirements (lab equipment), and need for immediate availability,
    recommend BLENDED modality with SCORM e-learning + classroom practicum."

3. ADD: Sync Metadata
   
   Add to both files:
   "Last synced: 2026-08-20 by /sync-docs-to-rules"

TEAM DECISION REQUIRED:

Should we apply these changes?

☐ YES - Apply all recommendations
☐ MODIFIED - Apply with modifications (explain below)
☐ NO - Don't sync (explain why)
☐ PARTIAL - Apply only specific recommendations (list below)
☐ ESCALATE - Need guidance from Aba Azeem

If modified or partial, what's different?
[Team input here]

═══════════════════════════════════════════════════════════════
```

### Team Reviews & Approves

Team reads the comparison and decides:

```
TEAM DECISION: YES - Apply all recommendations

Approved by: Peter Francis
Date: 2026-08-20
Rationale: New framework is clearly better. The 6-factor approach 
           addresses constraints we've seen in real projects.
```

### Changes Applied

```
Applying changes to .claude/rules/content-design-validation.md...

Changes made:
✓ Updated Rule 5 definition
✓ Updated examples with new framework
✓ Added sync date stamp
✓ Added reference back to docs/design/content-design-process.md

Commit: sync: update publication strategy decision framework (docs + rules)

Next time /design-training runs, it will use the updated rule.
```

---

## File Mapping: Docs to Rules

This skill knows which docs files relate to which rules:

| `docs/` File | Relates to `.claude/rules/` | Last Sync |
|---|---|---|
| `design/content-design-process.md` | `content-design-validation.md` | TBD |
| `design/file-mapping-guide.md` | `content-design-validation.md` (Rule 6) | TBD |
| `development/yaml-guide.md` | `legit-yaml.md` | TBD |
| `development/best-practices.md` | `legit-markdown-standards.md` | TBD |
| `development/content-blocks-reference.md` | `legit-blocks.md` | TBD |
| `development/presentations.md` | `legit-presentations.md` | TBD |

When you update a docs file, the skill automatically identifies related rules.

---

## Three Sync Strategies

### Strategy A: Manual with Checklist (Current)

**When to use**: You have a small documentation set

**Process**:
1. Update `.claude/rules/` file first (authoritative version)
2. Update `docs/` second (guide version)
3. Add sync date to both: "Last synced: YYYY-MM-DD"
4. Run /sync-docs-to-rules to verify
5. Commit both changes together

**Effort**: Low (requires discipline)

---

### Strategy B: Include References (Future)

**When to use**: Documentation grows and you want automatic sync

**Process**:
```markdown
# Step 3: Publication Strategy

For the authoritative decision framework:

!include(./.claude/rules/content-design-validation.md#decision-framework)

[Additional context and examples specific to this guide]
```

**Benefit**: Docs always show what Claude Code sees

**Effort**: Medium (initial refactoring)

---

### Strategy C: Automated Hook Validation (Long-term)

**When to use**: Multiple maintainers or large documentation

**Process**:
1. Pre-commit hook detects docs/ changes
2. Automatically compares with rules
3. If out of sync, commit fails with guidance
4. Developer must update both or provide rationale
5. Then commit succeeds

**Benefit**: Prevents accidental drift

**Effort**: High (hook development)

---

## Sync Workflow: Full Example

### Step 1: Update Documentation

File: `docs/design/content-design-process.md`

```markdown
## Step 3: Publication Strategy

NEW:
We use a six-factor framework to choose modality:

1. **Performance Type** — What kind of learning?
   - Recall/Knowledge
   - Procedure
   - Problem-Solving
   - Complex (requires practice)

2. **Instructor Involvement** — How much teaching?
   - None (self-paced)
   - Minimal (support only)
   - Moderate (guidance)
   - Required (live instruction)

[... etc]
```

### Step 2: Commit Changes

```bash
git add docs/design/content-design-process.md
git commit -m "Update publication strategy decision framework"
# Pre-commit hook runs /sync-docs-to-rules
```

### Step 3: Review Recommendations

Terminal output shows:

```
🔄 Documentation files detected
📝 Files changed in docs/:
   docs/design/content-design-process.md

🤖 Running sync validation with Claude...

[Displays comparison and recommendations]

Action Required:
1. Review Claude's recommendations
2. Approve or modify
3. Apply changes to .claude/rules/
```

### Step 4: Team Approves

Team reviews the comparison (side-by-side) and decides:

```
✅ APPROVED - Apply all recommendations
```

### Step 5: Rules Updated

```bash
/sync-docs-to-rules --apply
```

Result:
- `.claude/rules/content-design-validation.md` updated
- Sync dates added
- New commit created
- Claude Code skills now use latest rules

### Step 6: Verify

Next time /design-training runs:

```
Loading validation rules from .claude/rules/content-design-validation.md...
✓ Using latest framework (synced 2026-08-20)
✓ Six-factor decision framework active
```

---

## Conflict Resolution

### Scenario: Docs and Rules Disagree

If docs and rules say different things:

```
⚠️ CONFLICT DETECTED

docs/design/content-design-process.md Step 3 says:
"Use 6-factor framework for modality selection"

.claude/rules/content-design-validation.md Rule 5 says:
"Use binary yes/no questions for modality selection"

Which is correct?

☐ Update rules to match docs (new framework is better)
☐ Update docs to match rules (old framework is still valid)
☐ Keep both different (explain why)
☐ Need discussion with Aba Azeem
```

---

## When NOT to Sync

Not every docs change requires rule changes:

```
DOES NOT NEED SYNC:
- Adding examples to docs (if rule is still accurate)
- Rewording explanation (if meaning is same)
- Adding clarification (if rule is still valid)
- Adding new guide (if no corresponding rule exists yet)

DOES NEED SYNC:
- Changing decision criteria (rule needs updating)
- Adding new validation step (new rule needed)
- Deprecating an approach (rule needs retiring)
- Changing core definitions (rules must match)
```

The skill will ask:

```
This looks like a documentation enhancement, not a rule change.

Do you want to update the corresponding rule?
☐ Yes, rule should also change
☐ No, documentation is just elaborating
☐ Not sure, show me the difference
```

---

## For Claude Code Implementers

When /design-training or /develop-training runs:

```
Validation rules loaded from: .claude/rules/
├─ content-design-validation.md (last synced: 2026-08-20)
├─ legit-yaml.md (last synced: 2026-08-19)
└─ legit-markdown-standards.md (last synced: 2026-08-18)

All rules are current and consistent with documentation.
```

If rules are out of sync:

```
⚠️ WARNING: Rules may be out of sync with documentation

Rule: .claude/rules/content-design-validation.md
Last synced: 2026-08-15 (5 days ago)

Recent docs changes:
  • docs/design/content-design-process.md (2026-08-20)

Run /sync-docs-to-rules to review and apply updates.
```

---

## Summary

**Use `/sync-docs-to-rules` when:**
- You update documentation
- You want to ensure rules stay current
- You need to justify changes to the team
- You want a record of sync history

**What it provides:**
- ✅ Automatic change detection
- ✅ Side-by-side comparisons
- ✅ Recommended updates
- ✅ Rationale for each change
- ✅ Team approval workflow
- ✅ Sync history with dates

**Result:**
- Documentation and rules stay in sync
- Claude Code skills always use latest guidance
- Team maintains control over changes
- No silent drift between docs and enforcement

**Ready to sync?**

```
/sync-docs-to-rules
```

The skill will show the current state and recommend any needed updates.
