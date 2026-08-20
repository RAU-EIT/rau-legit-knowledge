---
name: sync-docs-to-rules
description: Detect documentation changes and recommend rule updates to keep them in sync
aliases: [sync, validate-sync, review-sync]
whenToUse: You've updated documentation and want to check if rules need updating
---

# Sync Documentation to Rules — Working Prototype

This is a functional prototype that detects documentation changes and recommends rule updates.

## How to Use

Run this skill anytime you've updated documentation files. It will:

1. **Detect changes** — Find which docs/ files were modified
2. **Compare** — Read docs and corresponding rules
3. **Analyze** — Find differences and map to specific rules
4. **Recommend** — Show what should be updated and why

## Current Implementation

This prototype focuses on the most critical sync relationship: **Publication Strategy Decision Framework**

**Monitors**:
- `docs/design/content-design-process.md` (Step 3)
- `.claude/rules/content-design-validation.md` (Rule 5)

## Running the Sync

Simply tell me:

```
Run sync check
```

Or provide context:

```
I updated the publication strategy in docs/
```

I will then:
1. Check if the decision framework has changed
2. Compare with the rule definition
3. Show any discrepancies
4. Recommend specific updates

## Example: Detecting the 6-Factor Framework

When the docs were updated to the 6-factor framework:

**What changed**: `docs/design/content-design-process.md` Step 3

**Old framework**: Binary yes/no questions  
**New framework**: Six-factor evaluation (Performance Type, Instructor Involvement, Audience Scale, Speed/Accessibility, Hardware Requirements, Content Complexity)

**Current rule**: `.claude/rules/content-design-validation.md` Rule 5

**Issue**: Rule still references old binary framework

**Recommendation**: Update Rule 5 with new 6-factor approach and examples

## How I Compare Documents

When you ask me to sync, I:

### 1. Identify the Change
```
📝 Detecting documentation changes...

Found changes in:
✓ docs/design/content-design-process.md
```

### 2. Read Both Versions
```
Reading:
✓ docs/design/content-design-process.md (Step 3)
✓ .claude/rules/content-design-validation.md (Rule 5)
```

### 3. Extract Key Differences
```
Decision Framework Difference:
┌─ OLD (in rules)
│  Binary: Can learner become competent without practice? Yes/No
│
└─ NEW (in docs)
   Six factors:
   1. Performance Type
   2. Instructor Involvement
   3. Audience Scale
   4. Speed/Accessibility
   5. Hardware Requirements
   6. Content Complexity
```

### 4. Generate Recommendations
```
RECOMMENDATION: Update Rule 5

Current:
  "Rule 5: Modality-Deliverable Alignment
   Binary decision tree determines modality"

Recommended:
  "Rule 5: Modality-Deliverable Alignment
   Six-factor evaluation determines modality:
   - Performance Type (Recall, Procedure, Problem-Solving, Complex)
   - Instructor Involvement (None, Minimal, Moderate, Required)
   - Audience Scale (Small, Medium, Large)
   - Speed & Accessibility
   - Hardware Requirements
   - Content Complexity"
```

### 5. Show Rationale
```
WHY THIS MATTERS:
✓ /design-training skill uses Rule 5 when recommending modality
✓ If rule is outdated, SMEs get old guidance
✓ New 6-factor framework is more comprehensive
✓ Sync ensures Claude Code uses latest decision logic
```

## What This Prototype Can Do

✅ **Detect changes** in `docs/design/content-design-process.md`  
✅ **Compare** with `.claude/rules/content-design-validation.md`  
✅ **Show differences** between old and new frameworks  
✅ **Recommend updates** for Rule 5  
✅ **Explain rationale** for each recommendation  
✅ **Support team decisions** (approve/modify/escalate)  

## What to Tell Me

### Option 1: Full Sync Check
```
"Run sync check"
```
I'll scan for any documentation changes and check sync status.

### Option 2: Check Specific Rule
```
"Check sync for Rule 5"
or
"Sync the publication strategy framework"
```
I'll focus on comparing the decision framework docs vs. rules.

### Option 3: After Making Changes
```
"I updated the decision framework in docs/design/content-design-process.md"
```
I'll immediately compare with the rule and show recommendations.

