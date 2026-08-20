---
name: sync-detection-engine
description: Internal engine for detecting and comparing documentation vs rules
internal: true
---

# Sync Detection Engine - Implementation

This document contains the actual detection and comparison logic for the sync-docs-to-rules skill.

## File Mapping: What Compares to What

```javascript
const syncMappings = [
  {
    docFile: "docs/design/content-design-process.md",
    docSection: "Step 3: Publication Strategy Decision Framework",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "Rule 5: Modality-Deliverable Alignment",
    name: "Publication Strategy Framework",
    critical: true
  },
  {
    docFile: "docs/design/content-design-process.md",
    docSection: "Step 7: Validation",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "All Rules 1-6",
    name: "Design Validation Rules",
    critical: true
  },
  {
    docFile: "docs/design/file-mapping-guide.md",
    docSection: "File Structure and Naming",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "Rule 6: File Mapping Completeness",
    name: "File Mapping Standards",
    critical: false
  },
  {
    docFile: "docs/development/yaml-guide.md",
    docSection: "YAML Frontmatter Requirements",
    ruleFile: ".claude/rules/legit-yaml.md",
    ruleSection: "Required Attributes",
    name: "YAML Standards",
    critical: false
  },
  {
    docFile: "docs/development/best-practices.md",
    docSection: "Writing Standards",
    ruleFile: ".claude/rules/legit-markdown-standards.md",
    ruleSection: "All Standards",
    name: "Markdown Writing Standards",
    critical: false
  }
];
```

## Detection Algorithm

### Step 1: Detect Changed Files

```javascript
function detectChangedDocs() {
  // Check which docs/ files have been modified
  // Can use git log or file modification timestamps
  
  const changedFiles = [];
  
  // Method A: Git-based (most accurate)
  // git log --name-only --oneline -n 1 -- docs/
  
  // Method B: File stat based (fallback)
  // Check mtime of docs/ files vs .claude/rules/ files
  
  return changedFiles;
}
```

### Step 2: Map Changed Files to Rules

```javascript
function findAffectedRules(changedDocFiles) {
  const affectedRules = [];
  
  for (const docFile of changedDocFiles) {
    const mapping = syncMappings.find(m => m.docFile === docFile);
    if (mapping) {
      affectedRules.push({
        docFile: docFile,
        ruleFile: mapping.ruleFile,
        docSection: mapping.docSection,
        ruleSection: mapping.ruleSection,
        name: mapping.name,
        critical: mapping.critical
      });
    }
  }
  
  return affectedRules;
}
```

### Step 3: Read and Compare

```javascript
function compareDocToRule(docFile, ruleFile, docSection, ruleSection) {
  // Read both files
  const docContent = readFile(docFile);
  const ruleContent = readFile(ruleFile);
  
  // Extract relevant sections
  const docSection = extractSection(docContent, docSection);
  const ruleSection = extractSection(ruleContent, ruleSection);
  
  // Compare
  const differences = findDifferences(docSection, ruleSection);
  
  return {
    isSynced: differences.length === 0,
    differences: differences,
    docSnippet: docSection.slice(0, 500),
    ruleSnippet: ruleSection.slice(0, 500)
  };
}
```

## Key Comparison Points

### 1. Publication Strategy Framework (Most Critical)

**What to look for in docs**:
- Presence of "six-factor framework" or similar
- Decision criteria listed
- Examples of modality recommendations

**What to check in rules**:
- Rule 5 definition
- Examples in the rule
- Decision logic described

**Sync signals**:
- If docs mention 6 factors but rule mentions binary → OUT OF SYNC
- If docs have new decision criteria not in rules → OUT OF SYNC
- If examples differ significantly → MIGHT BE OUT OF SYNC

### 2. Design Validation Rules

**What to check**:
- Number of rules (should be 6)
- Rule names and descriptions
- Examples provided
- Validation checklist

**Sync signals**:
- Missing rules in either version
- Rule descriptions differ
- Examples don't match

### 3. File Mapping Standards

**What to check**:
- File naming conventions
- Folder structure
- Outcome title kebab-case requirement

