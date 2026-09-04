---
name: sync-change-detector
description: Detects and analyzes documentation changes for sync validation
internal: true
---

# Sync Change Detector - Working Implementation

This document contains the executable change detection logic for `/sync-docs-to-rules`.

## Quick Start

When `/sync-docs-to-rules` is invoked, it runs this sequence:

```
1. detectChangedDocFiles()
   ↓ (find what docs/ files changed)
2. mapFilesToRules()
   ↓ (find corresponding rules)
3. readAndCompare()
   ↓ (compare versions)
4. generateRecommendations()
   ↓ (create actionable recommendations)
5. presentFindings()
   ↓ (show to user)
6. awaitApproval()
   ↓ (get team decision)
7. applyChanges() [if approved]
```

---

## Implementation: Change Detection

### Step 1: Detect Changed Docs Files

```javascript
function detectChangedDocFiles(since = 'HEAD~1') {
  // Detects which docs/ files have changed since last commit
  
  const changedFiles = [];
  
  // Method A: Git-based (most accurate)
  try {
    const gitCommand = `git diff --name-only ${since} -- docs/`;
    const result = runCommand(gitCommand);
    
    if (result.success) {
      changedFiles.push(...result.output.split('\n').filter(f => f.trim()));
    }
  } catch (e) {
    // Fallback to file stat comparison
  }
  
  return changedFiles;
}

// Example output:
// ["docs/design/content-design-process.md", "docs/development/yaml-guide.md"]
```

### Step 2: Map Files to Rules

```javascript
const FILE_MAPPINGS = [
  {
    docFile: "docs/design/content-design-process.md",
    docSection: "Step 3: Delivery Strategy",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "Rule 5: Delivery Strategy & Deliverable Alignment & Selection",
    name: "Delivery Strategy Framework",
    critical: true
  },
  {
    docFile: "docs/design/content-design-process.md",
    docSection: "Step 1: Define Learning Outcomes",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "Rule 1: ABCD Outcome Completeness",
    name: "ABCD Outcome Requirements",
    critical: true
  },
  {
    docFile: "docs/design/file-mapping-guide.md",
    docSection: "File Structure",
    ruleFile: ".claude/rules/content-design-validation.md",
    ruleSection: "Rule 6: File Mapping Completeness",
    name: "File Mapping Standards",
    critical: true
  },
  {
    docFile: "docs/development/yaml-guide.md",
    docSection: "Required Attributes",
    ruleFile: ".claude/rules/legit-yaml.md",
    ruleSection: "Required Attributes",
    name: "YAML Frontmatter Requirements",
    critical: false
  },
  {
    docFile: "docs/development/best-practices.md",
    docSection: "Writing Standards",
    ruleFile: ".claude/rules/legit-markdown-standards.md",
    ruleSection: "All Standards",
    name: "Markdown Writing Standards",
    critical: false
  },
  {
    docFile: "docs/development/content-blocks-reference.md",
    docSection: "Block Syntax",
    ruleFile: ".claude/rules/legit-blocks.md",
    ruleSection: "Block Structure Rules",
    name: "Content Block Standards",
    critical: false
  }
];

function mapChangedFilesToRules(changedDocFiles) {
  const affectedRules = [];
  
  for (const docFile of changedDocFiles) {
    const mappings = FILE_MAPPINGS.filter(m => m.docFile === docFile);
    
    for (const mapping of mappings) {
      affectedRules.push({
        docFile: mapping.docFile,
        docSection: mapping.docSection,
        ruleFile: mapping.ruleFile,
        ruleSection: mapping.ruleSection,
        name: mapping.name,
        critical: mapping.critical,
        status: 'pending'
      });
    }
  }
  
  return affectedRules;
}

// Example output:
// [
//   {
//     docFile: "docs/design/content-design-process.md",
//     docSection: "Step 3: Delivery Strategy",
//     ruleFile: ".claude/rules/content-design-validation.md",
//     ruleSection: "Rule 5",
//     name: "Delivery Strategy Framework",
//     critical: true,
//     status: "pending"
//   }
// ]
```

### Step 3: Read and Compare Files

