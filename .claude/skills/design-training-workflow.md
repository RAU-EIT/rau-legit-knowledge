---
name: design-training-workflow
description: Internal workflow implementation for /design-training skill
internal: true
---

# Design Training Workflow - Internal Implementation

This document describes the step-by-step workflow for guiding users through the design process.

## Workflow State Machine

```
START
  ↓
[Collect Skill Info] → SKILL_DEFINED
  ↓
[Define Outcomes (ABCD)] → OUTCOMES_DEFINED
  ↓
[Validate Outcomes] → OUTCOMES_VALID
  ↓
[Define Objectives] → OBJECTIVES_DEFINED
  ↓
[Validate Objectives] → OBJECTIVES_VALID
  ↓
[Choose Modality] → MODALITY_CHOSEN
  ↓
[Plan Activities] → ACTIVITIES_PLANNED
  ↓
[Create File Mapping] → FILES_MAPPED
  ↓
[Validate Design] → DESIGN_VALID
  ↓
[Generate Design JSON] → COMPLETE
```

---

## Step 1: Collect Skill Information

### User Input

Ask the user:

```
1. What is the skill name?
   (e.g., "Troubleshoot Hydraulic System Leaks")

2. Brief description (1-2 sentences):
   (e.g., "Train field technicians to identify, diagnose, and repair common hydraulic leaks")

3. Target audience:
   (e.g., "Field service technicians with 1-2 years experience")

4. Estimated completion time:
   (e.g., "3 hours", "2 days", "40 hours")
```

### Store

```json
{
  "skillInfo": {
    "name": "string",
    "description": "string",
    "audience": "string",
    "estimatedTime": "string"
  }
}
```

### Next Action
Confirm the skill info, then proceed to Step 2.

---

## Step 2: Define Learning Outcomes (ABCD)

### Context

Show the ABCD framework:

```
ABCD Framework for Learning Outcomes

A (Audience): WHO is learning?
   → Field technicians, operators, supervisors, engineers

B (Behavior): WHAT observable action shows they learned?
   → Analyze, troubleshoot, configure, design, explain

C (Condition): UNDER WHAT CIRCUMSTANCES?
   → Given a schematic, provided with tools, in a lab environment

D (Degree): TO WHAT STANDARD?
   → 80% accuracy, without errors, within 30 minutes

Complete statement:
"Given [condition], the [audience] will [behavior] [object] with [degree]."

Example:
"Given a schematic diagram, the field technician will analyze the components 
and explain the function of each with 90% accuracy."
```

### User Input

Ask:

```
How many learning outcomes does your skill have?
→ Minimum: 1 outcome per skill
→ Maximum: Usually 3-5 (complex skills can have more)

For each outcome:

OUTCOME TITLE:
(Clear, concise title - will be used in build outputs)
E.g., "Analyze Hydraulic System Components"

OUTCOME STATEMENT (ABCD):
Use this template:
"Given [C], the [A] will [B] [object] with [D]."

Example:
"Given a hydraulic system schematic, the field technician will analyze 
the components and explain the function of each with 90% accuracy."

Required elements in your statement:
□ A (Audience) - Who is learning?
□ B (Behavior) - What observable action?
□ C (Condition) - When/where/how?
□ D (Degree) - To what standard?
```

### Validation

For each outcome provided:

```javascript
function validateOutcome(outcome) {
  const hasTitle = outcome.title && outcome.title.length > 0;
  const hasStatement = outcome.statement && outcome.statement.length > 0;
  
  const statement = outcome.statement.toLowerCase();
  const hasAudience = /the (field technician|operator|engineer|supervisor|trainee)/.test(statement) 
                      || /audience/i.test(statement);
  const hasBehavior = /(analyze|troubleshoot|configure|design|explain|demonstrate|identify)/.test(statement);
  const hasCondition = /given|provided|when|with|in/.test(statement);
  const hasDegree = /accuracy|errors|minutes|standard|proficiency|competency/.test(statement) 
                    || /\d+%/.test(statement);
  
  const isComplete = hasTitle && hasStatement && hasAudience && hasBehavior && hasCondition && hasDegree;
  
  return {
    isComplete,
    missing: {
      title: !hasTitle,
      audience: !hasAudience,
      behavior: !hasBehavior,
      condition: !hasCondition,
      degree: !hasDegree
    }
  };
}
```

If outcome is incomplete, ask user to add missing elements.

### Store

