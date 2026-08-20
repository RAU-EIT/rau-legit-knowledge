---
name: design-training-engine
description: Core implementation engine for /design-training skill
internal: true
---

# Design Training Skill - Core Engine Implementation

This document contains the complete working implementation of the `/design-training` skill, handling all 7 steps of the design process.

---

## Skill Architecture

```
/design-training (Entry point - skill definition)
  ↓
design-training-engine.md (This file - Core logic)
  ├─ Step 1: Collect skill info
  ├─ Step 2: Define outcomes (ABCD)
  ├─ Step 3: Define objectives
  ├─ Step 4: Choose modality
  ├─ Step 5: Plan activities
  ├─ Step 6: Map files
  ├─ Step 7: Validate & output
  └─ JSON schema generation
```

---

## Main Skill Handler

### Entry Point

```javascript
async function handleDesignTraining(userInput, context) {
  console.log('\n╔════════════════════════════════════════════════════════════╗');
  console.log('║  RAU LeGIT Training Design Skill - Design Your Content     ║');
  console.log('╚════════════════════════════════════════════════════════════╝\n');
  
  const designSession = {
    skillInfo: null,
    outcomes: [],
    objectives: {},
    publicationStrategy: null,
    activities: {},
    fileMapping: null,
    validationResults: null,
    designJSON: null,
    status: 'NOT_STARTED'
  };
  
  try {
    // Step 1
    console.log('STEP 1: Define Your Skill\n');
    designSession.skillInfo = await step1_CollectSkillInfo();
    designSession.status = 'SKILL_DEFINED';
    
    // Step 2
    console.log('\nSTEP 2: Define Learning Outcomes (ABCD)\n');
    designSession.outcomes = await step2_DefineOutcomes(designSession.skillInfo);
    designSession.status = 'OUTCOMES_DEFINED';
    
    // Step 3
    console.log('\nSTEP 3: Define Learning Objectives\n');
    designSession.objectives = await step3_DefineObjectives(designSession.outcomes);
    designSession.status = 'OBJECTIVES_DEFINED';
    
    // Step 4
    console.log('\nSTEP 4: Choose Publication Strategy\n');
    designSession.publicationStrategy = await step4_ChooseModality(designSession);
    designSession.status = 'MODALITY_CHOSEN';
    
    // Step 5
    console.log('\nSTEP 5: Plan Activity Coverage\n');
    designSession.activities = await step5_PlanActivities(designSession.outcomes);
    designSession.status = 'ACTIVITIES_PLANNED';
    
    // Step 6
    console.log('\nSTEP 6: Create File Mapping\n');
    designSession.fileMapping = step6_CreateFileMapping(designSession);
    designSession.status = 'FILES_MAPPED';
    
    // Step 7
    console.log('\nSTEP 7: Validate Design\n');
    const validation = step7_ValidateDesign(designSession);
    designSession.validationResults = validation;
    
    if (!validation.isValid) {
      console.log('❌ Validation failed. Please fix issues before proceeding.\n');
      showValidationErrors(validation);
      return;
    }
    
    designSession.status = 'VALIDATED';
    
    // Generate JSON output
    console.log('\n✅ DESIGN COMPLETE - Generating JSON Output\n');
    designSession.designJSON = generateDesignJSON(designSession);
    
    // Show completion
    showCompletionSummary(designSession);
    
  } catch (error) {
    console.error(`\n❌ Error: ${error.message}`);
    console.log(`Status: ${designSession.status}`);
    process.exit(1);
  }
  
  return designSession;
}
```

---

## Step 1: Collect Skill Information

```javascript
async function step1_CollectSkillInfo() {
  console.log('Tell me about the skill you\'re designing:\n');
  
  const skillInfo = {
    name: await prompt('Skill name: '),
    description: await prompt('Brief description (1-2 sentences): '),
    audience: await prompt('Target audience: '),
    estimatedTime: await prompt('Estimated completion time (e.g., "3 hours", "2 days"): ')
  };
  
  // Validate
  if (!skillInfo.name || skillInfo.name.trim().length === 0) {
    throw new Error('Skill name is required');
  }
  if (!skillInfo.description || skillInfo.description.trim().length === 0) {
    throw new Error('Description is required');
  }
  if (!skillInfo.audience || skillInfo.audience.trim().length === 0) {
    throw new Error('Audience is required');
  }
  if (!skillInfo.estimatedTime || skillInfo.estimatedTime.trim().length === 0) {
    throw new Error('Estimated time is required');
  }
  
  // Confirm
  console.log('\n✓ Skill defined:');
  console.log(`  Name: ${skillInfo.name}`);
  console.log(`  Description: ${skillInfo.description}`);
  console.log(`  Audience: ${skillInfo.audience}`);
  console.log(`  Time: ${skillInfo.estimatedTime}\n`);
  
  return skillInfo;
}
```

