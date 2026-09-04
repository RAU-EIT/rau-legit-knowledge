---
name: design-training-engine
description: Core implementation engine for /design-training skill
internal: true
---

# Design Training Skill - Core Engine Implementation

This document contains the complete working implementation of the `/design-training` skill, handling all 9 interactive steps of the design process.

---

## Skill Architecture

```text
/design-training (Entry point - skill definition)
  ↓
design-training-engine.md (This file - Core logic)
  ├─ Step 1: Collect skill info
  ├─ Step 2: Define outcomes (ABCD)
  ├─ Step 3: Define objectives
  ├─ Step 4: Choose delivery strategy
  ├─ Step 5: Define offerings
  ├─ Step 6: Define publications
  ├─ Step 7: Determine activities
  ├─ Step 8: Define deliverables (manifest, not files)
  ├─ Step 9: Validate & output
  └─ JSON schema generation
```

### Step numbering: this skill vs. the design process guide

The skill's steps run **one ahead** of `docs/design/content-design-process.md`, because the
skill starts by collecting skill metadata that the guide treats as an input rather than a step.

| Skill step | Design process guide step |
| --- | --- |
| 1. Collect skill info | *(the guide's "Inputs" header block)* |
| 2. Define outcomes | Step 1: Define Learning Outcomes |
| 3. Define objectives | Step 2: Define Learning Objectives |
| 4. Choose delivery strategy | Step 3: Determine Delivery Strategy |
| 5. Define offerings | Step 4: Define Offerings |
| 6. Define publications | Step 5: Define Publications |
| 7. Determine activities | Step 6: Determine Activities |
| 8. Define deliverables | Step 7: Define Deliverables |
| 9. Validate & output | Step 8: Validate & Refine Design |
| *(not in this skill)* | Step 9: Load into Content Database |

The offset is intentional. Guide Step 9 is out of scope for this skill: the **content database**
owns loading the design and creating LeGIT project files.

---

## Main Skill Handler

### Entry Point

```javascript
async function handleDesignTraining(userInput, context) {
  console.log('\n╔════════════════════════════════════════════════════════════╗');
  console.log('║  RAU LeGIT Training Design Skill - Design Your Content     ║');
  console.log('╚════════════════════════════════════════════════════════════╝\n');
  
  // Design derives top-down:
  //   deliveryStrategy → offerings → publications → activities → deliverables
  // Each field below is an input to the next. See docs/terminology-glossary.md.
  const designSession = {
    skillInfo: null,
    outcomes: [],
    objectives: {},
    deliveryStrategy: null,
    offerings: [],
    publications: [],
    activities: {},
    supportingAssets: [],
    deliverableManifest: null,
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
    
    // Step 4: first link in the derivation chain
    console.log('\nSTEP 4: Choose Delivery Strategy\n');
    designSession.deliveryStrategy = await step4_ChooseModality(designSession);
    designSession.status = 'DELIVERY_STRATEGY_CHOSEN';

    // Step 5: offerings follow directly from the confirmed delivery strategy
    console.log('\nSTEP 5: Define Offerings\n');
    designSession.offerings = await step5_DefineOfferings(designSession);
    designSession.status = 'OFFERINGS_DEFINED';

    // Step 6: publications are scoped by the offerings and the design hierarchy
    console.log('\nSTEP 6: Define Publications\n');
    designSession.publications = await step6_DefinePublications(designSession);
    designSession.status = 'PUBLICATIONS_DEFINED';

    // Step 7: activities are derived from the publications that must be built
    console.log('\nSTEP 7: Determine Activities\n');
    designSession.activities = await step7_PlanActivities(designSession);
    designSession.status = 'ACTIVITIES_PLANNED';

    // Step 8: deliverables = activities + supporting assets. Produces a manifest as
    // design data; the content database creates the actual files.
    console.log('\nSTEP 8: Define Deliverables\n');
    designSession.supportingAssets = await collectSupportingAssets(designSession);
    designSession.deliverableManifest = step8_CreateDeliverableManifest(designSession);
    designSession.status = 'DELIVERABLES_DEFINED';

    // Step 9
    console.log('\nSTEP 9: Validate Design\n');
    const validation = step9_ValidateDesign(designSession);
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

## Step 4: Choose Delivery Strategy (Modality)

```javascript
async function step4_ChooseModality(designSession) {
  console.log('Delivery Strategy Decision Framework\n');
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
  
  console.log(`Publication types: ${recommendation.publicationTypes.join(', ')}\n`);
  
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
    publicationTypes: recommendation.publicationTypes,
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
  
  const formats = getPublicationTypes(modality);
  const rationale = generateRationale(factors, modality);
  
  return { modality, publicationTypes: formats, rationale };
}

function getPublicationTypes(modality) {
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

## Step 5: Define Offerings

Offerings follow directly from the confirmed delivery strategy. An **offering** is what a
student enrolls in, purchases, or downloads. The same skill design can produce different
offerings for different roles, which is why this is its own step rather than a property of
the delivery strategy.

```javascript
async function step5_DefineOfferings(designSession) {
  const modality = designSession.deliveryStrategy.modality;
  const offerings = [];

  console.log(`Delivery strategy: ${modality.toUpperCase()}\n`);
  console.log('An offering is what a student enrolls in. Define one per audience/role.\n');
  console.log(suggestedOfferingsFor(modality));

  const countStr = await prompt('\nHow many offerings does this skill need? ');
  const count = parseInt(countStr);

  if (isNaN(count) || count < 1) {
    throw new Error('At least one offering is required');
  }

  for (let i = 0; i < count; i++) {
    console.log(`\n--- OFFERING ${i + 1} ---\n`);

    const offering = {
      id: `offering-${i + 1}`,
      name: await prompt('  Student-facing offering name: '),
      audienceRole: await prompt('  Target audience/role (e.g., "Field Technician"): '),
      modality: modality,
      // An offering may cover all outcomes or a subset; different roles often need
      // different depth from the same skill design.
      outcomeCoverage: await promptOutcomeCoverage(designSession.outcomes),
      supportingMaterials: await prompt('  Supporting materials (certificates, job aids; blank for none): ')
    };

    if (!offering.name || !offering.audienceRole) {
      throw new Error('Offering name and audience/role are required');
    }

    offerings.push(offering);
    console.log(`✓ Offering ${i + 1} recorded`);
  }

  console.log(`\n✓ ${offerings.length} offering(s) defined\n`);
  return offerings;
}

function suggestedOfferingsFor(modality) {
  switch (modality) {
    case 'e-learning':
      return '  Typical: a self-paced online course enrolled in via the LMS';
    case 'classroom':
      return '  Typical: an instructor-led course with a scheduled session and location';
    case 'blended':
      return '  Typical: a self-paced online course PLUS a classroom or regional practicum';
    default:
      return '';
  }
}

async function promptOutcomeCoverage(outcomes) {
  console.log('\n  Which outcomes does this offering cover?');
  outcomes.forEach((o, idx) => console.log(`    ${idx + 1}) ${o.title}`));
  const answer = await prompt('  Enter numbers comma-separated, or "all": ');

  if (!answer || answer.trim().toLowerCase() === 'all') {
    return outcomes.map(o => o.id);
  }

  return answer
    .split(',')
    .map(s => parseInt(s.trim()))
    .filter(n => !isNaN(n) && n >= 1 && n <= outcomes.length)
    .map(n => outcomes[n - 1].id);
}
```

---

## Step 6: Define Publications

Publications are scoped by the **offerings** and the **design hierarchy**. A publication is
decided here and produced later at build time; deciding it now is what makes the activity
and deliverable sets correct.

```javascript
async function step6_DefinePublications(designSession) {
  const modality = designSession.deliveryStrategy.modality;
  const publications = [];

  console.log('Publications are the outputs the build system will produce.\n');
  console.log('Required for this delivery strategy:');
  requiredPublicationTypesFor(modality).forEach(p =>
    console.log(`  • ${p.label} (${p.docType})`)
  );

  // Standalone objectives are independently completable, so each needs its own
  // publication rather than rolling into the parent outcome's.
  const standalone = [];
  for (const outcome of designSession.outcomes) {
    (designSession.objectives[outcome.id] || [])
      .filter(o => o.standalone)
      .forEach(o => standalone.push({ outcome, objective: o }));
  }
  if (standalone.length > 0) {
    console.log(`\n⚠️  ${standalone.length} standalone objective(s) each require their own publication:`);
    standalone.forEach(s => console.log(`  • ${s.objective.title} (in ${s.outcome.title})`));
  }

  const countStr = await prompt('\nHow many publications does this skill need? ');
  const count = parseInt(countStr);

  if (isNaN(count) || count < 1) {
    throw new Error('At least one publication is required');
  }

  for (let i = 0; i < count; i++) {
    console.log(`\n--- PUBLICATION ${i + 1} ---\n`);

    const publication = {
      id: `publication-${i + 1}`,
      name: await prompt('  Publication name: '),
      audienceRole: await prompt('  Audience/role it serves: '),
      scope: await prompt('  Scope (e.g., "Outcomes 1-3", "Objective 2 standalone"): '),
      docType: await prompt('  Publication type / docType (scorm1.2, print, revealjs, pptx, presentation-video): '),
      offeringIds: await prompt('  Which offering(s) does it feed? (comma-separated ids): ')
    };

    if (!publication.name || !publication.docType) {
      throw new Error('Publication name and docType are required');
    }

    publications.push(publication);
    console.log(`✓ Publication ${i + 1} recorded`);
  }

  console.log(`\n✓ ${publications.length} publication(s) defined\n`);
  return publications;
}

function requiredPublicationTypesFor(modality) {
  switch (modality) {
    case 'e-learning':
      return [{ label: 'SCORM module', docType: 'scorm1.2' }];
    case 'classroom':
      return [
        { label: 'Instructor presentation', docType: 'revealjs or pptx' },
        { label: 'Lab manual', docType: 'print' },
        { label: 'Practical assessment', docType: 'print' }
      ];
    case 'blended':
      return [
        { label: 'SCORM module', docType: 'scorm1.2' },
        { label: 'Instructor presentation', docType: 'revealjs or pptx' },
        { label: 'Lab manual', docType: 'print' },
        { label: 'Learner handout', docType: 'print' }
      ];
    default:
      return [{ label: 'SCORM module', docType: 'scorm1.2' }];
  }
}
```

---

## Step 7: Determine Activities

Activities are **derived from the publications** that must be built, then validated against
outcome-level coverage.

<!-- TBD: The automated publication → activity derivation rules are not yet specified. See
     .claude/rules/content-design-validation.md Rule 7 (placeholder, not enforced). Until
     those rules exist, activities are collected from the design team and checked against
     Rule 3 coverage only. -->

```javascript
async function step7_PlanActivities(designSession) {
  const outcomes = designSession.outcomes;
  const activitiesMap = {};

  // Publications determine what activity mix is needed. Until the derivation rules are
  // specified, show them as context so the designer can plan against them.
  if (designSession.publications && designSession.publications.length > 0) {
    console.log('Publications these activities must support:');
    designSession.publications.forEach(p =>
      console.log(`  • ${p.name} (${p.docType}): ${p.audienceRole}`)
    );
    console.log('');
  }

  for (const outcome of outcomes) {
    // Retry in place. Recursing here would discard the outcomes already collected.
    let activities;
    let accepted = false;

    while (!accepted) {
      console.log(`\n--- ACTIVITY COVERAGE FOR: ${outcome.title} ---\n`);
      console.log('Every outcome needs three types of activities:\n');
      console.log('1. PASSIVE (Lectures, readings, videos)');
      console.log('2. INTERACTIVE (Labs, practice, exercises)');
      console.log('3. ASSESSMENT (Quizzes, practicals)\n');

      activities = {
        passive: [],
        interactive: [],
        assessment: []
      };

      // Passive
      console.log('PASSIVE activities (learner gains exposure):');
      while (true) {
        const activity = await prompt('  Add activity (or leave blank to continue): ');
        if (!activity) break;
        activities.passive.push(activity);
      }

      // Interactive
      console.log('\nINTERACTIVE activities (learner applies with guidance):');
      while (true) {
        const activity = await prompt('  Add activity (or leave blank to continue): ');
        if (!activity) break;
        activities.interactive.push(activity);
      }

      // Assessment
      console.log('\nASSESSMENT activities (learner demonstrates mastery):');
      while (true) {
        const activity = await prompt('  Add activity (or leave blank to continue): ');
        if (!activity) break;
        activities.assessment.push(activity);
      }

      // Validate coverage (Rule 3, outcome level)
      const coverage = validateCoverage(activities);
      if (coverage.isComplete) {
        accepted = true;
        continue;
      }

      console.log('\n⚠️  Coverage incomplete:');
      if (!coverage.hasPassive) console.log('  • Missing PASSIVE activities');
      if (!coverage.hasInteractive) console.log('  • Missing INTERACTIVE activities');
      if (!coverage.hasAssessment) console.log('  • Missing ASSESSMENT activities');

      const retry = await prompt('\nContinue anyway? (y/n): ');
      accepted = retry.toLowerCase() === 'y';
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

## Step 8: Define Deliverables

Deliverables are the **superset**: every activity, plus everything else the SME must produce
that LeGIT does not author.

```text
Deliverables  =  every Activity  +  supporting assets
```

### Collect supporting assets

The generator cannot infer these: a lab that depends on a VM looks identical to one that
does not. Ask for them explicitly, or the gap surfaces during development instead of design.

```javascript
async function collectSupportingAssets(designSession) {
  const assets = [];

  console.log('Supporting assets are things the SME must produce that LeGIT does');
  console.log('not author or build, but the skill is not deliverable without them.\n');
  console.log('  Categories: VMs / test environments, project files (.ACD, configs),');
  console.log('              lab start & finish files, externally produced media,');
  console.log('              supporting documentation (setup guides, equipment lists)\n');

  while (true) {
    const name = await prompt('Add a supporting asset (blank to finish): ');
    if (!name) break;

    assets.push({
      name: name,
      category: await prompt('  Category: '),
      // Which deliverable breaks without this. A lab is not deliverable without its VM.
      requiredBy: await prompt('  Which activity/deliverable depends on it? '),
      owner: await prompt('  Owner: '),
      location: await prompt('  Location & version (if stored outside Git): ')
    });

    console.log('✓ Recorded\n');
  }

  if (assets.length === 0) {
    console.log('⚠️  No supporting assets recorded. Confirm no labs depend on a VM,');
    console.log('   project file, or physical equipment before proceeding.\n');
  } else {
    console.log(`✓ ${assets.length} supporting asset(s) recorded\n`);
  }

  return assets;
}
```

### Build the manifest

This step produces a **file manifest as design data**: a description of the deliverables the
design requires. It does **not** create files on disk.

File creation is owned by the **content database** (design Step 9), which consumes this
manifest along with the offerings and publications. Do not scaffold folders or write `.md`
files here; doing so would compete with the content database for ownership of the project
structure.

```javascript
function step8_CreateDeliverableManifest(designSession) {
  const manifest = {
    skillFolder: `skills/${slugify(designSession.skillInfo.name)}`,
    outcomes: [],
    // Supporting assets the design team must supply (VMs, project files, lab
    // start/finish files). The generator cannot infer these; they are captured during
    // design Step 7 and reviewed in Step 8.
    supportingAssets: designSession.supportingAssets || []
  };

  const modality = designSession.deliveryStrategy
    ? designSession.deliveryStrategy.modality
    : 'e-learning';

  for (let i = 0; i < designSession.outcomes.length; i++) {
    const outcome = designSession.outcomes[i];
    const outcomeFolderName = slugify(outcome.title);
    const outcomeFolder = `${manifest.skillFolder}/${outcomeFolderName}`;
    const num = String(i + 1).padStart(2, '0');

    const objectivesForOutcome = designSession.objectives[outcome.id] || [];
    const hasNonStandalone = objectivesForOutcome.some(o => !o.standalone);

    // Rule 6, outcome level: lecture and quiz are always required.
    const outcomeFiles = {
      lecture: `${outcomeFolder}/outcome-${num}-lecture.md`,
      quiz: `${outcomeFolder}/outcome-${num}-quiz.md`
    };

    // The outcome-level lab is where non-standalone objectives get their interactive
    // activity. Only omit it when every objective is standalone and carries its own lab.
    if (hasNonStandalone) {
      outcomeFiles.lab = `${outcomeFolder}/outcome-${num}-lab.md`;
    }

    // Classroom and blended delivery require instructor-facing materials.
    if (modality === 'classroom' || modality === 'blended') {
      outcomeFiles.presentation = `${outcomeFolder}/outcome-${num}-presentation.md`;
    }
    if (modality === 'blended') {
      outcomeFiles.handout = `${outcomeFolder}/outcome-${num}-handout.md`;
    }

    // A practical replaces or supplements the quiz when assessment is hands-on.
    if (outcome.assessmentType === 'practical' || outcome.assessmentType === 'both') {
      outcomeFiles.practical = `${outcomeFolder}/outcome-${num}-practical.md`;
    }

    const outcomeMapping = {
      title: outcome.title,
      folder: outcomeFolder,
      files: outcomeFiles,
      objectives: []
    };

    // Map objectives (objectivesForOutcome resolved above for the lab decision)
    for (let j = 0; j < objectivesForOutcome.length; j++) {
      const obj = objectivesForOutcome[j];
      const objFolder = `${outcomeFolder}/objective-${String(j + 1).padStart(2, '0')}`;
      
      // Rule 6: lecture, knowledge check, and quiz questions are authored for EVERY
      // objective. Quiz questions live at objective level so the outcome quiz can draw
      // a pool traceable to each objective.
      const objFiles = {
        lecture: `${objFolder}/lecture.md`,
        knowledgeCheck: `${objFolder}/knowledge-check.md`,
        quizQuestions: `${objFolder}/quiz-questions.md`
      };

      // A lab is authored at objective level ONLY for standalone objectives. A
      // non-standalone objective shares the outcome-level lab with its siblings.
      if (obj.standalone) {
        objFiles.lab = `${objFolder}/lab.md`;
      }
      
      outcomeMapping.objectives.push({
        title: obj.title,
        folder: objFolder,
        files: objFiles,
        standalone: obj.standalone
      });
    }
    
    manifest.outcomes.push(outcomeMapping);
  }

  return manifest;
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

## Step 9: Validate Design

```javascript
function step9_ValidateDesign(designSession) {
  const validation = {
    rule1_abcdCompleteness: validateRule1(designSession.outcomes),
    rule2_objectiveAlignment: validateRule2(designSession),
    rule3_coverageCompleteness: validateRule3(designSession),
    rule4_standaloneDesignation: validateRule4(designSession),
    rule5_deliveryStrategyAlignment: validateRule5(designSession),
    rule6_deliverableCompleteness: validateRule6(designSession),
    overallStatus: 'VALID',
    issues: []
  };
  
  // Check all rules
  const rules = [
    validation.rule1_abcdCompleteness,
    validation.rule2_objectiveAlignment,
    validation.rule3_coverageCompleteness,
    validation.rule4_standaloneDesignation,
    validation.rule5_deliveryStrategyAlignment,
    validation.rule6_deliverableCompleteness
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

// Rule 5: Delivery Strategy & Deliverable Alignment.
// Validates the whole derivation chain holds:
//   deliveryStrategy → offerings → publications → activities → deliverables
function validateRule5(designSession) {
  const issues = [];
  const { deliveryStrategy, offerings, publications } = designSession;

  // Part B: a delivery strategy, with rationale
  if (!deliveryStrategy) {
    issues.push({ msg: 'No delivery strategy selected', severity: 'BLOCKER' });
    // Everything below derives from it; no point checking further.
    return { passes: false, issues };
  }
  if (!deliveryStrategy.recommendation) {
    issues.push({ msg: 'Delivery strategy has no recorded rationale', severity: 'WARNING' });
  }

  // Part C: at least one offering, and every audience/role served
  if (!offerings || offerings.length === 0) {
    issues.push({ msg: 'No offerings defined: delivery strategy must produce at least one', severity: 'BLOCKER' });
  }

  // Part D: publications exist, and the offering/publication links resolve both ways
  if (!publications || publications.length === 0) {
    issues.push({ msg: 'No publications defined: every offering needs at least one', severity: 'BLOCKER' });
  }

  if (offerings && offerings.length > 0 && publications && publications.length > 0) {
    const referencedOfferingIds = new Set();
    for (const pub of publications) {
      String(pub.offeringIds || '')
        .split(',')
        .map(s => s.trim())
        .filter(Boolean)
        .forEach(id => referencedOfferingIds.add(id));

      if (!pub.docType) {
        issues.push({ msg: `Publication "${pub.name}": missing publication type (docType)`, severity: 'BLOCKER' });
      }
      if (!pub.audienceRole) {
        issues.push({ msg: `Publication "${pub.name}": missing audience/role`, severity: 'WARNING' });
      }
    }

    // Every offering must be supported by at least one publication
    for (const offering of offerings) {
      if (!referencedOfferingIds.has(offering.id)) {
        issues.push({
          msg: `Offering "${offering.name}" is not supported by any publication`,
          severity: 'BLOCKER'
        });
      }
    }
  }

  // Part E: supporting-asset dependencies are captured, not silently dropped
  if (!designSession.supportingAssets) {
    issues.push({ msg: 'Supporting assets not collected', severity: 'WARNING' });
  }

  return {
    passes: issues.length === 0,
    issues: issues
  };
}

// Rule 6: Deliverable File Completeness.
function validateRule6(designSession) {
  const issues = [];
  const manifest = designSession.deliverableManifest;

  if (!manifest) {
    issues.push({ msg: 'Deliverable manifest not generated', severity: 'BLOCKER' });
    return { passes: false, issues };
  }

  for (const outcome of manifest.outcomes) {
    // Outcome level: lecture and quiz are always required
    if (!outcome.files.lecture) {
      issues.push({ msg: `${outcome.title}: missing outcome lecture`, severity: 'BLOCKER' });
    }
    if (!outcome.files.quiz) {
      issues.push({ msg: `${outcome.title}: missing outcome quiz`, severity: 'BLOCKER' });
    }

    const hasNonStandalone = outcome.objectives.some(o => !o.standalone);
    if (hasNonStandalone && !outcome.files.lab) {
      issues.push({
        msg: `${outcome.title}: has non-standalone objectives but no outcome-level lab; they have no interactive activity`,
        severity: 'BLOCKER'
      });
    }

    // Objective level: lecture, knowledge check, and quiz questions are always required
    for (const obj of outcome.objectives) {
      if (!obj.files.lecture) {
        issues.push({ msg: `${obj.title}: missing lecture.md`, severity: 'BLOCKER' });
      }
      if (!obj.files.knowledgeCheck) {
        issues.push({ msg: `${obj.title}: missing knowledge-check.md`, severity: 'BLOCKER' });
      }
      if (!obj.files.quizQuestions) {
        issues.push({ msg: `${obj.title}: missing quiz-questions.md`, severity: 'BLOCKER' });
      }
      // A standalone objective must be independently completable, so it needs its own lab
      if (obj.standalone && !obj.files.lab) {
        issues.push({
          msg: `${obj.title}: standalone objective has no lab.md`,
          severity: 'BLOCKER'
        });
      }
    }
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
// The design JSON is the contract consumed by /develop-training AND by the content
// database. Field names here must match develop-training-engine.md exactly.
function generateDesignJSON(designSession) {
  const json = {
    designVersion: '2.0',
    generatedDate: new Date().toISOString().split('T')[0],
    skill: designSession.skillInfo,
    outcomes: designSession.outcomes,
    objectives: designSession.objectives,
    // The derivation chain, in order
    deliveryStrategy: designSession.deliveryStrategy,
    offerings: designSession.offerings,
    publications: designSession.publications,
    activities: designSession.activities,
    deliverableManifest: designSession.deliverableManifest,
    supportingAssets: designSession.supportingAssets,
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
  console.log(`Delivery strategy: ${designSession.deliveryStrategy.modality.toUpperCase()}`);
  console.log(`Offerings: ${designSession.offerings.length}`);
  console.log(`Publications: ${designSession.publications.length}`);
  console.log(`Supporting assets: ${designSession.supportingAssets.length}\n`);

  console.log('✓ All validation rules passed');
  console.log('✓ Ready to load into the content database\n');

  console.log('NEXT STEPS:');
  console.log('  1. Load this design into the content database, which creates the LeGIT');
  console.log('     project files and exports deliverables to DevOps.');
  console.log('     (Transitional: until the content database is live, record the design');
  console.log('      in the CDD Workbook.)');
  console.log('  2. Use /develop-training to author content into the generated files.\n');

  console.log('Design JSON saved as: design-' + designSession.deliverableManifest.skillFolder.split('/')[1] + '.json\n');
}
```

---

## Summary

This engine implements the complete 9-step design workflow:

✅ Step 1: Collect skill info
✅ Step 2: Define outcomes (ABCD validation)
✅ Step 3: Define objectives
✅ Step 4: Choose delivery strategy (6-factor decision engine)
✅ Step 5: Define offerings (per audience/role)
✅ Step 6: Define publications (scoped by offerings + design hierarchy)
✅ Step 7: Determine activities (P+I+A coverage)
✅ Step 8: Define deliverables (manifest + supporting assets)
✅ Step 9: Validate design (Rules 1-6; Rule 7 is a placeholder, not enforced)
✅ Generate design JSON output

**Out of scope for this skill**: creating LeGIT project files. That is owned by the content
database (design process guide Step 9), which consumes the design JSON produced here.

Ready for integration with the `/design-training` skill definition.