```json
{
  "outcomes": [
    {
      "id": "outcome-1",
      "title": "Analyze Hydraulic System Components",
      "statement": "Given a hydraulic system schematic, the field technician will analyze the components and explain the function of each with 90% accuracy.",
      "abcd": {
        "audience": "field technician",
        "behavior": "analyze and explain",
        "condition": "given a hydraulic system schematic",
        "degree": "90% accuracy"
      }
    }
  ]
}
```

### Next Action
Once all outcomes are defined and valid, proceed to Step 3.

---

## Step 3: Define Learning Objectives

### Context

Explain objectives:

```
Learning Objectives are step-level learning toward the outcome.

Each outcome needs 2-5 objectives that together support the outcome.

TYPES OF OBJECTIVES:
• Knowledge: Learner can identify, recall, define
• Application: Learner can use knowledge to do something
• Analysis: Learner can break down and compare

STANDALONE vs NON-STANDALONE:
• Standalone: Can be completed independently (e.g., mini-module)
             Must have its own assessment
• Non-Standalone: Part of larger outcome
                  Rolls into outcome-level assessment (DEFAULT)

EXAMPLE:

Outcome: "Analyze Hydraulic System Components"

Objectives:
1. Identify basic hydraulic components (pump, motor, valve, filter)
   Type: Knowledge
   Standalone: No

2. Explain the function of each component in a system
   Type: Application
   Standalone: No

3. Troubleshoot common component failures
   Type: Analysis
   Standalone: Yes (could be a standalone troubleshooting module)
```

### User Input

For each outcome, ask:

```
How many objectives support this outcome?
(Typically 2-5; non-standalone objectives are default)

For each objective:

OBJECTIVE TITLE:
(e.g., "Identify basic hydraulic components")

OBJECTIVE STATEMENT:
What will the learner be able to do?
(e.g., "Given a hydraulic diagram, identify pump, motor, valve, filter")

TYPE:
☐ Knowledge (recall, identify, define)
☐ Application (apply, use, solve)
☐ Analysis (analyze, troubleshoot, compare)

STANDALONE?:
☐ No (default - rolls into outcome assessment)
☐ Yes (independent learning unit with its own assessment)

CLARIFICATION:
Is this marked as standalone, explain why it's independent:
(e.g., "This is a mini-troubleshooting module that learners might need 
as a reference even after completing the main skill")
```

### Store

```json
{
  "outcomes": [
    {
      "id": "outcome-1",
      "title": "Analyze Hydraulic System Components",
      "objectives": [
        {
          "id": "obj-1",
          "title": "Identify basic hydraulic components",
          "statement": "Given a hydraulic diagram, the technician will identify pump, motor, valve, and filter.",
          "type": "knowledge",
          "standalone": false
        },
        {
          "id": "obj-2",
          "title": "Explain component functions",
          "statement": "The technician will explain how each component contributes to system operation.",
          "type": "application",
          "standalone": false
        }
      ]
    }
  ]
}
```

### Next Action
Once all objectives are defined, validate objective-to-outcome alignment, then proceed to Step 4 (Modality).

---

## Step 4: Choose Publication Strategy (Modality)

### Context

Show decision framework:

```
PUBLICATION STRATEGY DECISION FRAMEWORK

Six factors determine the best modality:

1. PERFORMANCE TYPE
   ☐ Recall/Knowledge (facts, concepts)
   ☐ Procedure (step-by-step, hands-on)
   ☐ Problem-Solving (analysis, troubleshooting)
   ☐ Complex (requires practice and feedback)

2. INSTRUCTOR INVOLVEMENT
   ☐ None (learner self-paced)
   ☐ Minimal (Q&A support only)
   ☐ Moderate (guides, answers questions)
   ☐ Required (must teach live)

3. AUDIENCE SCALE
   ☐ Small (<20 learners)
   ☐ Medium (20-100 learners)
   ☐ Large (100+ learners)

4. SPEED & ACCESSIBILITY
   ☐ Needs to be available immediately
   ☐ Can be built in 1-2 weeks
   ☐ Can wait for longer development

5. HARDWARE REQUIREMENTS
   ☐ Physical equipment/lab needed
   ☐ Can simulate with software
   ☐ No special equipment needed

6. CONTENT COMPLEXITY
   ☐ Simple (straightforward content)
   ☐ Moderate (some interactive elements)
   ☐ Complex (many practice scenarios, simulations)
```

