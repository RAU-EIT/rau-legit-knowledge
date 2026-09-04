# Claude Code Skills Specification

## Overview

Two specialized Claude Code skills to automate RAU's content design and development workflows.

---

# SKILL 1: Design RAU Training Content

## Purpose
Guide SMEs through the content design process, capture design decisions, validate against rules, and output structured design JSON to the content database.

## When to Use
- Starting a new training project
- Need to design learning for multiple skills and audiences
- Unclear on modality, outcomes, or learning structure

## User Input
- **Skills to design**: List of skills needing training
- **Target audiences**: Roles and experience levels
- **Stakeholder requirements**: Any constraints or mandatory elements

## Workflow

### Command: `/design-training`

**Flow**:
```
1. Welcome + Context Gathering
   ├─ "What skills need training?"
   ├─ "Who are the target audiences?" (roles, experience levels)
   ├─ "Any constraints or requirements?"
   └─ "Do you have an existing design to update?"
        ├─ If YES → Load existing design.json, ask what changed
        └─ If NO → Start fresh design

2. Design Each Skill
   ├─ Skill name & description
   ├─ Define outcomes (ABCD)
   │  ├─ "What should learners accomplish?" (Behavior)
   │  ├─ "For which audience?" (Audience)
   │  ├─ "Under what circumstances?" (Condition)
   │  └─ "To what standard?" (Degree)
   │  └─ Suggest 2-3 outcomes automatically
   │
   ├─ Define objectives per outcome
   │  ├─ "Break this outcome into 2-5 objectives"
   │  ├─ Suggest progression from foundational to complex
   │  └─ Ask: "Standalone objectives?" (default: non-standalone)
   │
   ├─ Select modality for each audience
   │  ├─ Run decision tree:
   │  │  ├─ "Can learner become competent without practice?"
   │  │  ├─ "Is instructor feedback/coaching required?"
   │  │  └─ "Are learners distributed/need refreshers?"
   │  └─ Output: E-Learning | Classroom | Blended
   │
   └─ Map deliverables
      ├─ E-Learning → SCORM + lecture + knowledge-check + lab + quiz
      ├─ Classroom → Presentations + labs + quizzes + handouts
      └─ Blended → Both

3. Plan Activity Coverage
   ├─ For each outcome, ensure P+I+A coverage
   ├─ Check if coverage is aggregated or at objective level
   └─ Validate: Every outcome has all three activity types

4. Validation
   ├─ Check against 6 design rules
   │  ├─ ABCD completeness?
   │  ├─ Objective alignment?
   │  ├─ Coverage completeness?
   │  ├─ Standalone designation?
   │  ├─ Delivery strategy-deliverable alignment?
   │  └─ File mapping completeness?
   │
   ├─ If fails → Suggest fixes
   └─ If passes → Proceed

5. Output Design
   ├─ Generate design.json
   ├─ Show summary report
   └─ Offer to save to CDD database
       ├─ Create Git commit with design
       └─ Store JSON in designs/ folder or external database

6. Next Steps
   ├─ "Ready to author? Use /develop-training"
   └─ "Need to scaffold files? Use /scaffold-project"
```

## Output Format

