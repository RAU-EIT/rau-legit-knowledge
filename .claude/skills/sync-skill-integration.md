---
name: sync-skill-integration
description: Integration layer connecting change detection to /sync-docs-to-rules skill
internal: true
---

# Sync Skill Integration - Active Implementation

This document shows how the change detection engine integrates with the `/sync-docs-to-rules` skill to create a working end-to-end system.

---

## Skill Entry Points

### User Commands

The `/sync-docs-to-rules` skill responds to these user inputs:

```
"Check if documentation is in sync"
"Run sync check"
"Sync docs to rules"
"Check sync status"
"I updated docs/design/content-design-process.md"
```

### Automatic (Pre-commit Hook)

Git pre-commit hook automatically runs when docs/ files change:

```bash
git commit -m "Update docs/..."
# → .claude/hooks/pre-commit-docs-sync triggers
# → /sync-docs-to-rules --auto runs
# → Shows recommendations before commit
```

---

## Workflow: Full Execution Path

```
USER INPUT
  ↓
PARSE REQUEST
  ├─ Determine if manual or automatic
  └─ Get scope (all docs or specific file)
  ↓
DETECT CHANGES (sync-change-detector.md)
  ├─ detectChangedDocFiles()
  │   → Find changed docs/ files
  └─ mapChangedFilesToRules()
      → Map to corresponding rules
  ↓
COMPARE VERSIONS
  ├─ readAndCompare()
  │   → Extract sections
  │   → Identify differences
  │   → Get last sync date
  └─ generateRecommendations()
      → Create specific actions
  ↓
PRESENT FINDINGS
  ├─ presentFindings()
  │   → Show sync status
  │   → List in-sync items
  │   → Show out-of-sync with recommendations
  └─ awaitApproval()
      → Ask user what to do
  ↓
APPLY CHANGES [IF APPROVED]
  ├─ applyChanges()
  │   → Update rule files
  │   → Add sync dates
  │   → Add cross-references
  └─ createCommit()
      → Stage changes
      → Create commit
      → Ready for user to push
  ↓
DONE
  → User reviews and pushes changes
```

---

## Implementation: Skill Handler

When user invokes `/sync-docs-to-rules`:

```javascript
async function handleSyncCommand(userInput, context) {
  console.log('Initializing /sync-docs-to-rules...\n');
  
  // Step 1: Parse user request
  const request = parseRequest(userInput);
  // {
  //   command: 'sync-check' | 'apply' | 'review',
  //   scope: 'all' | string (specific file),
  //   mode: 'manual' | 'auto'
  // }
  
  // Step 2: Detect changes
  console.log('📝 Detecting documentation changes...\n');
  const changedFiles = detectChangedDocFiles(request.scope);
  
  if (changedFiles.length === 0) {
    console.log('✓ No changes detected. All documentation is in sync.\n');
    return;
  }
  
  console.log(`Found ${changedFiles.length} changed file(s):\n`);
  changedFiles.forEach(f => console.log(`  • ${f}`));
  console.log('');
  
  // Step 3: Map to rules
  console.log('🔍 Analyzing affected rules...\n');
  const affectedRules = mapChangedFilesToRules(changedFiles);
  
  // Step 4: Compare versions
  console.log('⚖️  Comparing documentation vs rules...\n');
  const comparisons = affectedRules.map(rule => readAndCompare(rule));
  
  // Step 5: Generate recommendations
  const recommendations = generateRecommendations(comparisons);
  
  // Step 6: Present findings
  const findings = presentFindings(recommendations);
  
  // Step 7: Handle user approval
  if (request.command === 'sync-check') {
    // User just wanted to see status
    await showApprovalPrompt(findings);
  } else if (request.command === 'apply') {
    // User approved changes already (or auto-applying)
    await applyChanges(findings);
  }
}
```

---

## Detailed Implementation: Each Phase

### Phase 1: Parse Request