### Recommendation Engine

Based on responses:

```javascript
function recommendModality(factors) {
  // Rule 1: If instructor required OR hardware required → classroom/blended
  if (factors.instructorRequired || factors.hardwareRequired) {
    return {
      modality: "blended",
      reason: "Instructor and/or hands-on equipment required",
      formats: ["presentation", "scorm", "lab-manual"]
    };
  }
  
  // Rule 2: If large scale + high complexity → e-learning SCORM
  if (factors.scale === "large" && factors.complexity === "high") {
    return {
      modality: "e-learning",
      reason: "Large audience, consistent delivery needed",
      formats: ["scorm1.2"]
    };
  }
  
  // Rule 3: If simple + small scale → classroom or e-learning
  if (factors.complexity === "simple") {
    if (factors.scale === "small") {
      return {
        modality: "classroom",
        formats: ["presentation", "handout"]
      };
    }
    return {
      modality: "e-learning",
      formats: ["scorm1.2", "pdf"]
    };
  }
  
  // Default: e-learning
  return {
    modality: "e-learning",
    formats: ["scorm1.2"]
  };
}
```

### User Input

Ask questions 1-6 above, then show recommendation.

```
Based on your responses, I recommend:

MODALITY: [e-learning | classroom | blended]
REASON: [explanation]
OUTPUT FORMATS: [PDF, RevealJS, SCORM, Video, etc.]

Does this match your vision?
☐ Yes, proceed
☐ No, let me reconsider
☐ I want a different modality (explain)
```

### Store

```json
{
  "publicationStrategy": {
    "modality": "e-learning",
    "outputFormats": ["scorm1.2"],
    "factors": {
      "performanceType": "procedure",
      "instructorInvolvement": "none",
      "audienceScale": "medium",
      "speedAccessibility": "immediate",
      "hardwareRequirements": false,
      "complexity": "moderate"
    }
  }
}
```

### Next Action
Once modality is confirmed, proceed to Step 5 (Activities).

---

## Step 5: Plan Activity Coverage

### Context

Explain three activity types:

```
ACTIVITY COVERAGE - Three Levels

Every learning outcome needs ALL THREE:

1. PASSIVE (Lecture/Reading)
   → Learner gains exposure to content
   → Examples: lectures, readings, videos, demonstrations
   → Outcome-level: One comprehensive lecture
   → Objective-level: Lectures supporting each objective

2. INTERACTIVE (Lab/Practice)
   → Learner applies knowledge with guidance
   → Examples: guided labs, practice exercises, simulations, scenarios
   → Outcome-level: Labs reinforcing multiple objectives
   → Objective-level: Labs for application-level objectives

3. ASSESSMENT (Quiz/Practical)
   → Learner demonstrates mastery
   → Examples: quizzes, practicals, projects, real-world tasks
   → Outcome-level: Comprehensive outcome quiz
   → Objective-level: Question pool from objectives

COVERAGE RULES:
✓ Non-standalone objectives: Coverage validated at OUTCOME level
  (Objective-level activities roll up to outcome)
✓ Standalone objectives: Coverage validated at OBJECTIVE level
  (Must have independent P+I+A)
```

### User Input

For each outcome:

```
OUTCOME: [outcome title]

Passive Activities (what will learners READ/WATCH):
- [Lecture title]
- [Reading]
- [Video]

Interactive Activities (what will learners DO/PRACTICE):
- [Lab name]
- [Exercise]
- [Simulation]

Assessment Activities (how will learners DEMONSTRATE learning):
- [Quiz type and scope]
- [Practical exercise]
- [Project]

Check:
□ Passive: At least one lecture covering the outcome
□ Interactive: At least one lab/practice activity
□ Assessment: At least one quiz or practical
```

### Validation

```javascript
function validateCoverage(outcome) {
  const hasPassive = outcome.passiveActivities && outcome.passiveActivities.length > 0;
  const hasInteractive = outcome.interactiveActivities && outcome.interactiveActivities.length > 0;
  const hasAssessment = outcome.assessmentActivities && outcome.assessmentActivities.length > 0;
  
  const isComplete = hasPassive && hasInteractive && hasAssessment;
  
  return {
    isComplete,
    missing: {
      passive: !hasPassive ? "Need at least one lecture" : null,
      interactive: !hasInteractive ? "Need at least one lab/practice" : null,
      assessment: !hasAssessment ? "Need at least one quiz/practical" : null
    }
  };
}
```