### Design JSON Schema
```json
{
  "metadata": {
    "skillId": "SKL12345",
    "skillName": "Hydraulic System Troubleshooting",
    "version": "1.0",
    "createdDate": "2026-08-20",
    "createdBy": "user@rockwellautomation.com",
    "audiences": [
      {
        "role": "field technician",
        "experienceLevel": "intermediate",
        "count": 150
      }
    ],
    "designedBy": "Aba Azeem",
    "reviewed": false
  },
  
  "outcomes": [
    {
      "id": "OUT01",
      "title": "Diagnose Hydraulic System Failures",
      "abcd": {
        "audience": "Field technician with 2+ years experience",
        "behavior": "Diagnose root cause of system failures",
        "condition": "Using pressure gauges, flow meters, and troubleshooting guide",
        "degree": "With 95% accuracy across 8 failure scenarios",
        "complete": true
      },
      "objectives": [
        {
          "id": "OBJ01",
          "statement": "Interpret pressure gauge readings in diagnostic scenarios",
          "standalone": false,
          "coverage": {
            "passive": "lecture",
            "interactive": "lab",
            "assessment": "quiz"
          }
        },
        {
          "id": "OBJ02",
          "statement": "Identify root causes using diagnostic procedures",
          "standalone": false,
          "coverage": {
            "passive": "lecture",
            "interactive": "lab",
            "assessment": "quiz"
          }
        }
      ]
    }
  ],
  
  "deliveryStrategy": {
    "modality": "blended",
    "publicationTypes": ["scorm1.2", "revealjs", "print"],
    "rationale": "Hands-on practice required, distributed workforce"
  },

  "offerings": [
    {
      "id": "offering-1",
      "name": "Hydraulic Troubleshooting for Field Technicians",
      "audienceRole": "field technician",
      "modality": "blended",
      "outcomeCoverage": ["OUT01"],
      "supportingMaterials": "Completion certificate, quick-reference job aid"
    }
  ],

  "publications": [
    {
      "id": "publication-1",
      "name": "Hydraulic Troubleshooting - E-Learning Module",
      "audienceRole": "field technician",
      "scope": "OUT01",
      "docType": "scorm1.2",
      "offeringIds": "offering-1"
    },
    {
      "id": "publication-2",
      "name": "Hydraulic Troubleshooting - Instructor Presentation",
      "audienceRole": "field technician",
      "scope": "OUT01",
      "docType": "revealjs",
      "offeringIds": "offering-1"
    },
    {
      "id": "publication-3",
      "name": "Hydraulic Troubleshooting - Lab Manual",
      "audienceRole": "field technician",
      "scope": "OUT01",
      "docType": "print",
      "offeringIds": "offering-1"
    }
  ],

  "activityCoverage": [
    {
      "outcomeId": "OUT01",
      "passive": {
        "source": "objective-level",
        "details": "Lectures in OBJ01, OBJ02"
      },
      "interactive": {
        "source": "outcome-level",
        "details": "Hands-on lab assessing multiple objectives"
      },
      "assessment": {
        "source": "outcome-level",
        "details": "Quiz covering all objectives"
      },
      "complete": true
    }
  ],
  
  "validation": {
    "abcdComplete": true,
    "objectiveAlignment": true,
    "outcomeLevelCoverageComplete": true,
    "standaloneCoverageComplete": true,
    "deliveryStrategyAligned": true,
    "deliverableCompleteness": true,
    "allValid": true,
    "issues": [],
    "checkedDate": "2026-09-03"
  },

  "deliverableManifest": {
    "skillFolder": "skills/hydraulic-troubleshooting",
    "outcomes": [
      {
        "id": "OUT01",
        "title": "diagnose-failures",
        "folder": "skills/hydraulic-troubleshooting/diagnose-failures",
        "files": [
          "objective-01/lecture.md",
          "objective-01/knowledge-check.md",
          "objective-01/quiz-questions.md",
          "objective-02/lecture.md",
          "objective-02/knowledge-check.md",
          "objective-02/quiz-questions.md",
          "outcome-01-lecture.md",
          "outcome-01-lab.md",
          "outcome-01-quiz.md",
          "outcome-01-presentation.md",
          "outcome-01-handout.md"
        ]
      }
    ]
  },

  "supportingAssets": [
    {
      "name": "Hydraulic simulator VM",
      "category": "VM / test environment",
      "requiredBy": "outcome-01-lab.md",
      "owner": "Lab Engineering",
      "location": "SharePoint > Training Assets > hydraulic-sim v2.1"
    }
  ]
}
```