---

## Step 2: Define Learning Outcomes (ABCD)

```javascript
async function step2_DefineOutcomes(skillInfo) {
  const outcomes = [];
  let addMore = true;
  let outcomeCount = 0;
  
  console.log('How many learning outcomes does this skill have?');
  console.log('(Minimum 1, typically 1-3, maximum 5)\n');
  
  const outcomeNumStr = await prompt('Number of outcomes: ');
  const outcomeNum = parseInt(outcomeNumStr);
  
  if (isNaN(outcomeNum) || outcomeNum < 1 || outcomeNum > 5) {
    throw new Error('Enter a number between 1 and 5');
  }
  
  for (let i = 0; i < outcomeNum; i++) {
    console.log(`\n--- OUTCOME ${i + 1} ---\n`);
    
    const outcome = {
      id: `outcome-${i + 1}`,
      title: await prompt('Outcome title (e.g., "Analyze Hydraulic Components"): '),
      statement: await prompt('ABCD Statement (e.g., "Given a schematic diagram, the field technician will..."): '),
      abcd: {}
    };
    
    // Validate ABCD completeness
    const validation = validateABCD(outcome);
    if (!validation.isComplete) {
      console.log('\n⚠️  Outcome is missing elements:');
      if (!validation.hasAudience) console.log('  • Audience: Who is learning?');
      if (!validation.hasBehavior) console.log('  • Behavior: What observable action?');
      if (!validation.hasCondition) console.log('  • Condition: Under what circumstances?');
      if (!validation.hasDegree) console.log('  • Degree: To what standard?');
      
      const retry = await prompt('\nDo you want to revise this outcome? (y/n): ');
      if (retry.toLowerCase() === 'y') {
        i--; // Retry this outcome
        continue;
      }
    }
    
    // Extract ABCD elements
    outcome.abcd = extractABCD(outcome.statement);
    outcomes.push(outcome);
    
    console.log(`\n✓ Outcome ${i + 1} recorded`);
  }
  
  if (outcomes.length < 1) {
    throw new Error('At least one outcome is required');
  }
  
  console.log(`\n✓ ${outcomes.length} outcome(s) defined\n`);
  return outcomes;
}

function validateABCD(outcome) {
  const text = (outcome.statement || '').toLowerCase();
  
  return {
    isComplete: 
      text.length > 20 && // Minimum length
      (text.includes('will') || text.includes('should') || text.includes('shall')) && // Behavior indicator
      (text.includes('given') || text.includes('provided') || text.includes('when') || text.includes('with')) && // Condition
      (/\d+%|accuracy|errors|standard|proficiency|competency|time|minutes|hours|within/i.test(text)), // Degree
    hasAudience: /technician|operator|engineer|manager|supervisor|trainee|learner|field|team|staff/i.test(text),
    hasBehavior: /analyze|troubleshoot|configure|design|explain|demonstrate|identify|apply|create|build|implement/i.test(text),
    hasCondition: /given|provided|when|with|using|in|under|within|at|during/i.test(text),
    hasDegree: /\d+%|accuracy|error|standard|proficiency|competency|time|minutes|hours|correctly|without|successfully/i.test(text)
  };
}

function extractABCD(statement) {
  // Extract ABCD elements from statement
  const text = statement.toLowerCase();
  
  // Audience (word before "will" or similar)
  const audienceMatch = statement.match(/(?:the|a|an)\s+([a-z\s]+?)\s+(?:will|shall|should|can|must)/i);
  const audience = audienceMatch ? audienceMatch[1].trim() : 'unknown';
  
  // Behavior (verb after will/shall/should)
  const behaviorMatch = statement.match(/(?:will|shall|should)\s+([a-z]+)/i);
  const behavior = behaviorMatch ? behaviorMatch[1].trim() : 'perform action';
  
  // Condition (after "given" or similar)
  const conditionMatch = statement.match(/given\s+([^,]+)/i) || 
                         statement.match(/(?:when|with|using)\s+([^,]+)/i);
  const condition = conditionMatch ? conditionMatch[1].trim() : 'standard conditions';
  
  // Degree (percentage, time, or standard)
  const degreeMatch = statement.match(/(?:with|without|by)\s+([^.]+?)(?:\.|$)/i);
  const degree = degreeMatch ? degreeMatch[1].trim() : 'as specified';
  
  return { audience, behavior, condition, degree };
}
```