```javascript
function parseRequest(userInput) {
  const input = userInput.toLowerCase();
  
  // Determine command
  let command = 'sync-check'; // default
  if (input.includes('apply') || input.includes('update')) {
    command = 'apply';
  } else if (input.includes('review') || input.includes('check')) {
    command = 'sync-check';
  }
  
  // Determine scope
  let scope = 'all';
  if (input.includes('rule 5') || input.includes('modality')) {
    scope = '.claude/rules/content-design-validation.md';
  } else if (input.includes('yaml')) {
    scope = '.claude/rules/legit-yaml.md';
  }
  // ... other specific file mappings
  
  // Determine mode
  const mode = userInput.includes('auto') ? 'auto' : 'manual';
  
  return { command, scope, mode };
}

// Examples:
parseRequest('Check if documentation is in sync')
// → { command: 'sync-check', scope: 'all', mode: 'manual' }

parseRequest('Apply sync changes to Rule 5')
// → { command: 'apply', scope: 'content-design-validation.md', mode: 'manual' }

parseRequest('I updated publication strategy in docs')
// → { command: 'sync-check', scope: 'content-design-validation.md', mode: 'manual' }
```

### Phase 2: Detect Changes

```javascript
function detectChangedDocFiles(scope = 'all') {
  // Finds docs/ files that changed since last sync
  
  if (scope === 'all') {
    // Check all docs/
    const gitOutput = runCommand('git diff --name-only HEAD~1 -- docs/');
    return gitOutput.split('\n').filter(f => f && f.startsWith('docs/'));
  } else {
    // Scope is specific file - check if it changed
    const gitOutput = runCommand(`git diff --name-only HEAD~1 -- ${scope}`);
    return gitOutput.length > 0 ? [scope] : [];
  }
}

// Real example:
// $ git diff --name-only HEAD~1 -- docs/
// docs/design/content-design-process.md
// docs/development/yaml-guide.md
//
// Returns: ["docs/design/content-design-process.md", "docs/development/yaml-guide.md"]
```

### Phase 3: Map to Rules

```javascript
function mapChangedFilesToRules(changedFiles) {
  const rules = [];
  
  for (const docFile of changedFiles) {
    const mappings = FILE_MAPPINGS.filter(m => m.docFile === docFile);
    
    for (const mapping of mappings) {
      // Check if rule file exists and has content
      const ruleExists = fileExists(mapping.ruleFile);
      
      if (ruleExists) {
        rules.push({
          ...mapping,
          status: 'pending',
          docSize: getFileSize(mapping.docFile),
          ruleSize: getFileSize(mapping.ruleFile)
        });
      } else {
        console.warn(`⚠️  Rule file not found: ${mapping.ruleFile}`);
      }
    }
  }
  
  return rules;
}

// Example output:
// [
//   {
//     docFile: "docs/design/content-design-process.md",
//     docSection: "Step 3",
//     ruleFile: ".claude/rules/content-design-validation.md",
//     ruleSection: "Rule 5",
//     name: "Publication Strategy Framework",
//     status: "pending"
//   },
//   {
//     docFile: "docs/development/yaml-guide.md",
//     docSection: "Required Attributes",
//     ruleFile: ".claude/rules/legit-yaml.md",
//     ruleSection: "Required Attributes",
//     name: "YAML Requirements",
//     status: "pending"
//   }
// ]
```

### Phase 4: Compare Versions

```javascript
function compareAllVersions(rules) {
  const comparisons = [];
  
  for (const rule of rules) {
    try {
      const comparison = readAndCompare(rule);
      
      comparison.syncStatus = comparison.isSynced ? 'SYNCED' : 'OUT_OF_SYNC';
      comparison.changesNeeded = !comparison.isSynced;
      
      comparisons.push(comparison);
      
    } catch (error) {
      console.error(`Error comparing ${rule.name}: ${error.message}`);
      comparisons.push({
        ...rule,
        error: error.message,
        syncStatus: 'ERROR'
      });
    }
  }
  
  return comparisons;
}

// Real example execution:
const comparisons = compareAllVersions([...]);
// Returns array with each having:
// {
//   syncStatus: 'SYNCED' | 'OUT_OF_SYNC' | 'ERROR',
//   lastSync: '2026-08-20',
//   differences: [...],
//   recommendations: [...]
// }
```

### Phase 5: Generate Recommendations

