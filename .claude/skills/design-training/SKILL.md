---
name: design-training
description: Guide SMEs through structured learning content design using the ABCD framework and LeGIT standards. Use when starting a new training project, designing a skill, or when asked to design, create a design, or validate a content design.
---

# Design RAU Training Content

Welcome to the RAU LeGIT training design skill. I'll guide you through a structured 9-step process to design learning content that's ready for development.

## Read These First

Before running any step, read both supporting files in this directory. This document is the
overview; the implementation lives in the other two:

- [`workflow.md`](./workflow.md): the step-by-step state machine. It defines what to ask at each
  of the 9 steps, what to validate, what to store, and when to advance. Follow it literally.
- [`engine.md`](./engine.md): the delivery strategy recommendation logic, the validation
  routines, and the design JSON and deliverable manifest output formats.

Do not run the design process from this overview alone.

## How Design Works: Derivation, Top-Down

Design **derives downward**. Each decision determines the next:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
(how it's taught)     (what the    (what must     (what        (what the SME
                       student      be built)      learners     must produce)
                       enrolls in)                 must do)
```

The build system later runs this chain in reverse: deliverables build into publications,
which are packaged into offerings. Both directions describe the same chain. See
[The Two Directions](../../../docs/terminology-glossary.md#the-two-directions).

This is why we decide offerings and publications *during* design rather than after: they are
what tell us which activities and deliverables are actually required. Deciding them late means
discovering late that you authored the wrong things.

## Quick Start

You'll answer questions about:

1. **Your skill**: What's being trained?
2. **Learning outcomes**: What will learners demonstrate?
3. **Learning objectives**: How will they get there?
4. **Delivery strategy**: How will this be taught? (e-learning / classroom / blended)
5. **Offerings**: What will students enroll in, per audience/role?
6. **Publications**: What must the build system produce?
7. **Activities**: What must learners do?
8. **Deliverables**: What must SMEs produce, including supporting assets?
9. **Validation**: Does everything align?

By the end, you'll have:

- ✅ Validated learning design
- ✅ Deliverable manifest ready for the content database
- ✅ Design JSON consumed by the content database and the `/develop-training` skill

## Let's Begin

To get started, tell me:

**1. What skill are you designing?**

Provide:

- Skill name (e.g., "Configure CompactLogix Controllers")
- Brief description (1-2 sentences)
- Target audiences and roles (e.g., "field technicians, application engineers")
- Estimated completion time (e.g., "4 hours")

**Example:**

```text
Skill: Troubleshoot Hydraulic System Leaks
Description: Train field technicians to identify, diagnose, and repair common hydraulic leaks
Audiences/roles: Field service technicians with 1-2 years experience
Time: 3 hours
```

> **Capture every audience/role.** Offerings and publications are scoped *per role*, so an
> incomplete role list here produces an incomplete publication set at Steps 5-6.

---

## Step-by-Step Workflow

Once you provide your skill, I'll guide you through:

### **Step 1: Define Learning Outcomes (ABCD)**

- Learn the ABCD framework (Audience, Behavior, Condition, Degree)
- Write complete learning outcome statements
- Validate each outcome has all four elements

### **Step 2: Define Learning Objectives**

- Create 2-5 objectives per outcome
- Ensure objectives support the outcome
- Mark which objectives are "standalone" (complete on their own)
- Together, Steps 1-2 produce the **design hierarchy**, which scopes publications later

### **Step 3: Determine Delivery Strategy**

- Evaluate 6 decision criteria (performance type, instructor involvement, audience scale, speed, hardware, complexity)
- Select the strategy: e-learning, classroom (ILT), or blended
- Record the rationale. A strategy without one fails validation

### **Step 4: Define Offerings**

- Define what students actually enroll in, purchase, or download
- One or more per audience/role; the same skill often serves roles differently
- Record outcome coverage and any supporting materials (certificates, job aids)

### **Step 5: Define Publications**

- Determine what the build system must produce for each offering
- Tag each with audience/role, scope, and publication type (`docType`)
- Scope by the design hierarchy: per skill, per outcome, or per standalone objective

### **Step 6: Determine Activities**

- Passive activities (lectures, readings)
- Interactive activities (labs, hands-on exercises)
- Assessment activities (quizzes, practicals)
- Derived from the publications; validated for coverage at outcome level

### **Step 7: Define Deliverables**

- Deliverables = every activity **plus** supporting assets
- Content deliverables: `lecture.md`, `knowledge-check.md`, `quiz-questions.md`, labs, quizzes
- Supporting assets: VMs, project files, lab start/finish files, externally produced media
- Produces a **deliverable manifest**: design data describing the files, not the files themselves

### **Step 8: Review the Generated Set**

- Steps 5-7 are generated; this is the human gate
- Add, change, or remove anything the generator got wrong

### **Step 9: Validation**

- Check the design against the validation rules
- Resolve any gaps or conflicts

---

## What This Skill Does *Not* Do

**It does not create files.** The **content database** owns creating the LeGIT project files
and exporting deliverables to DevOps (design process guide Step 9). This skill produces the
design JSON and deliverable manifest that the database consumes.

*Transitional*: until the content database is live, record the design in the CDD Workbook and
create the project structure from the deliverable manifest.

---

## Step Numbering: This Skill vs. the Design Process Guide

This skill's interactive steps run **one ahead** of `docs/design/content-design-process.md`,
because it starts by collecting skill metadata that the guide treats as an input.

| This skill | Design process guide |
| --- | --- |
| Collect skill info | *(the guide's "Inputs" header block)* |
| 1. Outcomes | Step 1: Define Learning Outcomes |
| 2. Objectives | Step 2: Define Learning Objectives |
| 3. Delivery strategy | Step 3: Determine Delivery Strategy |
| 4. Offerings | Step 4: Define Offerings |
| 5. Publications | Step 5: Define Publications |
| 6. Activities | Step 6: Determine Activities |
| 7. Deliverables | Step 7: Define Deliverables |
| 8. Review | Step 8: Validate & Refine Design |
| 9. Validation | Step 8: Validate & Refine Design |
| *(not in this skill)* | Step 9: Load into Content Database |

The offset is intentional, not drift.

---

## Validation Rules I'll Check

As we go through the design, I'll validate:

1. ✅ **ABCD Completeness**: Each outcome has complete ABCD elements
2. ✅ **Objective-to-Outcome Alignment**: Each objective supports a documented outcome
3. ✅ **Coverage Completeness**: Each outcome has Passive + Interactive + Assessment
4. ✅ **Standalone Objective Designation**: Marked objectives have their own coverage
5. ✅ **Delivery Strategy & Deliverable Alignment**: The whole chain holds: strategy → offerings → publications → activities → deliverables
6. ✅ **Deliverable File Completeness**: Every objective and outcome has its required files
7. ⊘ **Publication-to-Activity Derivation**: placeholder, **not enforced yet** (mapping rules TBD)

---

## Examples to Reference

### Example 1: Simple Single-Outcome Skill

**Skill**: Analyze Hydraulic System Components
**Outcome 1**: Analyze hydraulic components and explain their function

Objectives:

- Obj 1: Identify basic hydraulic components (pump, motor, valve), non-standalone
- Obj 2: Explain function of each component (non-standalone)
- Obj 3: Troubleshoot common component failures (**standalone**)

Delivery strategy: E-learning
Offerings: Hydraulic Components for Field Technicians (self-paced LMS course)
Publications: Main SCORM module (Outcomes 1) + separate SCORM for the standalone Obj 3
Activities: Lectures → Lab with schematics → Quiz
Deliverables: per-objective lecture + knowledge-check + quiz-questions; `lab.md` for Obj 3 only; outcome-level lab and quiz

---

### Example 2: Complex Multi-Outcome, Multi-Role Skill

**Skill**: Design Industrial Automation Systems
**Outcome 1**: Design control logic for discrete manufacturing
**Outcome 2**: Design motion control for assembly equipment

Delivery strategy: Blended (e-learning + classroom workshop)

Offerings differ by role:

- **Controls Engineer** → blended: self-paced course + hands-on workshop (both outcomes)
- **Project Manager** → e-learning only: Outcome 1 as reference, no workshop

Publications: SCORM module (both roles), instructor deck + lab manual + handout (engineers only)

Supporting assets: Studio 5000 starter project files, motion simulator VM

---

## Ready to Start?

To begin your design, reply with your skill information:

```text
Skill: [Name]
Description: [1-2 sentences about what it is]
Audiences/roles: [Who is learning? List every role.]
Estimated Time: [How long to complete?]
```

Once you provide this, I'll walk you through the 9-step design process, validate your design
against the rules, and prepare it for the content database and the `/develop-training` skill.

---

## Additional Resources

- **Terminology**: See `docs/terminology-glossary.md`, the source of truth for what these terms mean
- **Full Design Guide**: See `docs/design/content-design-process.md` for detailed step-by-step guidance
- **Validation Rules**: See `.claude/rules/content-design-validation.md` for complete rule definitions
- **File Structure**: See `docs/design/file-mapping-guide.md` for the deliverable file contract
- **Development Next**: Once the design is loaded into the content database, use `/develop-training` to author content

**Need clarification?** Ask anytime during the process. I'll explain the ABCD framework, the
derivation chain, delivery strategy options, or validation rules in more detail.