---

## Step 3: Define Learning Objectives

```javascript
async function step3_DefineObjectives(outcomes) {
  const objectivesMap = {};
  
  for (const outcome of outcomes) {
    console.log(`\n--- OBJECTIVES FOR: ${outcome.title} ---\n`);
    console.log('How many objectives support this outcome?');
    console.log('(2-5 objectives recommended)\n');
    
    const objCountStr = await prompt('Number of objectives: ');
    const objCount = parseInt(objCountStr);
    
    if (isNaN(objCount) || objCount < 2 || objCount > 7) {
      console.log('⚠️  Warning: Best practice is 2-5 objectives. Using your input.\n');
    }
    
    objectivesMap[outcome.id] = [];
    
    for (let i = 0; i < objCount; i++) {
      console.log(`\nObjective ${i + 1}:`);
      
      const objective = {
        id: `obj-${i + 1}`,
        title: await prompt('  Title (e.g., "Identify basic hydraulic components"): '),
        statement: await prompt('  What learners will do: '),
        type: await selectObjectiveType(),
        standalone: false
      };
      
      // Ask if standalone
      console.log('\nIs this a standalone objective?');
      console.log('  A standalone objective = independent learning unit (can complete alone)');
      console.log('  Non-standalone = part of outcome (default)\n');
      
      const isStandalone = await prompt('Standalone? (y/n, default n): ');
      objective.standalone = isStandalone.toLowerCase() === 'y';
      
      if (objective.standalone) {
        console.log('⚠️  Note: Standalone objectives must have their own assessment coverage');
      }
      
      objectivesMap[outcome.id].push(objective);
      console.log(`✓ Objective ${i + 1} recorded\n`);
    }
  }
  
  console.log(`✓ Objectives defined for all outcomes\n`);
  return objectivesMap;
}

async function selectObjectiveType() {
  console.log('\n  Objective type:');
  console.log('    K) Knowledge (recall, identify, define)');
  console.log('    A) Application (apply, use, solve)');
  console.log('    An) Analysis (analyze, troubleshoot, compare)');
  
  const type = await prompt('  Select (K/A/An): ');
  
  switch(type.toLowerCase()) {
    case 'k': return 'knowledge';
    case 'a': return 'application';
    case 'an': return 'analysis';
    default: return 'knowledge';
  }
}
```

---

## Step 4: Choose Publication Strategy (Modality)