**Sync signals**:
- Naming examples differ
- Structure diagrams differ
- Requirements changed

## Recommendation Generator

### Algorithm

```javascript
function generateRecommendations(comparison) {
  const recommendations = [];
  
  for (const diff of comparison.differences) {
    const rec = {
      type: classifyDifference(diff),
      severity: assessSeverity(diff),
      docValue: diff.docValue,
      ruleValue: diff.ruleValue,
      recommendation: generateRecommendation(diff),
      rationale: generateRationale(diff)
    };
    recommendations.push(rec);
  }
  
  // Sort by severity
  return recommendations.sort((a, b) => b.severity - a.severity);
}
```

### Difference Classification

```javascript
function classifyDifference(diff) {
  if (diff.isNewInDoc && !diff.isInRule) {
    return "NEW_CONTENT"; // Doc has something rule doesn't
  } else if (diff.isInRule && !diff.isNewInDoc) {
    return "DEPRECATED"; // Rule has something docs don't
  } else if (diff.docValue !== diff.ruleValue) {
    return "CHANGED"; // Both exist but differ
  } else {
    return "UNCLEAR"; // Unknown difference type
  }
}
```

### Severity Assessment

```javascript
function assessSeverity(diff) {
  // 0-1: Low (cosmetic, examples)
  // 1-2: Medium (description changes)
  // 2-3: High (core framework changes)
  
  if (diff.type === "NEW_CONTENT" && diff.context.includes("framework")) {
    return 3; // New framework = HIGH
  } else if (diff.type === "CHANGED" && diff.context.includes("rule")) {
    return 2; // Rule changed = MEDIUM
  } else if (diff.type === "NEW_CONTENT" && diff.context.includes("example")) {
    return 1; // New example = LOW
  }
  
  return 1; // Default low
}
```

## Real Example: 6-Factor Framework Sync

### Detection Phase
```
DETECTING CHANGES...

Scanning docs/
  ✓ docs/design/content-design-process.md (CHANGED)
  ✓ docs/design/file-mapping-guide.md (unchanged)

Changed files: 1
Mapping to rules: 
  ✓ Found sync mapping: content-design-validation.md (Rule 5)
```

### Comparison Phase
```
COMPARING VERSIONS...

File: docs/design/content-design-process.md
Section: Step 3 - Publication Strategy Decision Framework

EXTRACTING CONTENT...
✓ Found section (612 lines)
✓ Extracted 6-factor framework
✓ Found decision criteria
✓ Found examples

Comparing to: .claude/rules/content-design-validation.md
Section: Rule 5 - Modality-Deliverable Alignment

✓ Found rule section (145 lines)
⚠️ Found binary framework (old)
⚠️ Missing 6-factor model
⚠️ Missing new examples
```

### Analysis Phase
```
ANALYZING DIFFERENCES...

Difference 1 (HIGH SEVERITY):
  Type: CHANGED
  What changed: Decision framework
  From: "Binary yes/no questions"
  To: "Six-factor evaluation"
  Impact: Rule enforcement logic

Difference 2 (MEDIUM SEVERITY):
  Type: NEW_CONTENT
  What's new: Specific decision criteria
  Criteria: [list of 6 factors]
  Impact: Examples and guidance

Difference 3 (MEDIUM SEVERITY):
  Type: CHANGED
  What changed: Example scenarios
  From: 2 examples (old)
  To: Multiple scenarios (new)
  Impact: Guidance quality
```

### Recommendation Phase
```
GENERATING RECOMMENDATIONS...

RECOMMENDATION 1: Update Rule 5 Framework
  Severity: HIGH
  Action: Update Rule 5 definition
  From:
    "Binary decision tree determines modality"
  To:
    "Six-factor evaluation:
     1. Performance Type
     2. Instructor Involvement
     3. Audience Scale
     4. Speed & Accessibility
     5. Hardware Requirements
     6. Content Complexity"
  Rationale:
    - New framework is more comprehensive
    - /design-training will use this rule
    - Better matches real-world constraints
    - SMEs need latest guidance

RECOMMENDATION 2: Add Examples to Rule 5
  Severity: MEDIUM
  Action: Add new example scenarios
  Current examples: 2 (binary-based)
  Suggested examples: 4+ (6-factor based)
  Rationale:
    - Examples help SMEs understand decision logic
    - New framework has more nuanced examples
    - Improves guidance clarity

RECOMMENDATION 3: Update Sync Date
  Severity: LOW
  Action: Add timestamp to both files
  Format: "Last synced: 2026-08-20"
  Rationale:
    - Track when syncs happen
    - Audit trail for changes
```