```javascript
function generateRecommendations(comparisons) {
  const recommendations = [];
  
  for (const comp of comparisons) {
    if (comp.error) {
      recommendations.push({
        type: 'ERROR',
        rule: comp.ruleFile,
        message: `Error reading: ${comp.error}`,
        action: 'INVESTIGATE'
      });
      continue;
    }
    
    if (comp.isSynced) {
      recommendations.push({
        type: 'SYNCED',
        rule: comp.ruleFile,
        name: comp.name,
        lastSync: comp.lastSync,
        message: `✓ In sync (last updated ${comp.lastSync})`,
        action: 'NONE'
      });
      continue;
    }
    
    // Out of sync - generate specific recommendations
    const specificRecs = comp.differences.map(diff => ({
      action: diff.type === 'MISSING_IN_RULE' ? 'ADD' : 'REMOVE',
      text: diff.phrase,
      severity: diff.severity
    }));
    
    recommendations.push({
      type: 'OUT_OF_SYNC',
      rule: comp.ruleFile,
      name: comp.name,
      message: `⚠️  Out of sync (${comp.differences.length} changes needed)`,
      changes: specificRecs,
      action: 'REVIEW_AND_APPLY',
      lastSync: comp.lastSync
    });
  }
  
  // Sort: errors first, then out-of-sync, then synced
  return recommendations.sort((a, b) => {
    const priority = { 'ERROR': 0, 'OUT_OF_SYNC': 1, 'SYNCED': 2 };
    return priority[a.type] - priority[b.type];
  });
}
```

### Phase 6: Present to User

```javascript
function presentFindings(recommendations) {
  // Display header
  console.log('\n═══════════════════════════════════════════════════════════');
  console.log('  DOCUMENTATION SYNC STATUS');
  console.log('═══════════════════════════════════════════════════════════\n');
  
  const grouped = {
    errors: recommendations.filter(r => r.type === 'ERROR'),
    outOfSync: recommendations.filter(r => r.type === 'OUT_OF_SYNC'),
    synced: recommendations.filter(r => r.type === 'SYNCED')
  };
  
  // Summary line
  console.log(`Total checks: ${recommendations.length}`);
  if (grouped.errors.length > 0) console.log(`Errors: ${grouped.errors.length} 🔴`);
  console.log(`Out of sync: ${grouped.outOfSync.length} ⚠️`);
  console.log(`In sync: ${grouped.synced.length} ✓\n`);
  
  // Errors (if any)
  if (grouped.errors.length > 0) {
    console.log('ERRORS:');
    for (const err of grouped.errors) {
      console.log(`  🔴 ${err.rule}`);
      console.log(`     ${err.message}\n`);
    }
  }
  
  // Out of sync
  if (grouped.outOfSync.length > 0) {
    console.log('OUT OF SYNC:');
    for (const item of grouped.outOfSync) {
      console.log(`  ⚠️  ${item.name}`);
      console.log(`     File: ${item.rule}`);
      console.log(`     Changes needed: ${item.changes.length}`);
      
      for (const change of item.changes.slice(0, 3)) { // Show first 3
        const symbol = change.action === 'ADD' ? '➕' : '➖';
        console.log(`     ${symbol} ${change.text}`);
      }
      
      if (item.changes.length > 3) {
        console.log(`     ... and ${item.changes.length - 3} more`);
      }
      console.log('');
    }
  }
  
  // In sync
  if (grouped.synced.length > 0) {
    console.log('IN SYNC:');
    for (const item of grouped.synced) {
      console.log(`  ✓ ${item.name} (${item.lastSync})`);
    }
    console.log('');
  }
  
  console.log('═══════════════════════════════════════════════════════════\n');
  
  return {
    totalChecks: recommendations.length,
    errors: grouped.errors.length,
    outOfSync: grouped.outOfSync.length,
    synced: grouped.synced.length,
    hasIssues: grouped.errors.length > 0 || grouped.outOfSync.length > 0,
    recommendations: recommendations
  };
}
```

### Phase 7: Get Approval

```javascript
async function awaitApproval(findings) {
  if (!findings.hasIssues) {
    console.log('✓ All documentation is in sync. No action needed.\n');
    return 'DONE';
  }
  
  console.log('What would you like to do?\n');
  console.log('☐ A) Apply all recommendations');
  console.log('☐ B) Apply partial (select specific changes)');
  console.log('☐ C) Don\'t sync (keep as is)');
  console.log('☐ D) Escalate to Aba Azeem\n');
  
  const userChoice = await getUserInput('Your choice (A/B/C/D): ');
  
  switch(userChoice.toUpperCase()) {
    case 'A':
      return 'APPLY_ALL';
    case 'B':
      return 'APPLY_PARTIAL';
    case 'C':
      return 'SKIP';
    case 'D':
      return 'ESCALATE';
    default:
      return 'SKIP';
  }
}
```

