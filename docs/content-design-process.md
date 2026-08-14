# Content Design Process Guide

This guide walks you through the content design phase—where learning intent is defined, independent of delivery method. Complete this phase BEFORE starting content development.

**Timeline**: Typically 1-2 weeks per skill set  
**Inputs**: Skill statements (already defined), target roles, stakeholder requirements  
**Outputs**: Learning outcomes, learning objectives, publication strategy, deliverables list, CDD Workbook entry  
**Validation**: Claude Code checks design completeness; escalates complex questions to Aba Azeem

## Step 1: Define Learning Outcomes Using ABCD

A **learning outcome** is an observable, measurable statement of what learners must accomplish to demonstrate skill attainment. Use the **ABCD method**:

### A: Audience
**Who is learning?**  
Specify the job role, experience level, and learner characteristics.

### B: Behavior
**What action demonstrates learning?** (Observable, measurable)  
Use action verbs from Bloom's Taxonomy (analyze, apply, construct, demonstrate, evaluate, etc.). Avoid vague verbs like "understand" or "know."

### C: Condition
**Under what circumstances will they perform this?**  
Specify the context, tools, documentation, or constraints present when the learner performs the behavior.

### D: Degree
**To what standard or level of proficiency?**  
Specify measurable success criteria.

### Example ABCD Outcome

"Given a schematic diagram of a hydraulic system, the field technician will analyze the components and explain the function of each **with 90% accuracy**."

- **Audience**: field technician
- **Behavior**: analyze components and explain function
- **Condition**: given a schematic diagram
- **Degree**: with 90% accuracy

## Step 2: Define Learning Objectives

Break each outcome into **2-5 step-level objectives**. Objectives are the building blocks learners work through to reach full outcome competence.

### Characteristics of Good Objectives
- Represent step-level learning, not full verification
- Are specific and observable behaviors
- Support outcomes but do not replace them
- Typically 2-5 objectives per outcome

### Standalone vs. Non-Standalone Objectives

**Standalone Objective**:
- Designed as an independent learning unit (often for e-learning modules)
- Must have its own complete coverage: passive (lecture) + interactive (lab/practice) + assessment (quiz or practical)
- Learner can complete it without requiring other objectives

**Non-Standalone Objective**:
- Authored separately but rolls into parent outcome for delivery
- Requires its own lecture content (will be included in outcome-level lecture)
- Shares interactive and assessment activities at the outcome level
- Works best when delivery method requires integrated outcomes

## Step 3: Determine Publication Strategy

**Publication strategy** answers: "What delivery modalities are needed for this skill?"

### Quick Decision Framework

**Question 1**: Can the learner become competent without practice?
- **YES** → E-Learning 
- **NO** → Go to Question 2

**Question 2**: Is instructor feedback, coaching, or lab facilitation required?
- **NO** → E-Learning
- **YES** → Go to Question 3

**Question 3**: Are learners distributed, refreshers needed, or pre-work useful?
- **YES** → **Blended** (e-learning + classroom labs + refreshers)
- **NO** → **Classroom** (instructor-led, hands-on labs)

## Step 4: Map to Required Deliverables

Once you know the publication strategy, the required deliverables are determined.

**E-Learning requires**: SCORM modules, embedded quizzes, knowledge checks, job aids, course descriptions

**Classroom requires**: Instructor presentations, student labs, practicals, assessment quizzes, handouts

**Blended requires**: All of the above

**Customer-facing adds**: TLB (Training Lab Book), TSL (Training Summary/Lesson Book)

## Step 5: Plan Activity Coverage

**Critical Rule**: Every learning outcome must have three types of activities:

1. **Passive Activity** (Lecture) — Learner gains exposure
   - Explanation, terminology, concepts, demonstrations, examples
   - Includes embedded knowledge checks

2. **Interactive Activity** (Lab/Practice) — Learner applies with guidance
   - Hands-on lab exercise, guided practice, scenario-based activity
   - With feedback or guidance (not assessment)

3. **Assessment Activity** (Quiz/Practical) — Learner demonstrates mastery
   - Quiz questions (graded, tied to outcome)
   - Practical exercise or project
   - Graded and scored

This coverage can be satisfied:
- ✓ Directly at the outcome level, OR
- ✓ Through aggregation of related objective-level activities

## Step 6: Enter Your Design into the CDD Workbook

The **CDD Workbook** (Excel) is the system of record for instructional alignment. Your design team completes:

- Skills tab
- Outcomes tab (ABCD)
- Objectives tab
- Activities tab
- Coverage validation tab
- Deliverables tab
- Modality/Publication tab

## Step 7: Validate Your Design

### Self-Check Checklist
- ☐ All outcomes follow ABCD method
- ☐ All objectives support outcomes
- ☐ Coverage matrix complete (passive + interactive + assessment per outcome)
- ☐ Standalone designations documented
- ☐ Publication strategy selected with rationale
- ☐ Deliverables list matches modality
- ☐ File mapping plan started
- ☐ CDD Workbook entry complete

### Claude Code Validation

Ask Claude Code: **"Validate my content design"**

Claude Code will check for ABCD completeness, objective alignment, coverage matrix, and modality-deliverable alignment.

### Escalation to Aba Azeem

If Claude Code finds complex design questions, it escalates to **Aba Azeem** (aba.azeem@rockwellautomation.com) for guidance.

For more details, see the complete guide in CLAUDE.md Section 0.