## Output Format

### For User Display

```
═══════════════════════════════════════════════════════
  DOCUMENTATION SYNC ANALYSIS
═══════════════════════════════════════════════════════

✓ SYNC CHECK COMPLETE

Changed files detected: 1
  • docs/design/content-design-process.md

Affected rules: 1
  • .claude/rules/content-design-validation.md (Rule 5)

Sync status: OUT OF SYNC ⚠️

═══════════════════════════════════════════════════════

RECOMMENDATIONS (3 total)

1. UPDATE Rule 5 - Modality Decision Framework [HIGH]
   The decision framework in docs has been updated from binary
   to six-factor evaluation. Rule 5 should match.
   
   FROM: "Binary yes/no questions"
   TO: "Six-factor evaluation (Performance, Instructor, Scale, Speed, Hardware, Complexity)"
   
   Impact: /design-training skill will use this framework

2. ADD Examples to Rule 5 [MEDIUM]
   Docs now include detailed examples for each factor combination.
   Examples in rules should be updated to match.

3. UPDATE Sync Date [LOW]
   Add "Last synced: 2026-08-20" to both files for audit trail

═══════════════════════════════════════════════════════

ACTION REQUIRED:

Do you want to apply these recommendations?

☐ YES - Apply all recommendations
☐ MODIFIED - Apply with modifications
☐ NO - Don't sync (explain why)
☐ ESCALATE - Need review from Aba Azeem

═══════════════════════════════════════════════════════
```

## Applied Changes Output

When approved, the skill would prepare:

```markdown
APPLYING CHANGES...

✓ Reading: .claude/rules/content-design-validation.md
✓ Updating: Rule 5 - Modality-Deliverable Alignment
✓ Adding: New 6-factor framework
✓ Adding: New examples
✓ Adding: Sync date stamp
✓ Preparing: Updated file

PREVIEW OF CHANGES:

File: .claude/rules/content-design-validation.md
Rule: 5 - Modality-Deliverable Alignment

--- OLD ---
[old definition]

+++ NEW +++
[new definition with 6 factors]

--- CHANGES ---
+ Added 6-factor decision framework
+ Added 4 new example scenarios
+ Added sync date: 2026-08-20
- Removed binary decision reference

Ready to commit? (This would be done by the user)

Commit message:
"sync: update publication strategy decision framework (docs + rules)"
```

## Edge Cases & Handling

### Case 1: Docs Changed, Rules Haven't
```
Status: OUT OF SYNC
Action: Recommend updating rules to match docs
Decision: Usually approve (docs are source of truth)
```

### Case 2: Rules Changed, Docs Haven't
```
Status: OUT OF SYNC
Action: Recommend updating docs to match rules
Decision: Review carefully (rules reflect enforcement)
```

### Case 3: Both Changed Differently
```
Status: CONFLICT
Action: Show both versions, ask team to decide
Decision: Escalate to Aba Azeem if strategic
```

### Case 4: Content Moved But Not Changed
```
Status: UNCLEAR
Action: Compare semantically, not just line-by-line
Decision: May not need sync if meaning is same
```

## Integration with Claude Code

When /design-training or /develop-training runs:

```
Loading validation rules...
Checking sync status: .claude/rules/content-design-validation.md
Last synced: 2026-08-20 ✓ (current)

Using Rule 5: Six-factor publication strategy framework
Decision engine loaded with latest criteria
```

## Summary

This engine:
- ✅ Detects documentation changes
- ✅ Maps to affected rules
- ✅ Compares versions
- ✅ Generates recommendations
- ✅ Supports team decisions
- ✅ Tracks sync history

Result: Documentation and rules stay synchronized, Claude Code always uses latest guidance.