### Phase 8: Apply Changes

```javascript
async function applyChanges(findings) {
  console.log('Applying sync changes...\n');
  
  const outOfSync = findings.recommendations.filter(r => r.type === 'OUT_OF_SYNC');
  
  for (const item of outOfSync) {
    console.log(`Updating ${item.rule}...`);
    
    // Read rule file
    let ruleContent = readFile(item.rule);
    
    // Apply changes
    for (const change of item.changes) {
      if (change.action === 'ADD') {
        // Add content to rule
        ruleContent = addToRule(ruleContent, change.text);
      } else if (change.action === 'REMOVE') {
        // Remove outdated content
        ruleContent = removeFromRule(ruleContent, change.text);
      }
    }
    
    // Add sync date stamp
    const today = getDateYYYYMMDD();
    ruleContent = addSyncDateStamp(ruleContent, today);
    
    // Write back
    writeFile(item.rule, ruleContent);
    console.log(`  ✓ Updated\n`);
  }
  
  console.log('Sync changes applied successfully!');
  console.log('\nNext step: Review and commit changes');
  console.log('  git status');
  console.log('  git add .');
  console.log('  git commit -m "sync: [description of changes]"');
  console.log('  git push origin main\n');
}
```

---

## Error Handling

The integration handles these cases:

```javascript
try {
  const changedFiles = detectChangedDocFiles();
  const mappings = mapChangedFilesToRules(changedFiles);
  const comparisons = compareAllVersions(mappings);
  const recommendations = generateRecommendations(comparisons);
  const findings = presentFindings(recommendations);
  
} catch (error) {
  if (error.type === 'GIT_NOT_AVAILABLE') {
    console.error('Git not available. Cannot detect changes.');
    console.error('Make sure you\'re in a git repository.');
  } else if (error.type === 'FILE_NOT_FOUND') {
    console.error(`File not found: ${error.file}`);
    console.error('Check that the file path is correct.');
  } else if (error.type === 'PERMISSION_DENIED') {
    console.error('Permission denied reading files.');
    console.error('Check file permissions.');
  } else {
    console.error(`Unexpected error: ${error.message}`);
  }
  
  process.exit(1);
}
```

---

## Real Usage Examples

### Example 1: Basic Sync Check

```
USER: "Check if documentation is in sync"

EXECUTION:
  ✓ Detecting changes...
  ✓ No changes detected
  
OUTPUT:
  ✓ All documentation is in sync. No action needed.
```

### Example 2: Out of Sync

```
USER: "Run sync check"

EXECUTION:
  ✓ Detecting changes...
    Found: docs/design/content-design-process.md
  ✓ Mapping to rules...
    Found: Rule 5 (content-design-validation.md)
  ✓ Comparing...
    Status: OUT OF SYNC
    Differences: 3 changes needed

OUTPUT:
  ⚠️  OUT OF SYNC:
  
  Publication Strategy Framework
  File: .claude/rules/content-design-validation.md
  Changes needed: 3
  ➕ Add: "6-factor decision framework"
  ➕ Add: "Hardware Requirements"
  ➕ Add: "Content Complexity"
  
  What would you like to do?
  ☐ A) Apply all recommendations
  ☐ B) Apply partial
  ☐ C) Don't sync
  ☐ D) Escalate
```

### Example 3: Apply Changes

```
USER: "A" (Apply all)

EXECUTION:
  ✓ Applying sync changes...
  ✓ Updated .claude/rules/content-design-validation.md
  ✓ Added sync date: 2026-08-20

OUTPUT:
  Sync changes applied successfully!
  
  Next step: Review and commit changes
    git status
    git add .
    git commit -m "sync: add publication strategy decision framework"
    git push origin main
```

---

## Summary

The integration layer:

✅ Parses user requests  
✅ Detects changes using git  
✅ Maps to corresponding rules  
✅ Compares versions  
✅ Generates specific recommendations  
✅ Presents findings clearly  
✅ Gets team approval  
✅ Applies approved changes  
✅ Handles errors gracefully  

Result: A fully functional `/sync-docs-to-rules` skill that actively detects and manages documentation sync.
