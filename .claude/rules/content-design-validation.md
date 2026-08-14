---
name: content-design-validation
description: Validation rules for learning content design
type: soft-guidance
---

# Content Design Validation Rules

Claude Code checks learning designs against these rules.

## Rule 1: ABCD Outcome Completeness

Every learning outcome must have all four ABCD elements:
- **A (Audience)**: WHO is learning
- **B (Behavior)**: WHAT observable action demonstrates learning
- **C (Condition)**: WHEN/WHERE/HOW the behavior occurs
- **D (Degree)**: TO WHAT STANDARD

Example Valid: "Given a schematic diagram, the field technician will analyze components and explain function with 90% accuracy."

Example Invalid: "Understand hydraulic principles." (Missing A, B, C, D)

## Rule 2: Objective-to-Outcome Alignment

Every learning objective must support a documented learning outcome.

- Each objective traces back to its outcome
- Objectives represent step-level learning toward outcome
- No orphaned objectives (not tied to any outcome)

## Rule 3: Coverage Completeness

Every learning outcome must have three types of activities:

1. **Passive** (Lecture) - Learner gains exposure
2. **Interactive** (Lab) - Learner applies knowledge with guidance  
3. **Assessment** (Quiz/Practical) - Learner demonstrates mastery

Coverage can be satisfied:
- Directly at the outcome level, OR
- Through aggregation of objective-level activities

## Rule 4: Standalone Objective Designation

Objectives marked as "standalone" must have their own full coverage:
- **Standalone**: Independent learning unit (e-learning)
  - Must have Passive + Interactive + Assessment
  - Learner can complete independently
  
- **Non-Standalone**: Aggregates into parent outcome
  - Requires lecture content (included in outcome)
  - Shares interactive and assessment at outcome level

## Rule 5: Modality-Deliverable Alignment

Selected modality must produce required deliverables:
- **E-Learning** → SCORM modules, quizzes, knowledge checks
- **Classroom** → Instructor presentations, labs, practicals
- **Blended** → All of the above

## Rule 6: File Mapping Completeness

For each outcome/objective, corresponding markdown files must be planned:
- Every objective needs: `objective-##-lecture.md`
- Every outcome needs: `outcome-##-lecture.md`
- Standalone objectives need: `objective-##-lab.md` and `objective-##-quiz.md`
- All file names follow naming convention

## Validation Workflow

### Step 1: Self-Check
- ☐ All outcomes have ABCD elements
- ☐ All objectives tied to outcomes
- ☐ Coverage matrix complete (Passive + Interactive + Assessment per outcome)
- ☐ Standalone designations documented
- ☐ Publication modality selected with rationale
- ☐ Deliverables list complete
- ☐ File mapping complete and follows naming convention
- ☐ CDD Workbook entry complete

### Step 2: Claude Code Validation
Ask Claude Code: "Validate my content design"

Result:
- ✅ **Passes** - Ready for development
- ⚠️ **Warnings** - Functional but has minor issues
- ❌ **Blockers** - Must fix before proceeding

### Step 3: Escalation
Complex questions escalate to Aba Azeem (aba.azeem@rockwellautomation.com)

### Step 4: Approval & Development
Once approved, proceed to content development phase.

---

**Escalation Contact**: Aba Azeem (aba.azeem@rockwellautomation.com)  
**Response Time**: 1-2 business days