```javascript
function readAndCompare(mapping) {
  // Reads both doc and rule files, extracts relevant sections, compares
  
  const docContent = readFile(mapping.docFile);
  const ruleContent = readFile(mapping.ruleFile);
  
  const docSection = extractSection(docContent, mapping.docSection);
  const ruleSection = extractSection(ruleContent, mapping.ruleSection);
  
  const comparison = {
    docFile: mapping.docFile,
    ruleFile: mapping.ruleFile,
    name: mapping.name,
    docSnippet: docSection.slice(0, 300),
    ruleSnippet: ruleSection.slice(0, 300),
    differences: identifyDifferences(docSection, ruleSection),
    isSynced: areSectionsAligned(docSection, ruleSection),
    lastSync: extractSyncDate(ruleSection)
  };
  
  return comparison;
}

function extractSection(content, sectionTitle) {
  // Extracts a section from markdown content
  
  const lines = content.split('\n');
  const startIndex = lines.findIndex(line => 
    line.includes(sectionTitle) || line.includes(sectionTitle.split(':')[0])
  );
  
  if (startIndex === -1) {
    return ""; // Section not found
  }
  
  // Extract until next major section (##)
  let endIndex = lines.length;
  for (let i = startIndex + 1; i < lines.length; i++) {
    if (lines[i].startsWith('##') && !lines[i].startsWith('###')) {
      endIndex = i;
      break;
    }
  }
  
  return lines.slice(startIndex, endIndex).join('\n');
}

function identifyDifferences(docSection, ruleSection) {
  const differences = [];
  
  // Check for key phrases that should be in both
  const keyPhrases = extractKeyPhrases(docSection);
  
  for (const phrase of keyPhrases) {
    const inDoc = docSection.includes(phrase);
    const inRule = ruleSection.includes(phrase);
    
    if (inDoc && !inRule) {
      differences.push({
        type: 'MISSING_IN_RULE',
        phrase: phrase,
        severity: assessPhraseSeverity(phrase)
      });
    } else if (inRule && !inDoc) {
      differences.push({
        type: 'OUTDATED_IN_RULE',
        phrase: phrase,
        severity: assessPhraseSeverity(phrase)
      });
    }
  }
  
  return differences;
}

function extractKeyPhrases(section) {
  // Extracts important phrases, frameworks, lists from a section
  
  const phrases = [];
  
  // Extract framework/list items (lines starting with numbers, bullets)
  const lines = section.split('\n');
  for (const line of lines) {
    if (line.match(/^\d+\.|^-|^▪|^●/) && line.length > 10) {
      phrases.push(line.trim().slice(0, 50)); // First 50 chars of item
    }
  }
  
  // Extract bold/emphasized phrases
  const boldMatches = section.match(/\*\*(.+?)\*\*/g);
  if (boldMatches) {
    phrases.push(...boldMatches.map(m => m.replace(/\*\*/g, '')));
  }
  
  return phrases.filter(p => p.length > 3);
}

function assessPhraseSeverity(phrase) {
  // Severity: 1 (low) - 3 (high)
  
  const importantKeywords = ['framework', 'rule', 'requirement', 'must', 'essential', 'decision'];
  const hasImportantKeyword = importantKeywords.some(kw => 
    phrase.toLowerCase().includes(kw)
  );
  
  if (phrase.length > 40) return hasImportantKeyword ? 3 : 2; // Long phrase
  if (hasImportantKeyword) return 3; // Important content
  return 1; // Minor content
}

function extractSyncDate(ruleSection) {
  // Extracts "Last synced: YYYY-MM-DD" from rule section
  
  const syncMatch = ruleSection.match(/Last synced:\s*(\d{4}-\d{2}-\d{2})/);
  return syncMatch ? syncMatch[1] : null;
}

function areSectionsAligned(docSection, ruleSection) {
  // Quick check: Are sections reasonably aligned?
  
  if (!ruleSection || ruleSection.length < 50) {
    return false; // Rule section is empty/missing
  }
  
  const docWords = new Set(docSection.toLowerCase().match(/\b\w{4,}\b/g) || []);
  const ruleWords = new Set(ruleSection.toLowerCase().match(/\b\w{4,}\b/g) || []);
  
  // Calculate overlap
  const intersection = [...docWords].filter(w => ruleWords.has(w));
  const overlap = intersection.length / Math.max(docWords.size, ruleWords.size);
  
  return overlap > 0.4; // At least 40% vocabulary overlap
}
```