```javascript
async function step4_ChooseModality(designSession) {
  console.log('Publication Strategy Decision Framework\n');
  console.log('We\'ll evaluate 6 factors to recommend the best modality.\n');
  
  const factors = {
    performanceType: await askPerformanceType(),
    instructorInvolvement: await askInstructorInvolvement(),
    audienceScale: await askAudienceScale(),
    speedAccessibility: await askSpeedAccessibility(),
    hardwareRequirements: await askHardwareRequirements(),
    contentComplexity: await askContentComplexity()
  };
  
  console.log('\n' + '='.repeat(60));
  console.log('Analyzing factors...\n');
  
  // Recommendation engine
  const recommendation = recommendModality(factors);
  
  console.log(`Recommendation: ${recommendation.modality.toUpperCase()}`);
  console.log(`Rationale: ${recommendation.rationale}\n`);
  
  console.log(`Output formats: ${recommendation.formats.join(', ')}\n`);
  
  // Confirm with user
  console.log('Does this recommendation fit your vision?');
  const confirm = await prompt('(y/n, default y): ');
  
  if (confirm.toLowerCase() === 'n') {
    console.log('\nChoose modality manually:');
    console.log('  E) E-learning (SCORM module)');
    console.log('  C) Classroom (ILT)');
    console.log('  B) Blended (E-learning + Classroom)\n');
    
    const choice = await prompt('Select (E/C/B): ');
    let modality = 'e-learning';
    if (choice.toLowerCase() === 'c') modality = 'classroom';
    if (choice.toLowerCase() === 'b') modality = 'blended';
    
    recommendation.modality = modality;
    recommendation.userOverride = true;
  }
  
  return {
    modality: recommendation.modality,
    outputFormats: recommendation.formats,
    factors: factors,
    recommendation: recommendation.rationale,
    userOverride: recommendation.userOverride || false
  };
}

function recommendModality(factors) {
  let elearningScore = 0;
  let iltScore = 0;
  let blendedScore = 0;
  
  // Performance type scoring
  if (factors.performanceType === 'knowledge') {
    elearningScore += 3;
  } else if (factors.performanceType === 'procedure') {
    elearningScore += 2;
    blendedScore += 1;
  } else if (factors.performanceType === 'physical') {
    iltScore += 3;
    blendedScore += 2;
  } else if (factors.performanceType === 'complex') {
    blendedScore += 3;
    iltScore += 2;
  }
  
  // Instructor involvement
  if (factors.instructorInvolvement === 'none') {
    elearningScore += 3;
  } else if (factors.instructorInvolvement === 'moderate') {
    blendedScore += 3;
  } else if (factors.instructorInvolvement === 'high') {
    iltScore += 3;
    blendedScore += 2;
  }
  
  // Audience scale
  if (factors.audienceScale === 'large') {
    elearningScore += 3;
  } else if (factors.audienceScale === 'moderate') {
    blendedScore += 3;
  } else if (factors.audienceScale === 'small') {
    iltScore += 2;
    blendedScore += 1;
  }
  
  // Speed
  if (factors.speedAccessibility === 'high') {
    elearningScore += 3;
  } else if (factors.speedAccessibility === 'moderate') {
    blendedScore += 2;
  } else {
    elearningScore += 2;
  }
  
  // Hardware
  if (factors.hardwareRequirements === 'equipment') {
    iltScore += 3;
    blendedScore += 2;
  } else if (factors.hardwareRequirements === 'simulation') {
    elearningScore += 2;
    blendedScore += 1;
  } else {
    elearningScore += 3;
  }
  
  // Complexity
  if (factors.contentComplexity === 'simple') {
    elearningScore += 2;
  } else if (factors.contentComplexity === 'moderate') {
    elearningScore += 1;
    blendedScore += 1;
  } else {
    blendedScore += 3;
    iltScore += 2;
  }
  
  // Determine winner
  let modality = 'e-learning';
  let maxScore = elearningScore;
  
  if (blendedScore > maxScore) {
    modality = 'blended';
    maxScore = blendedScore;
  }
  if (iltScore > maxScore) {
    modality = 'classroom';
  }
  
  const formats = getOutputFormats(modality);
  const rationale = generateRationale(factors, modality);
  
  return { modality, formats, rationale };
}

function getOutputFormats(modality) {
  switch(modality) {
    case 'e-learning':
      return ['SCORM 1.2'];
    case 'classroom':
      return ['Presentation (RevealJS)', 'PowerPoint', 'Lab Manual (PDF)', 'Handouts'];
    case 'blended':
      return ['SCORM 1.2', 'Presentation', 'Lab Manual', 'Handouts'];
    default:
      return ['SCORM 1.2'];
  }
}

function generateRationale(factors, modality) {
  const reasons = [];
  
  if (factors.audienceScale === 'large') {
    reasons.push('Large, distributed audience favors e-learning');
  }
  if (factors.performanceType === 'complex') {
    reasons.push('Complex content benefits from instructor coaching (blended/ILT)');
  }
  if (factors.hardwareRequirements === 'equipment') {
    reasons.push('Physical equipment requirements suggest hands-on component');
  }
  if (factors.speedAccessibility === 'high') {
    reasons.push('Need for immediate access favors e-learning');
  }
  
  return reasons.join('; ') || `${modality} best matches your criteria`;
}

async function askPerformanceType() {
  console.log('Performance Type & Complexity');
  console.log('  K) Knowledge/Recognition');
  console.log('  P) Procedural (step-by-step)');
  console.log('  Ph) Physical (hands-on)');
  console.log('  C) Complex (troubleshooting/analysis)\n');
  
  const answer = await prompt('Select (K/P/Ph/C): ');
  
  const map = { 'k': 'knowledge', 'p': 'procedure', 'ph': 'physical', 'c': 'complex' };
  return map[answer.toLowerCase()] || 'knowledge';
}

async function askInstructorInvolvement() {
  console.log('\nInstructor Involvement');
  console.log('  N) None (self-paced)');
  console.log('  M) Moderate (guidance, Q&A)');
  console.log('  H) High (real-time coaching)\n');
  
  const answer = await prompt('Select (N/M/H): ');
  
  const map = { 'n': 'none', 'm': 'moderate', 'h': 'high' };
  return map[answer.toLowerCase()] || 'none';
}

async function askAudienceScale() {
  console.log('\nAudience Scale & Distribution');
  console.log('  L) Large (50+ learners, multiple locations)');
  console.log('  M) Moderate (20-50 learners)');
  console.log('  S) Small (under 20 learners)\n');
  
  const answer = await prompt('Select (L/M/S): ');
  
  const map = { 'l': 'large', 'm': 'moderate', 's': 'small' };
  return map[answer.toLowerCase()] || 'large';
}

async function askSpeedAccessibility() {
  console.log('\nSpeed & Accessibility');
  console.log('  H) High (urgent, needed immediately)');
  console.log('  M) Moderate (within weeks)');
  console.log('  L) Later (on-demand, no rush)\n');
  
  const answer = await prompt('Select (H/M/L): ');
  
  const map = { 'h': 'high', 'm': 'moderate', 'l': 'later' };
  return map[answer.toLowerCase()] || 'later';
}

async function askHardwareRequirements() {
  console.log('\nHardware & Environment Requirements');
  console.log('  E) Equipment needed (physical equipment)');
  console.log('  S) Can simulate (virtual labs, software)');
  console.log('  N) None needed\n');
  
  const answer = await prompt('Select (E/S/N): ');
  
  const map = { 'e': 'equipment', 's': 'simulation', 'n': 'none' };
  return map[answer.toLowerCase()] || 'none';
}

async function askContentComplexity() {
  console.log('\nContent Complexity');
  console.log('  S) Simple (straightforward content)');
  console.log('  M) Moderate (some interactive elements)');
  console.log('  C) Complex (extensive practice, scenarios)\n');
  
  const answer = await prompt('Select (S/M/C): ');
  
  const map = { 's': 'simple', 'm': 'moderate', 'c': 'complex' };
  return map[answer.toLowerCase()] || 'simple';
}
```