### Store

```json
{
  "outcomes": [
    {
      "id": "outcome-1",
      "title": "Analyze Hydraulic System Components",
      "activities": {
        "passive": [
          "Lecture: Hydraulic System Fundamentals",
          "Video: Component Overview (8 min)"
        ],
        "interactive": [
          "Lab 1: Identify components on actual system",
          "Lab 2: Schematic analysis exercise"
        ],
        "assessment": [
          "Quiz: Component identification (10 questions)",
          "Practical: Diagnose a system failure"
        ]
      }
    }
  ]
}
```

### Next Action
Once all outcome activities are planned and validated for coverage, proceed to Step 6 (File Mapping).

---

## Step 6: File Mapping

### Context

Explain file structure:

```
FILE MAPPING - Converting Design to File Structure

Design specifies WHAT learners do.
File mapping specifies WHERE content goes.

STRUCTURE:
skills/[skill-name]/
├── [outcome-title]/                (outcome folder in kebab-case)
│   ├── objective-01/               (objective folder)
│   │   ├── lecture.md              (objective lecture - always required)
│   │   ├── knowledge-check.md      (embedded checks - always required)
│   │   ├── lab.md                  (hands-on - if standalone objective)
│   │   ├── quiz-questions.md       (questions - if standalone)
│   │   └── media/                  (objective-specific images)
│   ├── objective-02/
│   │   └── ...
│   ├── outcome-01-lecture.md       (aggregates objective lectures)
│   ├── outcome-01-quiz.md          (aggregates quiz questions)
│   └── media/                      (outcome-shared images)
└── media/                          (skill-wide shared images)

KEY POINTS:
• Outcome titles in kebab-case: "analyze-hydraulic-components"
• Objective lectures always exist
• Outcome lectures aggregate objective lectures
• Labs only for standalone objectives
• Quizzes aggregate from objective questions
```

### Generate File Structure

Based on user's outcomes and objectives:

```javascript
function generateFileMapping(skill, outcomes) {
  const mapping = {
    skillFolder: `skills/${skill.name.toLowerCase().replace(/ /g, '-')}`,
    outcomes: []
  };
  
  outcomes.forEach((outcome, idx) => {
    const outcomeFolderName = outcome.title.toLowerCase().replace(/ /g, '-');
    const outcomeFolder = `${mapping.skillFolder}/${outcomeFolderName}`;
    
    mapping.outcomes.push({
      title: outcome.title,
      folder: outcomeFolder,
      files: {
        lecture: `${outcomeFolder}/outcome-${String(idx+1).padStart(2,'0')}-lecture.md`,
        quiz: `${outcomeFolder}/outcome-${String(idx+1).padStart(2,'0')}-quiz.md`
      },
      objectives: outcome.objectives.map((obj, objIdx) => {
        const objFolder = `${outcomeFolder}/objective-${String(objIdx+1).padStart(2,'0')}`;
        const objFiles = {
          lecture: `${objFolder}/lecture.md`,
          knowledgeCheck: `${objFolder}/knowledge-check.md`
        };
        
        if (obj.standalone) {
          objFiles.lab = `${objFolder}/lab.md`;
          objFiles.quizQuestions = `${objFolder}/quiz-questions.md`;
        }
        
        return {
          title: obj.title,
          folder: objFolder,
          files: objFiles
        };
      })
    });
  });
  
  return mapping;
}
```

### User Input

Display the generated file mapping:

```
FILE STRUCTURE FOR: [skill name]

skills/analyze-hydraulic-components/
├── analyze-hydraulic-components/
│   ├── objective-01/
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   └── media/
│   ├── objective-02/
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── lab.md
│   │   ├── quiz-questions.md
│   │   └── media/
│   ├── outcome-01-lecture.md
│   ├── outcome-01-quiz.md
│   └── media/
└── media/

Does this structure work for your content?
☐ Yes, looks good
☐ No, I need to adjust
```

### Store

```json
{
  "fileMapping": {
    "skillFolder": "skills/analyze-hydraulic-components",
    "outcomes": [...]
  }
}
```

### Next Action
Once file mapping is confirmed, proceed to Step 7 (Validation).

---

## Step 7: Validation & Summary

### Validation Checklist

Run all 6 validation rules:

```javascript
function validateDesign(designJSON) {
  return {
    rule1_abcdCompleteness: validateOutcomeCompleteness(designJSON.outcomes),
    rule2_objectiveAlignment: validateObjectiveAlignment(designJSON.outcomes),
    rule3_coverageCompleteness: validateCoverage(designJSON.outcomes),
    rule4_standalonDesignation: validateStandalone(designJSON.outcomes),
    rule5_modalityAlignment: validateModality(designJSON.publicationStrategy),
    rule6_fileMapping: validateFileMapping(designJSON.fileMapping)
  };
}
```

### Show Results

Display validation result (PASS/WARNING/BLOCKER):

```
DESIGN VALIDATION RESULTS

✅ Rule 1: ABCD Completeness - PASS
   All 2 outcomes have complete ABCD elements

✅ Rule 2: Objective-to-Outcome Alignment - PASS
   All 4 objectives are mapped to outcomes

✅ Rule 3: Coverage Completeness - PASS
   All outcomes have Passive + Interactive + Assessment

✅ Rule 4: Standalone Designation - PASS
   1 standalone objective is properly designated

✅ Rule 5: Modality-Deliverable Alignment - PASS
   E-learning modality matches output formats (SCORM 1.2)

✅ Rule 6: File Mapping Completeness - PASS
   All files properly named and structured

OVERALL: ✅ DESIGN VALID - READY FOR DEVELOPMENT
```

### Generate Design JSON

Create the output JSON:

```json
{
  "designVersion": "1.0",
  "generatedDate": "2026-08-20",
  "skill": {
    "name": "Analyze Hydraulic System Components",
    "description": "Train field technicians to identify, diagnose, and repair hydraulic components",
    "audience": "Field service technicians with 1-2 years experience",
    "estimatedTime": "3 hours"
  },
  "outcomes": [...],
  "publicationStrategy": {...},
  "fileMapping": {...},
  "validation": {
    "rule1_abcdCompleteness": "PASS",
    "rule2_objectiveAlignment": "PASS",
    "rule3_coverageCompleteness": "PASS",
    "rule4_standalonDesignation": "PASS",
    "rule5_modalityAlignment": "PASS",
    "rule6_fileMapping": "PASS",
    "overallStatus": "VALID"
  }
}
```

### Next Steps

```
YOUR DESIGN IS COMPLETE! ✅

Next: Use the /develop-training skill to author content files

The /develop-training skill will:
1. Read this design JSON
2. Create file scaffolding
3. Help you write lectures, labs, and quizzes
4. Validate against LeGIT standards
5. Support multiple output formats

Ready to develop? Run:
/develop-training [design-json]

Or save this design for later:
Design saved as: design-[skill-slug].json
```

---

## Error Handling

### User Provides Incomplete Input

```
⚠️ Missing information

I need a bit more detail on outcome 1:
- Missing: What is the specific DEGREE of performance?
  (E.g., "90% accuracy", "without errors", "within 30 minutes")

Please provide: 
"Given a schematic diagram, the field technician will analyze the 
components and explain the function of each with [DEGREE]."
```

### Validation Fails

```
❌ BLOCKER: Outcome 1 has only 2 objectives

Rule 3 requires every outcome to have:
• At least 1 passive activity (lecture)
• At least 1 interactive activity (lab)
• At least 1 assessment activity (quiz)

Outcome 1 is missing: Assessment activity

What quiz or practical assessment will learners complete?
```

### User Wants to Reconsider

```
No problem! We can adjust:

☐ Modify a specific outcome
☐ Add/remove objectives
☐ Change publication modality
☐ Adjust activity coverage
☐ Start over with a different skill

What would you like to change?
```

---

## Output Formats

The design can be exported as:

1. **JSON** (machine-readable, for /develop-training)
   ```
   design-[slug].json
   ```

2. **Markdown Summary** (human-readable, for documentation)
   ```
   design-[slug]-summary.md
   ```

3. **CDD Workbook Entry** (for organizational tracking)
   ```
   [Skill Name] - Design Completed
   - Outcomes: [count]
   - Objectives: [count]
   - Modality: [type]
   - Status: VALIDATED
   ```

---

## Summary

This workflow guides users through:
1. ✅ Skill definition
2. ✅ ABCD outcome creation and validation
3. ✅ Objective definition (with standalone marking)
4. ✅ Modality selection with decision framework
5. ✅ Activity coverage planning
6. ✅ File structure mapping
7. ✅ Complete design validation

Result: A validated design ready for /develop-training to create files.