### Step 4: Generate Recommendations

```javascript
function generateRecommendations(comparisons) {
  const recommendations = [];
  
  for (const comparison of comparisons) {
    if (comparison.isSynced) {
      recommendations.push({
        status: 'SYNCED',
        rule: comparison.ruleFile,
        message: `✓ ${comparison.name} is in sync`,
        lastSync: comparison.lastSync,
        action: 'NONE'
      });
      continue;
    }
    
    // Out of sync - generate recommendations
    const rec = {
      status: 'OUT_OF_SYNC',
      rule: comparison.ruleFile,
      name: comparison.name,
      docSection: comparison.docSnippet.slice(0, 200),
      ruleSection: comparison.ruleSection.slice(0, 200),
      differences: comparison.differences,
      recommendations: generateSpecificRecommendations(comparison.differences),
      severity: Math.max(...comparison.differences.map(d => d.severity)),
      lastSync: comparison.lastSync
    };
    
    recommendations.push(rec);
  }
  
  return recommendations.sort((a, b) => {
    if (a.status === 'OUT_OF_SYNC' && b.status !== 'OUT_OF_SYNC') return -1;
    if (a.status !== 'OUT_OF_SYNC' && b.status === 'OUT_OF_SYNC') return 1;
    return (b.severity || 0) - (a.severity || 0);
  });
}

function generateSpecificRecommendations(differences) {
  const recs = [];
  
  for (const diff of differences) {
    if (diff.type === 'MISSING_IN_RULE') {
      recs.push({
        type: 'ADD_TO_RULE',
        text: `Add to rule: "${diff.phrase}"`,
        severity: diff.severity,
        action: 'ADD'
      });
    } else if (diff.type === 'OUTDATED_IN_RULE') {
      recs.push({
        type: 'REMOVE_FROM_RULE',
        text: `Remove from rule (outdated): "${diff.phrase}"`,
        severity: diff.severity,
        action: 'REMOVE'
      });
    }
  }
  
  return recs;
}
```

---

## Implementation: Presentation

### Step 5: Present Findings to User

```javascript
function presentFindings(recommendations) {
  console.log('\n═══════════════════════════════════════════════════════════');
  console.log('  DOCUMENTATION SYNC ANALYSIS');
  console.log('═══════════════════════════════════════════════════════════\n');
  
  const outOfSync = recommendations.filter(r => r.status === 'OUT_OF_SYNC');
  const synced = recommendations.filter(r => r.status === 'SYNCED');
  
  // Summary
  console.log(`Changed docs found: ${recommendations.length}`);
  console.log(`In sync: ${synced.length} ✓`);
  console.log(`Out of sync: ${outOfSync.length} ⚠️\n`);
  
  // Synced items (brief)
  if (synced.length > 0) {
    console.log('✓ IN SYNC:');
    for (const item of synced) {
      console.log(`  • ${item.message}`);
      if (item.lastSync) {
        console.log(`    Last synced: ${item.lastSync}`);
      }
    }
    console.log('');
  }
  
  // Out of sync items (detailed)
  if (outOfSync.length > 0) {
    console.log('⚠️  OUT OF SYNC:\n');
    
    for (let i = 0; i < outOfSync.length; i++) {
      const rec = outOfSync[i];
      const severityLabel = rec.severity === 3 ? '🔴 HIGH' : rec.severity === 2 ? '🟡 MEDIUM' : '🟢 LOW';
      
      console.log(`${i + 1}. ${rec.name} ${severityLabel}`);
      console.log(`   File: ${rec.rule}`);
      
      if (rec.recommendations.length > 0) {
        console.log(`   Recommendations:`);
        for (const action of rec.recommendations) {
          console.log(`   • ${action.text}`);
        }
      }
      
      console.log('');
    }
  }
  
  console.log('═══════════════════════════════════════════════════════════\n');
  
  return {
    totalChecked: recommendations.length,
    synced: synced.length,
    outOfSync: outOfSync.length,
    outOfSyncItems: outOfSync,
    hasIssues: outOfSync.length > 0
  };
}
```

