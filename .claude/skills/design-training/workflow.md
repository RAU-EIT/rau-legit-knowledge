# Design Training Workflow - Internal Implementation

This document describes the step-by-step workflow for guiding users through the design process.

## Workflow State Machine

```text
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
─── derivation chain starts here ────────────────────────
  ↓
[Choose Delivery Strategy] → DELIVERY_STRATEGY_CHOSEN
  ↓
[Define Offerings] → OFFERINGS_DEFINED
  ↓
[Define Publications] → PUBLICATIONS_DEFINED
  ↓
[Determine Activities] → ACTIVITIES_PLANNED
  ↓
[Define Deliverables] → DELIVERABLES_DEFINED
  ↓
─────────────────────────────────────────────────────────
  ↓
[Validate Design] → DESIGN_VALID
  ↓
[Generate Design JSON] → COMPLETE
  ↓
(handoff: the content database creates LeGIT project files
          and exports deliverables to DevOps)
```

Each state in the derivation chain is an input to the next. See
[docs/terminology-glossary.md](../../../docs/terminology-glossary.md#the-two-directions).

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
Once all objectives are defined, validate objective-to-outcome alignment, then proceed to Step 4 (Delivery Strategy).

---

## Step 4: Choose Delivery Strategy (Modality)

### Context

Show decision framework:

```
DELIVERY STRATEGY DECISION FRAMEWORK

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

DELIVERY STRATEGY: [e-learning | classroom | blended]
REASON: [explanation]
PUBLICATION TYPES: [print, revealjs, scorm1.2, presentation-video, etc.]

Does this match your vision?
☐ Yes, proceed
☐ No, let me reconsider
☐ I want a different modality (explain)
```

### Store

```json
{
  "deliveryStrategy": {
    "modality": "e-learning",
    "publicationTypes": ["scorm1.2"],
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
Once the delivery strategy is confirmed, proceed to Step 5 (Offerings).

---

## Step 5: Define Offerings

### Context

```text
OFFERINGS - What the Student Enrolls In

The delivery strategy is confirmed. Now define what students actually receive.

An offering is a student-facing, packaged learning product: something a student
enrolls in, purchases, or downloads.

TYPICAL OFFERINGS BY DELIVERY STRATEGY:
• E-Learning  → a self-paced online course enrolled in via the LMS
• Classroom   → an instructor-led course with a scheduled session and location
• Blended     → a self-paced online course PLUS a classroom/regional practicum

WHY THIS IS ITS OWN STEP:
The same skill design often serves different roles differently.

  Field Technician    → blended: e-learning course + regional hands-on practicum
  Application Engineer → e-learning only: same theory, no practicum, as reference

Both draw on the same outcomes. They differ in which publications they include,
which is exactly what Step 6 works out.
```

### User Input

```text
How many offerings does this skill need?
(At least one. Define one per audience/role that needs something different.)

For each offering:

OFFERING NAME (student-facing):
(e.g., "Hydraulic Troubleshooting for Field Technicians")

TARGET AUDIENCE/ROLE:
(e.g., "Field Technician")

OUTCOME COVERAGE:
Which outcomes does this offering cover? (all, or a subset)

SUPPORTING MATERIALS:
(certificates, job aids, reference guides; blank for none)
```

### Store

```json
{
  "offerings": [
    {
      "id": "offering-1",
      "name": "Hydraulic Troubleshooting for Field Technicians",
      "audienceRole": "Field Technician",
      "modality": "blended",
      "outcomeCoverage": ["outcome-1", "outcome-2"],
      "supportingMaterials": "Completion certificate, quick-reference job aid"
    }
  ]
}
```

### Next Action

Confirm every audience/role from intake is served by at least one offering, then proceed to
Step 6 (Publications).

---

## Step 6: Define Publications

### Context

```text
PUBLICATIONS - What the Build System Must Produce

A publication is decided HERE, during design, and produced later at build time.

Deciding it now is what makes the activity and deliverable sets correct. Deciding
it later means discovering later that you authored the wrong things.

REQUIRED PUBLICATIONS BY DELIVERY STRATEGY:
• E-Learning  → SCORM module (scorm1.2)
• Classroom   → Instructor presentation (revealjs/pptx)
                Lab manual (print)
                Practical assessment (print)
• Blended     → All of the above, plus a learner handout (print)

SCOPING BY DESIGN HIERARCHY:
• One publication per skill      → the default
• One publication per outcome    → when outcomes ship separately, or roles need
                                   different outcome subsets
• One per standalone objective   → REQUIRED; a standalone objective is by
                                   definition independently completable
```

### Flag Standalone Objectives

Before collecting input, list any standalone objectives, each needs its own publication:

```text
⚠️  2 standalone objective(s) each require their own publication:
   • Troubleshoot common component failures (in Analyze Hydraulic Components)
   • Interpret pressure anomalies (in Interpret Pressure Readings)
```

### User Input

```text
How many publications does this skill need?

For each publication:

PUBLICATION NAME:
(e.g., "Hydraulic Troubleshooting - Technician SCORM")

AUDIENCE/ROLE IT SERVES:
(e.g., "Field Technician")

SCOPE:
(e.g., "Outcomes 1-3", "Objective 2 standalone")

PUBLICATION TYPE (docType):
☐ scorm1.2            (SCORM e-learning module)
☐ print               (PDF: lab manual, handout, practical)
☐ revealjs            (HTML presentation)
☐ pptx                (PowerPoint)
☐ presentation-video  (MP4 recording)

WHICH OFFERING(S) DOES IT FEED?
(comma-separated offering ids)
```

### Store

```json
{
  "publications": [
    {
      "id": "publication-1",
      "name": "Hydraulic Troubleshooting - Technician SCORM",
      "audienceRole": "Field Technician",
      "scope": "Outcomes 1-3",
      "docType": "scorm1.2",
      "offeringIds": "offering-1"
    }
  ]
}
```

### Next Action

Confirm every offering is supported by at least one publication and every publication feeds
at least one offering (Rule 5 Part D), then proceed to Step 7 (Activities).

---

## Step 7: Determine Activities

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
Once all outcome activities are planned and validated for coverage, proceed to Step 8 (Deliverables).

---

## Step 8: Define Deliverables

### Context

Explain what a deliverable is and where it goes:

```text
DELIVERABLES - Everything the SME Must Produce

Deliverables = every Activity + supporting assets

Most are authored content. Some (VMs, project files) are not authored content at all,
LeGIT but are still required, tracked, and exported to DevOps.

STRUCTURE:
skills/[skill-name]/
├── [outcome-title]/                (outcome folder in kebab-case)
│   ├── objective-01/               (objective folder)
│   │   ├── lecture.md              (always required)
│   │   ├── knowledge-check.md      (always required)
│   │   ├── quiz-questions.md       (always required - feeds outcome quiz pool)
│   │   ├── lab.md                  (ONLY if the objective is standalone)
│   │   └── media/                  (objective-specific images)
│   ├── objective-02/
│   │   └── ...
│   ├── outcome-01-lecture.md       (aggregates objective lectures)
│   ├── outcome-01-lab.md           (interactive activity for non-standalone objectives)
│   ├── outcome-01-quiz.md          (draws from objective quiz-questions pools)
│   ├── outcome-01-presentation.md  (classroom or blended)
│   ├── outcome-01-handout.md       (blended)
│   └── media/                      (outcome-shared images)
├── assets/                         (supporting assets + required README.md)
└── media/                          (skill-wide shared images)

KEY POINTS:
• Outcome titles in kebab-case: "analyze-hydraulic-components"
• lecture.md, knowledge-check.md, and quiz-questions.md exist for EVERY objective
• lab.md at objective level ONLY for standalone objectives
• Non-standalone objectives share the outcome-level lab
• Presentations and practicals are OUTCOME level, not skill level
```

### Collect Supporting Assets

The generator cannot infer these, so ask explicitly:

```text
Does any activity depend on something LeGIT does not author?

  • VMs or test environments
  • Project files (.ACD, configuration exports)
  • Lab start & finish files
  • Externally produced media
  • Supporting documentation (setup guides, equipment lists)

For each: what it is, which deliverable depends on it, who owns it, where it lives.
```

### Generate the Deliverable Manifest

This produces **design data describing the files**: it does not create files. File creation
is owned by the content database (design process guide Step 9).

```javascript
function generateDeliverableManifest(skill, outcomes, deliveryStrategy, supportingAssets) {
  const modality = deliveryStrategy.modality;
  const manifest = {
    skillFolder: `skills/${skill.name.toLowerCase().replace(/ /g, '-')}`,
    outcomes: [],
    supportingAssets: supportingAssets || []
  };

  outcomes.forEach((outcome, idx) => {
    const outcomeFolderName = outcome.title.toLowerCase().replace(/ /g, '-');
    const outcomeFolder = `${manifest.skillFolder}/${outcomeFolderName}`;
    const num = String(idx + 1).padStart(2, '0');
    const hasNonStandalone = outcome.objectives.some(o => !o.standalone);

    // Rule 6, outcome level: lecture and quiz always required.
    const outcomeFiles = {
      lecture: `${outcomeFolder}/outcome-${num}-lecture.md`,
      quiz: `${outcomeFolder}/outcome-${num}-quiz.md`
    };

    // Non-standalone objectives get their interactive activity here.
    if (hasNonStandalone) {
      outcomeFiles.lab = `${outcomeFolder}/outcome-${num}-lab.md`;
    }
    if (modality === 'classroom' || modality === 'blended') {
      outcomeFiles.presentation = `${outcomeFolder}/outcome-${num}-presentation.md`;
    }
    if (modality === 'blended') {
      outcomeFiles.handout = `${outcomeFolder}/outcome-${num}-handout.md`;
    }
    if (outcome.assessmentType === 'practical' || outcome.assessmentType === 'both') {
      outcomeFiles.practical = `${outcomeFolder}/outcome-${num}-practical.md`;
    }

    manifest.outcomes.push({
      title: outcome.title,
      folder: outcomeFolder,
      files: outcomeFiles,
      objectives: outcome.objectives.map((obj, objIdx) => {
        const objFolder = `${outcomeFolder}/objective-${String(objIdx + 1).padStart(2, '0')}`;

        // Rule 6: lecture, knowledge check, and quiz questions for EVERY objective.
        // Quiz questions live here so the outcome quiz can draw a pool traceable
        // to each objective.
        const objFiles = {
          lecture: `${objFolder}/lecture.md`,
          knowledgeCheck: `${objFolder}/knowledge-check.md`,
          quizQuestions: `${objFolder}/quiz-questions.md`
        };

        // A lab is authored at objective level ONLY for standalone objectives.
        if (obj.standalone) {
          objFiles.lab = `${objFolder}/lab.md`;
        }

        return {
          title: obj.title,
          folder: objFolder,
          files: objFiles,
          standalone: obj.standalone
        };
      })
    });
  });

  return manifest;
}
```

### User Input

Display the generated deliverable manifest (example: blended delivery, objective-02 standalone):

```text
DELIVERABLES FOR: [skill name]

skills/analyze-hydraulic-components/
├── analyze-hydraulic-components/
│   ├── objective-01/               (non-standalone)
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── quiz-questions.md
│   │   └── media/
│   ├── objective-02/               (STANDALONE - gets its own lab)
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── quiz-questions.md
│   │   ├── lab.md
│   │   └── media/
│   ├── outcome-01-lecture.md
│   ├── outcome-01-lab.md           (for objective-01)
│   ├── outcome-01-quiz.md
│   ├── outcome-01-presentation.md  (blended)
│   ├── outcome-01-handout.md       (blended)
│   └── media/
├── assets/
│   ├── vm/                         (hydraulic simulator VM)
│   └── README.md                   (inventory - required)
└── media/

NON-LEGIT DELIVERABLES:
• Hydraulic simulator VM (required by outcome-01-lab.md, owner: Lab Engineering)

Does this set of deliverables look right?
☐ Yes, looks good
☐ No, I need to add/change/remove something
```

### Store

```json
{
  "deliverableManifest": {
    "skillFolder": "skills/analyze-hydraulic-components",
    "outcomes": [...],
    "supportingAssets": [...]
  }
}
```

### Next Action

Once the deliverables are confirmed, proceed to Step 9 (Validation).

---

## Step 9: Validation & Summary

### Validation Checklist

Run the 6 enforced validation rules. (Rule 7, publication-to-activity derivation, is a
placeholder in `.claude/rules/content-design-validation.md` and is **not** enforced yet.)

```javascript
function validateDesign(designJSON) {
  return {
    rule1_abcdCompleteness: validateOutcomeCompleteness(designJSON.outcomes),
    rule2_objectiveAlignment: validateObjectiveAlignment(designJSON.outcomes),
    rule3_coverageCompleteness: validateCoverage(designJSON.outcomes),
    rule4_standaloneDesignation: validateStandalone(designJSON.outcomes),
    // Rule 5 validates the whole derivation chain, not just the strategy:
    // deliveryStrategy → offerings → publications → deliverables
    rule5_deliveryStrategyAlignment: validateDeliveryStrategyAlignment(designJSON),
    rule6_deliverableCompleteness: validateDeliverableManifest(designJSON.deliverableManifest)
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

✅ Rule 5: Delivery Strategy & Deliverable Alignment - PASS
   Blended strategy → 2 offerings → 4 publications → all activities → deliverables
   Every audience/role is served; every offering has a publication

✅ Rule 6: Deliverable File Completeness - PASS
   All files properly named and structured
   Every objective has lecture + knowledge-check + quiz-questions
   Outcome-level lab present for non-standalone objectives
   1 supporting asset recorded with owner

⊘ Rule 7: Publication-to-Activity Derivation - NOT ENFORCED
   Mapping rules not yet specified

OVERALL: ✅ DESIGN VALID - READY TO LOAD INTO CONTENT DATABASE
```

### Generate Design JSON

Create the output JSON:

The fields appear in **derivation order**: each one is an input to the next.

```json
{
  "designVersion": "2.0",
  "generatedDate": "2026-09-03",
  "skill": {
    "name": "Analyze Hydraulic System Components",
    "description": "Train field technicians to identify, diagnose, and repair hydraulic components",
    "audience": "Field service technicians with 1-2 years experience",
    "estimatedTime": "3 hours"
  },
  "outcomes": [...],
  "objectives": {...},
  "deliveryStrategy": {
    "modality": "blended",
    "publicationTypes": ["scorm1.2", "revealjs", "print"],
    "factors": {...},
    "recommendation": "Blended: distributed audience favors e-learning, but hands-on practice requires field equipment"
  },
  "offerings": [
    {
      "id": "offering-1",
      "name": "Hydraulic Troubleshooting for Field Technicians",
      "audienceRole": "Field Technician",
      "modality": "blended",
      "outcomeCoverage": ["outcome-1", "outcome-2", "outcome-3"],
      "supportingMaterials": "Completion certificate, quick-reference job aid"
    }
  ],
  "publications": [
    {
      "id": "publication-1",
      "name": "Hydraulic Troubleshooting - Technician SCORM",
      "audienceRole": "Field Technician",
      "scope": "Outcomes 1-3",
      "docType": "scorm1.2",
      "offeringIds": "offering-1"
    }
  ],
  "activities": {...},
  "deliverableManifest": {
    "skillFolder": "skills/analyze-hydraulic-system-components",
    "outcomes": [...],
    "supportingAssets": [...]
  },
  "supportingAssets": [
    {
      "name": "Hydraulic simulator VM",
      "category": "VM / test environment",
      "requiredBy": "outcome-01-lab.md",
      "owner": "Lab Engineering",
      "location": "SharePoint > Training Assets > hydraulic-sim v2.1"
    }
  ],
  "validation": {
    "rule1_abcdCompleteness": "PASS",
    "rule2_objectiveAlignment": "PASS",
    "rule3_coverageCompleteness": "PASS",
    "rule4_standaloneDesignation": "PASS",
    "rule5_deliveryStrategyAlignment": "PASS",
    "rule6_deliverableCompleteness": "PASS",
    "overallStatus": "VALID"
  }
}
```

**Contract note**: these field names are consumed by `/develop-training` and by the content
database. Renaming any of them requires updating `develop-training-engine.md` in the same
change, or the handoff breaks.

### Next Steps

```text
YOUR DESIGN IS COMPLETE! ✅

Next: load this design into the content database, which creates the LeGIT project
files and exports the deliverables to DevOps.

(Transitional: until the content database is live, record the design in the CDD
Workbook and create the project structure from the deliverable manifest.)

Then: use the /develop-training skill to author content into those files.

The /develop-training skill will:
1. Read this design JSON
2. Locate the files the content database created
3. Help you write lectures, labs, and quizzes
4. Validate against LeGIT standards
5. Support multiple publication types

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