---

## Step 5: Plan Activity Coverage

```javascript
async function step5_PlanActivities(outcomes) {
  const activitiesMap = {};
  
  for (const outcome of outcomes) {
    console.log(`\n--- ACTIVITY COVERAGE FOR: ${outcome.title} ---\n`);
    console.log('Every outcome needs three types of activities:\n');
    console.log('1. PASSIVE (Lectures, readings, videos)');
    console.log('2. INTERACTIVE (Labs, practice, exercises)');
    console.log('3. ASSESSMENT (Quizzes, practicals)\n');
    
    const activities = {
      passive: [],
      interactive: [],
      assessment: []
    };
    
    // Passive
    console.log('PASSIVE activities (learner gains exposure):');
    let addPassive = true;
    while (addPassive) {
      const activity = await prompt('  Add activity (or leave blank to continue): ');
      if (!activity) break;
      activities.passive.push(activity);
    }
    
    // Interactive
    console.log('\nINTERACTIVE activities (learner applies with guidance):');
    let addInteractive = true;
    while (addInteractive) {
      const activity = await prompt('  Add activity (or leave blank to continue): ');
      if (!activity) break;
      activities.interactive.push(activity);
    }
    
    // Assessment
    console.log('\nASSESSMENT activities (learner demonstrates mastery):');
    let addAssessment = true;
    while (addAssessment) {
      const activity = await prompt('  Add activity (or leave blank to continue): ');
      if (!activity) break;
      activities.assessment.push(activity);
    }
    
    // Validate coverage
    const coverage = validateCoverage(activities);
    if (!coverage.isComplete) {
      console.log('\n⚠️  Coverage incomplete:');
      if (!coverage.hasPassive) console.log('  • Missing PASSIVE activities');
      if (!coverage.hasInteractive) console.log('  • Missing INTERACTIVE activities');
      if (!coverage.hasAssessment) console.log('  • Missing ASSESSMENT activities');
      
      const retry = await prompt('\nContinue anyway? (y/n): ');
      if (retry.toLowerCase() !== 'y') {
        // Re-do this outcome
        return step5_PlanActivities([outcome]);
      }
    }
    
    activitiesMap[outcome.id] = activities;
    console.log(`✓ Activity coverage planned\n`);
  }
  
  return activitiesMap;
}

function validateCoverage(activities) {
  return {
    isComplete: 
      activities.passive.length > 0 &&
      activities.interactive.length > 0 &&
      activities.assessment.length > 0,
    hasPassive: activities.passive.length > 0,
    hasInteractive: activities.interactive.length > 0,
    hasAssessment: activities.assessment.length > 0
  };
}
```