### Option 4: Review Specific File
```
"Check if docs/design/content-design-process.md is in sync"
```
I'll verify all sections related to design validation rules.

## Example Workflow

### Step 1: You Update Documentation
```
Updated: docs/design/content-design-process.md
Changed: Step 3 - Publication Strategy
New addition: 6-factor decision framework
```

### Step 2: Run Sync Check
```
"Check if this is in sync with the rules"
```

### Step 3: I Analyze and Recommend
```
✓ CHANGE DETECTED in publication strategy framework

CURRENT STATE:
docs/: 6-factor framework (new)
rules/: Binary framework (old)

OUT OF SYNC: YES

RECOMMENDATION:
Update .claude/rules/content-design-validation.md Rule 5
to match new 6-factor framework in docs/
```

### Step 4: Team Decides
```
☐ YES - Apply recommendation
☐ MODIFIED - Apply with changes
☐ NO - Keep as is (explain why)
☐ ESCALATE - Need guidance
```

### Step 5: Changes Applied (if approved)
```
✓ Updated .claude/rules/content-design-validation.md
✓ Added sync date: 2026-08-20
✓ Next time /design-training runs, uses new framework
```

## Current Sync Status

### ✅ Up to Date
- `legit-yaml.md` — Matches docs/development/yaml-guide.md
- `legit-markdown-standards.md` — Matches docs/development/best-practices.md
- `legit-blocks.md` — Matches docs/development/content-blocks-reference.md

### ⚠️ Check Needed
- `content-design-validation.md` Rule 5 — May be out of sync with Step 3 of docs/design/content-design-process.md (depends on whether 6-factor framework is in docs yet)

### 📋 Not Yet Tracked
- New validation rules (none yet)
- Rules without corresponding docs (none yet)

## How to Request Changes

When I recommend a sync update, you can:

### Approve All
```
"Apply all recommendations"
```
I'll prepare the changes for you to review.

### Approve Specific
```
"Update Rule 5 to match the new framework"
```
I'll update only Rule 5, keep others as-is.

### Modify and Approve
```
"Update Rule 5 but also add an example about hardware requirements"
```
I'll apply changes and add your requested example.

### Escalate
```
"This needs discussion with Aba Azeem before updating"
```
I'll document the recommendation and mark it for discussion.

## Sync History

| Date | Change | Rule | Status |
|------|--------|------|--------|
| 2026-08-20 | Decision framework to 6-factor | Rule 5 | ⏳ Pending review |

## Tips for Using This Skill

### Best Practices
1. **Run after doc updates** — Check sync immediately after changes
2. **Review before approving** — Read recommendations carefully
3. **Document decisions** — Record why you approved/rejected changes
4. **Keep sync regular** — Check weekly if docs change frequently
5. **Escalate as needed** — Use for strategic decisions with Aba Azeem

### Common Scenarios

**Scenario 1: You fix a typo in docs**
```
Result: Sync check finds no rule changes needed ✓
```

**Scenario 2: You add examples to docs**
```
Result: Sync check suggests adding examples to rules too
Decision: Usually approve (keep them consistent)
```

**Scenario 3: You change a decision framework**
```
Result: Sync check shows rules are out of sync
Decision: Review carefully, get team approval, apply changes
```

**Scenario 4: You add a new rule**
```
Result: Sync check alerts that rules have no corresponding docs
Decision: Create docs to match the rule
```

## What's Next

This prototype handles the core sync functionality. Future enhancements could include:

- 📊 Sync dashboard (overview of all syncs)
- 🔄 Automatic sync for non-strategic changes
- 📝 Sync history and audit trail
- 🎯 Batch sync for multiple files
- ⚡ Performance optimization for large docs
- 🔗 Bi-directional sync (docs ← → rules)

## Ready to Use

To start syncing, simply tell me:

**"Check if documentation is in sync with rules"**

or

**"Run a sync check"**

I'll immediately:
1. Detect any documentation changes
2. Compare with corresponding rules
3. Show recommendations
4. Ask for your approval before making changes

This ensures your documentation and validation rules stay consistent, and Claude Code skills always use the latest guidance.