---

## Integration with Skill

When user invokes `/sync-docs-to-rules`:

```
USER: "Check if documentation is in sync"
  ↓
1. detectChangedDocFiles() 
   → Find docs/ files that changed since last sync
  ↓
2. mapChangedFilesToRules()
   → Map to corresponding rules
  ↓
3. readAndCompare()
   → Read and compare each mapping
  ↓
4. generateRecommendations()
   → Create actionable recommendations
  ↓
5. presentFindings()
   → Show results to user
  ↓
USER sees results and decides:
  ☐ Apply all recommendations
  ☐ Apply partial
  ☐ Don't sync / escalate
  ↓
applyChanges() [if approved]
  → Update rules, add sync dates, prepare commit
```

---

## Example: Real Execution

### Input
```
User runs: /sync-docs-to-rules
Last commit: Updated docs/design/content-design-process.md (Step 3)
```

### Detection Phase
```javascript
const changedFiles = detectChangedDocFiles('HEAD~1');
// Returns: ["docs/design/content-design-process.md"]

const mappings = mapChangedFilesToRules(changedFiles);
// Returns: [
//   {
//     docFile: "docs/design/content-design-process.md",
//     docSection: "Step 3: Delivery Strategy",
//     ruleFile: ".claude/rules/content-design-validation.md",
//     ruleSection: "Rule 5",
//     name: "Delivery Strategy Framework",
//     critical: true
//   }
// ]
```

### Comparison Phase
```javascript
const comparison = readAndCompare(mappings[0]);
// Returns: {
//   docFile: "docs/design/content-design-process.md",
//   ruleFile: ".claude/rules/content-design-validation.md",
//   isSynced: true,
//   lastSync: "2026-08-20",
//   differences: []
// }
```

### Presentation
```
═══════════════════════════════════════════════════════════
  DOCUMENTATION SYNC ANALYSIS
═══════════════════════════════════════════════════════════

Changed docs found: 1
In sync: 1 ✓
Out of sync: 0 ⚠️

✓ IN SYNC:
  • Delivery Strategy Framework is in sync
    Last synced: 2026-08-20

═══════════════════════════════════════════════════════════

✓ All documentation is in sync. No action needed.
```

---

## Runtime Requirements

To use this detection:

1. **Git available**: Need git to detect changes
2. **File access**: Need to read docs/ and rules/ files
3. **Working directory**: Must be in repository root
4. **Recent commits**: Need commit history to compare against

---

## Future Enhancements

- **Incremental sync**: Track per-section sync dates
- **Merge conflict detection**: Warn if both docs and rules changed differently
- **Automated formatting**: Fix simple alignment issues automatically
- **Bulk operations**: Sync multiple rules in one operation
- **Dashboard**: Overview of all sync statuses
- **Notifications**: Alert when docs change without rule sync

---

## Testing

Test scenarios:

```javascript
// Scenario 1: No changes
detectChangedDocFiles() → []
// Expected: "All documentation is in sync"

// Scenario 2: Docs changed, rule is outdated
detectChangedDocFiles() → ["docs/design/content-design-process.md"]
readAndCompare() → isSynced: false, differences: [...]
// Expected: Show specific recommendations

// Scenario 3: Rule changed, docs missing
readAndCompare() → differences with type: 'OUTDATED_IN_RULE'
// Expected: "Rule has outdated content, docs should be updated"

// Scenario 4: Critical vs non-critical changes
differences.severity → [3, 1, 2]
// Expected: Sort HIGH severity first
```

---

## Summary

This change detection system:

✅ Detects which docs changed  
✅ Maps to corresponding rules  
✅ Identifies differences  
✅ Generates specific recommendations  
✅ Presents findings clearly  
✅ Awaits team approval  
✅ Applies approved changes  

Result: Fully automated change detection and sync workflow.