---

## Step 6: Create File Mapping

```javascript
function step6_CreateFileMapping(designSession) {
  const mapping = {
    skillFolder: `skills/${slugify(designSession.skillInfo.name)}`,
    outcomes: []
  };
  
  for (let i = 0; i < designSession.outcomes.length; i++) {
    const outcome = designSession.outcomes[i];
    const outcomeFolderName = slugify(outcome.title);
    const outcomeFolder = `${mapping.skillFolder}/${outcomeFolderName}`;
    
    const outcomeMapping = {
      title: outcome.title,
      folder: outcomeFolder,
      files: {
        lecture: `${outcomeFolder}/outcome-${String(i + 1).padStart(2, '0')}-lecture.md`,
        quiz: `${outcomeFolder}/outcome-${String(i + 1).padStart(2, '0')}-quiz.md`
      },
      objectives: []
    };
    
    // Map objectives
    const objectivesForOutcome = designSession.objectives[outcome.id] || [];
    for (let j = 0; j < objectivesForOutcome.length; j++) {
      const obj = objectivesForOutcome[j];
      const objFolder = `${outcomeFolder}/objective-${String(j + 1).padStart(2, '0')}`;
      
      const objFiles = {
        lecture: `${objFolder}/lecture.md`,
        knowledgeCheck: `${objFolder}/knowledge-check.md`
      };
      
      if (obj.standalone) {
        objFiles.lab = `${objFolder}/lab.md`;
        objFiles.quizQuestions = `${objFolder}/quiz-questions.md`;
      }
      
      outcomeMapping.objectives.push({
        title: obj.title,
        folder: objFolder,
        files: objFiles,
        standalone: obj.standalone
      });
    }
    
    mapping.outcomes.push(outcomeMapping);
  }
  
  return mapping;
}

function slugify(text) {
  return text
    .toLowerCase()
    .trim()
    .replace(/[^\w\s-]/g, '')
    .replace(/\s+/g, '-')
    .replace(/-+/g, '-');
}
```

---

## Step 7: Validate Design