**Note on the manifest above**: OBJ01 and OBJ02 are both `standalone: false`, so neither gets
an objective-level `lab.md`: they share `outcome-01-lab.md`. Both still get
`quiz-questions.md`, which feeds the `outcome-01-quiz.md` pool. `outcome-01-presentation.md`
and `outcome-01-handout.md` appear because the delivery strategy is blended. See
[Rule 6](../.claude/rules/content-design-validation.md#rule-6-deliverable-file-completeness).

### Summary Report
```
✅ DESIGN VALIDATION COMPLETE

Skill: Hydraulic System Troubleshooting

OUTCOMES: 2
├─ Diagnose Hydraulic System Failures (2 objectives)
└─ Recommend Preventive Maintenance (3 objectives)

AUDIENCES & MODALITIES:
├─ Field Technician → Blended (e-learning + classroom)
└─ Maintenance Manager → Classroom only

DELIVERABLES SUMMARY:
├─ SCORM Modules: 1 (main e-learning course)
├─ Presentations: 1 (classroom instructor deck)
├─ Lab Manuals: 1 (PDF)
├─ Quizzes: 2 (outcome-level)
└─ Estimated Content Volume: ~15,000 words

VALIDATION: ✅ PASS
All 6 design rules validated

Next Step: Use /scaffold-project to create folder structure
          Then use /develop-training to author content
```

## Integration Points

### Input
- Design questionnaire responses (structured)
- Existing design.json (if updating)
- Reference to knowledge base rules

### Output Destination
- **Primary**: Git repo `designs/` folder + JSON
- **Secondary**: External CDD database (if configured)
- **Notification**: Webhook to team (Slack optional)

### References Knowledge Base
- [`docs/content-design-process.md`](./docs/content-design-process.md) - Design steps
- [`.claude/rules/content-design-validation.md`](./.claude/rules/content-design-validation.md) - Validation rules

---

# SKILL 2: Develop RAU Training Content

## Purpose
Guide SMEs through the content development process, help translate their knowledge (PPTs, manuals, expertise) into formal LeGIT-formatted markdown, validate against standards, and prepare files for publishing.

## When to Use
- Have a completed design (outcomes, objectives, modalities)
- Have source material to translate (PPTs, documentation, subject matter expertise)
- Need to author content that fits LeGIT standards
- Want feedback on clarity, structure, content blocks

## User Input
- **Existing design.json** (from Design skill or manual)
- **Source materials**: PPTs, product docs, user guides, or text descriptions
- **Scope**: Which objectives/files to author
- **Preferences**: Difficulty level, example style, audience level

## Workflow

### Command: `/develop-training`

**Flow**:
```
1. Setup
   ├─ "Which skill are you developing?" → Load design.json
   ├─ "What's your source material?"
   │  ├─ PowerPoint deck → Extract text, structure, images
   │  ├─ Product documentation → Adapt from formal to instructional
   │  ├─ Manual or guide → Convert to learning format
   │  └─ Subject matter expertise → Create from scratch
   │
   ├─ "Which outcome/objective to author first?"
   └─ Show folder structure & file checklist

2. Author Lectures
   ├─ "What are the key concepts?" → Organize into 800-1200 word structure
   ├─ Show lecture template:
   │  ├─ Learning objective statement
   │  ├─ Key concepts & terminology
   │  ├─ Real-world examples
   │  ├─ Demonstrations or walkthroughs
   │  └─ Summary
   │
   ├─ Help translate/rewrite from source material
   ├─ Suggest where to include visuals
   ├─ Check for clarity and conciseness
   └─ Validate against markdown standards

3. Create Knowledge Checks
   ├─ "1-2 simple questions to check understanding"
   ├─ Suggest question types:
   │  ├─ Fill in the blank
   │  ├─ True/false (with explanation)
   │  └─ Quick scenario
   │
   ├─ Keep brief (~50-100 words total)
   └─ Provide answer explanations

4. Create Labs (if needed)
   ├─ "Hands-on practice: what will learners DO?"
   ├─ Show lab template:
   │  ├─ Objective being practiced
   │  ├─ Prerequisites
   │  ├─ Step-by-step with screenshots
   │  ├─ Learning checkpoints
   │  ├─ Troubleshooting tips
   │  └─ Reflection questions
   │
   ├─ Help structure procedures
   ├─ Suggest where screenshots needed
   └─ Check for clarity & completeness

5. Create Quiz Questions
   ├─ "3-5 graded assessment questions"
   ├─ Suggest variety:
   │  ├─ Multiple choice
   │  ├─ Short answer
   │  └─ Scenario-based
   │
   ├─ Ask for correct answers + rationale
   ├─ Difficulty level per question
   └─ Validate question quality

6. Add LeGIT Elements
   ├─ Content Blocks
   │  ├─ "Where could this be an accordion?"
   │  ├─ "Would a 2-column layout work?"
   │  ├─ "Should this be a tip/warning/reference?"
   │  └─ Suggest blocks with examples
   │
   ├─ Visuals
   │  ├─ "Need an image here?"
   │  ├─ "SVG preferred over PNG"
   │  └─ Check alt text
   │
   └─ Formatting
      ├─ Check heading hierarchy
      ├─ Validate relative links
      └─ Ensure semantic markdown

7. Validate Standards
   ├─ Markdown linting
   │  ├─ Heading hierarchy
   │  ├─ List formatting
   │  ├─ Link structure
   │  └─ Special characters
   │
   ├─ YAML frontmatter
   │  ├─ Required fields present
   │  ├─ Correct docType
   │  ├─ Valid CSS reference
   │  └─ Skill metadata complete
   │
   ├─ Content block syntax
   │  ├─ Proper fenced divs
   │  ├─ Closing markers correct
   │  └─ Attributes valid
   │
   └─ LeGIT standards
      ├─ Semantic structure
      ├─ DRY principles
      └─ Accessibility

8. Preview & Iterate
   ├─ Show how content will look in outputs:
   │  ├─ SCORM preview
   │  ├─ PDF preview
   │  └─ Presentation preview
   │
   ├─ Ask for rewrites if needed
   └─ Check clarity with target audience in mind

9. Finalize & Save
   ├─ Files saved to correct locations
   ├─ Git commit with content
   ├─ Summary of what was authored
   └─ Checklist of next files to author
```

## Guided Authoring Examples

### Example 1: Translating from PPT

**PPT Slide**:
```
Slide 4: Pressure Gauge Reading
- Gauges show system pressure
- Normal operating range: 2000-2500 PSI
- Warning if over 3000 PSI or under 1500 PSI
- Always check gauge before starting system
```

**Skill Output**:
```markdown
---
title: "Objective 01: Interpret Pressure Gauge Readings"
docType: scorm
css: ../style-rau-base/rau-scorm.css
skill:
  id: SKL12345
  revisionDate: 2026-08
  classification: Public
---

# Objective 01: Interpret Pressure Gauge Readings

## Learning Objective
You will interpret pressure gauge readings to identify normal operating conditions 
and potential system issues.

## Pressure Gauge Basics

A **pressure gauge** measures the force exerted by system fluid. On a hydraulic 
system, the gauge displays readings in PSI (pounds per square inch).

### Reading the Gauge

The gauge has a scale with numbered markings. As pressure increases, the needle 
moves clockwise around the dial. Key readings to know:

- **Normal Operating Range**: 2000-2500 PSI
  - System is working correctly
  - Safe to operate under normal load
  
- **High Pressure Warning**: Over 3000 PSI
  - Possible system overload or blockage
  - Reduce load or stop operation
  - May trigger relief valve
  
- **Low Pressure Warning**: Below 1500 PSI
  - Possible leak or pump failure
  - System may not function properly
  - Requires investigation before operation

### Before Starting

**Always check the pressure gauge before starting the system:**

1. Look at the gauge needle position
2. Verify it's in the green (normal) zone
3. If outside normal range, investigate before starting
4. If abnormal, contact maintenance

::: {.rau-alert .tip}
Pro tip: Most technicians glance at the gauge when walking past the system. 
Make it a habit to check it every time you approach.
:::

## Real-World Scenario

You're starting your shift at the pumping station. You notice the pressure gauge 
reading is 1200 PSI, which is below the normal 2000 PSI minimum. 

**What you know**:
- Low pressure indicates a potential problem
- The system shouldn't operate under these conditions
- Something may be leaking or the pump may be failing

**What you do**:
- Do NOT start the system
- Notify maintenance immediately
- Document the low reading in the maintenance log

## Summary

Pressure gauge readings tell you whether your hydraulic system is operating 
normally. You should always check the gauge before operation and respond 
appropriately if readings are outside the normal 2000-2500 PSI range.
```

**Skill's Work**:
- ✅ Extracted key points from PPT
- ✅ Organized into learning structure
- ✅ Added examples and context
- ✅ Included real-world scenario
- ✅ Used LeGIT alert block
- ✅ ~800 words (within guidelines)

---

### Example 2: Translating from Documentation

**Source**: Product Installation Manual (technical, formal)

**Original**:
```
3.2 Pressure Relief Valve Installation
The pressure relief valve shall be installed in the main pressure line 
downstream of the pump. The valve body must be oriented with the pilot 
port facing upward. Torque specifications: 45 ft-lbs ± 2 ft-lbs.
```

**Skill Output**:
```markdown
## Where to Install the Pressure Relief Valve

The **pressure relief valve** is the safety device that prevents pressure 
from exceeding safe limits. It must be installed in exactly the right location.

### Correct Location: Main Pressure Line, Downstream of Pump

Think of it like a circuit:

Pump → **Pressure Relief Valve** → Rest of System

The valve sits right after the pump, in the main pressure line. This allows 
it to monitor all system pressure and act immediately if pressure gets too high.

**Why this location?**
- Catches excess pressure immediately after the pump
- Protects the entire downstream system
- Allows the valve to function as a safety device

### Orientation: Pilot Port Facing Up

The valve has a small **pilot port** on the top. This port must face upward.

::: {.rau-alert .attention}
If the pilot port faces sideways or downward, the valve won't function correctly 
and pressure can exceed safe limits.
:::

### Torque Specification: 45 ft-lbs ± 2 ft-lbs

Use a torque wrench set to 45 foot-pounds. The ± 2 means you can go from 
43-47 foot-pounds and still be correct.

**Don't over-tighten or under-tighten:**
- Too tight: Can damage threads and seal
- Too loose: Valve may leak or fail under pressure
```

**Skill's Work**:
- ✅ Converted technical → instructional
- ✅ Added explanation of WHY (not just WHAT)
- ✅ Used analogy (circuit diagram)
- ✅ Emphasized critical safety element
- ✅ Made procedural information clear
- ✅ Conversational tone for technicians

---

## Validation Checklist

**Before Finishing Each File**:

- [ ] Markdown standards met (heading hierarchy, formatting, links)
- [ ] YAML frontmatter complete and valid
- [ ] Content blocks used appropriately
- [ ] Images have alt text and relative paths
- [ ] ~800-1200 words (lectures), ~50-100 (knowledge checks)
- [ ] Learning objective clear
- [ ] Examples provided
- [ ] Appropriate for audience level
- [ ] DRY principles applied (no unnecessary repetition)
- [ ] Semantic markdown (no inline HTML, proper emphasis)

## Output Format

### Authored File
Markdown file in correct location with:
- Valid YAML frontmatter
- Proper heading hierarchy
- Content blocks where applicable
- Images with alt text
- Validation ✅ passed

### Development Report
```
✅ DEVELOPMENT SUMMARY

Objective: 01 - Interpret Pressure Gauge Readings

FILES AUTHORED:
├─ objective-01/lecture.md (850 words) ✅
├─ objective-01/knowledge-check.md (75 words) ✅
└─ objective-01/quiz-questions.md (5 questions) ✅

VALIDATION:
✅ Markdown standards pass
✅ YAML frontmatter complete
✅ Content blocks valid (1 alert block)
✅ Images: 2 (with alt text)
✅ Word count within guidelines

NEXT FILES TO AUTHOR:
├─ objective-02/lecture.md
├─ objective-02/knowledge-check.md
└─ objective-02/quiz-questions.md

Estimated completion: 2-3 days at 1 objective/day
```

## Integration Points

### Input
- Design JSON (from Design skill)
- Source materials (uploaded or pasted)
- Target file path (from scaffolded structure)
- Reference to knowledge base standards

### Output
- Markdown files in correct locations
- Git commits per file
- Validation reports
- Authoring checklist

### References Knowledge Base
- [`docs/legit-markdown-standards.md`](./docs/legit-markdown-standards.md) - Writing standards
- [`docs/content-blocks-reference.md`](./docs/content-blocks-reference.md) - Block syntax
- [`docs/legit-yaml.md`](./docs/legit-yaml.md) - YAML reference
- [`.claude/rules/legit-markdown-standards.md`](./.claude/rules/legit-markdown-standards.md) - Validation

---

# SKILL 3: Sync Docs to Rules (Automatic Validation)

## Purpose
Automatically validate that `.claude/rules/` files are in sync with updated documentation files. When docs change, Claude analyzes the differences and recommends updates to rules, then applies approved changes.

## When to Use
- Triggered automatically when docs/ files are committed
- Run manually with `/sync-docs-to-rules` to audit current state
- Used to maintain consistency between documentation and validation rules

## Workflow

### Automatic (On Doc Commit)

```
Git hook detects docs/ changes
  ↓
Claude: /sync-docs-to-rules (automatic)
  ↓
Analysis Phase:
  ├─ Read changed docs/ files
  ├─ Read corresponding .claude/rules/ files
  └─ Identify differences
  ↓
Recommendation Phase:
  ├─ Generate side-by-side comparison
  ├─ Explain why sync matters
  └─ Suggest specific updates to rules
  ↓
Team Review:
  ├─ Review Claude's analysis
  ├─ Approve or modify recommendations
  └─ Provide feedback
  ↓
Update Phase (Team Action):
  ├─ Apply approved changes to .claude/rules/
  ├─ Add "Last synced: YYYY-MM-DD" stamps
  └─ Commit changes
```

### Manual (For Auditing)

```
User: /sync-docs-to-rules
  ↓
Claude: "Here's the current state of docs vs rules"
  ↓
Recommendations shown
  ↓
User decides whether to apply
```

## Claude's Approach: Advisory, Not Enforcement

### What Claude DOES:
- ✅ **Identify gaps** between documentation and rules
- ✅ **Provide side-by-side comparisons** showing what changed
- ✅ **Recommend specific updates** to rules files
- ✅ **Explain rationale** for why sync matters
- ✅ **Offer multiple approaches** when there are options
- ✅ **Highlight conflicts** if team judgment needed

### What Claude DOES NOT:
- ❌ Auto-update rules without team approval
- ❌ Enforce rules rigidly without context
- ❌ Make strategic decisions (team decides)
- ❌ Bypass team review process
- ❌ Force a single "correct" interpretation

### Example Output

```
DOCUMENTATION SYNC ANALYSIS

Changed files:
- docs/design/content-design-process.md

Comparison Results:

FILE: docs/design/content-design-process.md
SECTION: Step 3 - Delivery Strategy Decision Framework

CHANGE DETECTED:
Updated decision criteria from "Can learner become competent without practice?"
to structured evaluation of 6 factors.

IMPACT ON RULES:
.claude/rules/content-design-validation.md Rule 5 (Delivery Strategy & Deliverable Alignment)
is based on outdated decision framework.

RECOMMENDED CHANGES:

1. Update Rule 5 Decision Framework
   FROM: Binary yes/no questions
   TO: 6-factor evaluation model
   
   Details:
   - Performance Type & Complexity
   - Instructor Involvement
   - Audience Scale & Distribution
   - Speed & Accessibility
   - Hardware & Environment Requirements
   - Content Complexity & Audience Level

2. Update Example Scenarios in Rule 5
   Old examples reference old decision tree
   New examples should reference new 6-factor model

3. Add Sync Metadata
   Add: "Last synced: 2026-08-20" to both files

TEAM DECISION REQUIRED:
Should we apply these changes? Any modifications or concerns?

IMPACT:
- Claude Code skills will use new decision framework
- Consistency maintained between docs and rules
- SMEs see what Claude Code enforces
```

## Command Syntax

**Automatic (triggered on commit):**
```
git commit -m "Update delivery strategy decision framework"
# Git hook automatically runs:
# /sync-docs-to-rules --auto --changed-files [list]
```

**Manual audit:**
```
/sync-docs-to-rules
```

**Manual audit with specific focus:**
```
/sync-docs-to-rules --focus content-design-validation.md
```

**Apply approved changes:**
```
/sync-docs-to-rules --apply-changes
# (Only after team reviews recommendations)
```

## Integration with Other Skills

```
/design-training
  ↓ (uses rules)
.claude/rules/content-design-validation.md
  ↓ (kept in sync by)
/sync-docs-to-rules
  ↓ (validates against)
docs/design/content-design-process.md
```

When you update design documentation:
1. `/sync-docs-to-rules` detects changes
2. Shows what needs updating in rules
3. You approve
4. Rules update automatically
5. `/design-training` now uses latest guidance

## Approval Workflow

**Step 1: Commit Documentation**
```bash
git commit -m "Update decision framework in docs/"
# Git hook runs sync validation
```

**Step 2: Review Recommendations**
```
Claude shows side-by-side comparison:
  docs/ version ← → .claude/rules/ version
  Shows what needs updating
```

**Step 3: Approve Changes**
```
Team decides:
  ✅ Apply all recommendations
  ✏️ Apply with modifications
  ❌ Don't sync (with explanation)
  ❓ Need clarification from Claude
```

**Step 4: Changes Applied**
```
If approved:
  - .claude/rules/ files updated
  - Sync dates added
  - New commit created
  - Claude Code skills get latest rules
```

## Configuration

To enable automatic sync validation:

1. **Enable git hook:**
   ```bash
   cp .claude/hooks/pre-commit-docs-sync .git/hooks/pre-commit
   chmod +x .git/hooks/pre-commit
   ```

2. **Set Claude Code preference** (in settings.json):
   ```json
   {
     "documentation": {
       "autoSyncValidation": true,
       "syncOnCommit": true,
       "requireApprovalBeforeUpdate": true
     }
   }
   ```

3. **Configure notification method:**
   - Option A: Console output (default)
   - Option B: Create GitHub issue with recommendations
   - Option C: Slack notification to team
   - Option D: Email summary to maintainer

---

# Integration Strategy

## How the Skills Work Together

```
START
  │
  ├─→ /design-training
  │      └─→ Outputs: design.json
  │
  ├─→ /scaffold-project (future skill)
  │      └─→ Creates folder structure + templates
  │
  └─→ /develop-training
         └─→ Fills in the templates with content
         └─→ Outputs: Markdown files ready to build
```

## Data Flow

```
Input: Skills List + Audiences
  ↓
Design JSON (design rules validated)
  ↓
Folder Structure (auto-scaffolded)
  ↓
Source Materials (PPT, docs, SME expertise)
  ↓
Authored Markdown Files (LeGIT standards validated)
  ↓
Build System (Pandoc + Lua filters)
  ↓
Output: PDF, SCORM, HTML, Video
```

## CDD Database Integration

**Stores**:
- Design JSON documents (source of truth for learning intent)
- File mapping (which objectives map to which files)
- Version history (design evolution)
- Authoring status (which files are complete)

**Queries**:
- "Show all designs for field technician audience"
- "List incomplete authoring tasks"
- "Which skills have been designed but not authored?"
- "Show version history of skill X"

---

# Implementation Roadmap

## Phase 2a: Build Design Skill
**Duration**: 3-4 weeks
**Deliverable**: /design-training command fully functional
**Testing**: 3 test designs (simple, complex, blended)

## Phase 2b: Build Development Skill
**Duration**: 3-4 weeks
**Deliverable**: /develop-training command fully functional
**Testing**: Translate 3 existing PPTs → LeGIT format

## Phase 2c: Integrate Skills
**Duration**: 2 weeks
**Deliverable**: Skills work together seamlessly
**Testing**: End-to-end workflow (design → develop → build → publish)

---

**Document Version**: 1.0  
**Created**: 2026-08-20  
**Status**: Specification for development  