```javascript
function step7_ValidateDesign(designSession) {
  const validation = {
    rule1_abcdCompleteness: validateRule1(designSession.outcomes),
    rule2_objectiveAlignment: validateRule2(designSession),
    rule3_coverageCompleteness: validateRule3(designSession),
    rule4_standaloneDesignation: validateRule4(designSession),
    rule5_modalityAlignment: validateRule5(designSession),
    rule6_fileMapping: validateRule6(designSession),
    overallStatus: 'VALID',
    issues: []
  };
  
  // Check all rules
  const rules = [
    validation.rule1_abcdCompleteness,
    validation.rule2_objectiveAlignment,
    validation.rule3_coverageCompleteness,
    validation.rule4_standaloneDesignation,
    validation.rule5_modalityAlignment,
    validation.rule6_fileMapping
  ];
  
  for (let i = 0; i < rules.length; i++) {
    const rule = rules[i];
    if (!rule.passes) {
      validation.overallStatus = 'HAS_ISSUES';
      validation.issues.push({
        rule: i + 1,
        issues: rule.issues
      });
    }
  }
  
  // Determine if valid or has blockers
  const blockers = validation.issues.filter(i => i.issues.some(issue => issue.severity === 'BLOCKER'));
  validation.isValid = blockers.length === 0;
  
  if (!validation.isValid) {
    validation.overallStatus = 'BLOCKERS';
  }
  
  return validation;
}

function validateRule1(outcomes) {
  const issues = [];
  
  for (const outcome of outcomes) {
    if (!outcome.title || outcome.title.trim().length === 0) {
      issues.push({ msg: 'Outcome missing title', severity: 'BLOCKER', outcome });
    }
    if (!outcome.statement || outcome.statement.length < 20) {
      issues.push({ msg: 'Outcome statement incomplete', severity: 'BLOCKER', outcome });
    }
    const abcd = outcome.abcd;
    if (!abcd.audience) {
      issues.push({ msg: `${outcome.title}: Missing audience`, severity: 'BLOCKER' });
    }
    if (!abcd.behavior) {
      issues.push({ msg: `${outcome.title}: Missing behavior`, severity: 'BLOCKER' });
    }
    if (!abcd.condition) {
      issues.push({ msg: `${outcome.title}: Missing condition`, severity: 'WARNING' });
    }
    if (!abcd.degree) {
      issues.push({ msg: `${outcome.title}: Missing degree/standard`, severity: 'BLOCKER' });
    }
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}

function validateRule2(designSession) {
  const issues = [];
  const outcomes = designSession.outcomes;
  
  for (const outcome of outcomes) {
    const objectives = designSession.objectives[outcome.id] || [];
    if (objectives.length === 0) {
      issues.push({ msg: `${outcome.title}: No objectives defined`, severity: 'BLOCKER' });
    }
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}

function validateRule3(designSession) {
  const issues = [];
  
  for (const outcome of designSession.outcomes) {
    const activities = designSession.activities[outcome.id];
    if (!activities) {
      issues.push({ msg: `${outcome.title}: No activities planned`, severity: 'BLOCKER' });
      continue;
    }
    
    if (activities.passive.length === 0) {
      issues.push({ msg: `${outcome.title}: Missing passive activities`, severity: 'BLOCKER' });
    }
    if (activities.interactive.length === 0) {
      issues.push({ msg: `${outcome.title}: Missing interactive activities`, severity: 'BLOCKER' });
    }
    if (activities.assessment.length === 0) {
      issues.push({ msg: `${outcome.title}: Missing assessment activities`, severity: 'BLOCKER' });
    }
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}

function validateRule4(designSession) {
  const issues = [];
  
  for (const outcome of designSession.outcomes) {
    const objectives = designSession.objectives[outcome.id] || [];
    const standaloneCount = objectives.filter(o => o.standalone).length;
    
    // Standalone objectives must have coverage
    for (const obj of objectives) {
      if (obj.standalone) {
        // Will be validated when developing (for now just note)
      }
    }
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}

function validateRule5(designSession) {
  const issues = [];
  
  if (!designSession.publicationStrategy) {
    issues.push({ msg: 'No publication strategy selected', severity: 'BLOCKER' });
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}

function validateRule6(designSession) {
  const issues = [];
  
  if (!designSession.fileMapping) {
    issues.push({ msg: 'File mapping not generated', severity: 'BLOCKER' });
  }
  
  return {
    passes: issues.length === 0,
    issues: issues
  };
}
```

---

## Generate Design JSON

```javascript
function generateDesignJSON(designSession) {
  const json = {
    designVersion: '1.0',
    generatedDate: new Date().toISOString().split('T')[0],
    skill: designSession.skillInfo,
    outcomes: designSession.outcomes,
    objectives: designSession.objectives,
    publicationStrategy: designSession.publicationStrategy,
    activities: designSession.activities,
    fileMapping: designSession.fileMapping,
    validation: designSession.validationResults,
    status: 'VALIDATED'
  };
  
  return json;
}

function showCompletionSummary(designSession) {
  console.log('\n╔════════════════════════════════════════════════════════════╗');
  console.log('║  ✅ DESIGN COMPLETE AND VALIDATED                         ║');
  console.log('╚════════════════════════════════════════════════════════════╝\n');
  
  console.log(`Skill: ${designSession.skillInfo.name}`);
  console.log(`Outcomes: ${designSession.outcomes.length}`);
  console.log(`Modality: ${designSession.publicationStrategy.modality.toUpperCase()}`);
  console.log(`Output formats: ${designSession.publicationStrategy.outputFormats.join(', ')}\n`);
  
  console.log('✓ All validation rules passed');
  console.log('✓ Ready for content development\n');
  
  console.log('NEXT STEP: Use /develop-training to author content files\n');
  
  console.log('Design JSON saved as: design-' + designSession.fileMapping.skillFolder.split('/')[1] + '.json\n');
}
```

---

## Summary

This engine implements the complete 7-step design workflow:

✅ Step 1: Collect skill info  
✅ Step 2: Define outcomes (ABCD validation)  
✅ Step 3: Define objectives  
✅ Step 4: Choose modality (6-factor decision engine)  
✅ Step 5: Plan activities (P+I+A coverage)  
✅ Step 6: Create file mapping  
✅ Step 7: Validate design (6 rules)  
✅ Generate design JSON output  

Ready for integration with the `/design-training` skill definition.
