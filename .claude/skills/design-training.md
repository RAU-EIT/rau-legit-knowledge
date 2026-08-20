---
name: design-training
description: Guide SMEs through structured learning content design using ABCD framework and LeGIT standards
aliases: [design, design-skill, create-design]
whenToUse: You're starting a new training project or need to design learning content
---

# Design RAU Training Content

Welcome to the RAU LeGIT training design skill. I'll guide you through a structured 7-step process to design learning content that's ready for development.

## Quick Start

You'll answer questions about:
1. **Your skill** — What's being trained?
2. **Learning outcomes** — What will learners demonstrate?
3. **Learning objectives** — How will they get there?
4. **Publication strategy** — How will content be delivered?
5. **Activity coverage** — What types of learning activities?
6. **File structure** — How will content be organized?
7. **Validation** — Does everything align?

By the end, you'll have:
- ✅ Validated learning design
- ✅ File mapping ready for development
- ✅ Design JSON for the /develop-training skill

## Let's Begin

To get started, tell me:

**1. What skill are you designing?**

Provide:
- Skill name (e.g., "Configure CompactLogix Controllers")
- Brief description (1-2 sentences)
- Target audience (e.g., "field technicians")
- Estimated completion time (e.g., "4 hours")

**Example:**
```
Skill: Troubleshoot Hydraulic System Leaks
Description: Train field technicians to identify, diagnose, and repair common hydraulic leaks
Audience: Field service technicians with 1-2 years experience
Time: 3 hours
```

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

### **Step 3: Choose Publication Strategy**
- Evaluate 6 decision criteria (performance type, instructor involvement, audience scale, etc.)
- Select modality: e-learning, classroom, or blended
- Choose output formats: PDF, HTML presentations, SCORM, video

### **Step 4: Map Required Deliverables**
- Based on modality, determine what needs to be built
- Identify content components (lectures, labs, quizzes)
- Understand output formats and their requirements

### **Step 5: Plan Activity Coverage**
- Passive activities (lectures, readings)
- Interactive activities (labs, hands-on exercises)
- Assessment activities (quizzes, practicals)
- Validate coverage at outcome level

### **Step 6: File Mapping**
- Understand skill folder structure
- Map outcomes to folders
- Map objectives to files
- Identify file dependencies

### **Step 7: Validation**
- Check design against validation rules
- Ensure completeness and alignment
- Resolve any gaps or conflicts

---

## Validation Rules I'll Check

As we go through the design, I'll validate:

1. ✅ **ABCD Completeness** — Each outcome has complete ABCD elements
2. ✅ **Objective-to-Outcome Alignment** — Each objective supports a documented outcome
3. ✅ **Coverage Completeness** — Each outcome has Passive + Interactive + Assessment
4. ✅ **Standalone Objective Designation** — Marked objectives have their own coverage
5. ✅ **Modality-Deliverable Alignment** — Chosen modality matches required outputs
6. ✅ **File Mapping Completeness** — All outcomes/objectives have corresponding files

---

## Examples to Reference

### Example 1: Simple Single-Outcome Skill

**Skill**: Analyze Hydraulic System Components  
**Outcome 1**: Analyze hydraulic components and explain their function

Objectives:
- Obj 1: Identify basic hydraulic components (pump, motor, valve)
- Obj 2: Explain function of each component
- Obj 3: Troubleshoot common component failures

Modality: E-learning (SCORM)  
Coverage: Lectures → Labs with schematics → Quiz

---

### Example 2: Complex Multi-Outcome Skill

**Skill**: Design Industrial Automation Systems  
**Outcome 1**: Design control logic for discrete manufacturing  
**Outcome 2**: Design motion control for assembly equipment

Each outcome has its own:
- Objectives (3-5 per outcome)
- Learning activities
- Assessment

Modality: Blended (e-learning + classroom workshop)

---

## Ready to Start?

To begin your design, reply with your skill information:

```
Skill: [Name]
Description: [1-2 sentences about what it is]
Audience: [Who is learning?]
Estimated Time: [How long to complete?]
```

Once you provide this, I'll walk you through the 7-step design process, validate your design against all 6 rules, and prepare you for content development with the `/develop-training` skill.

---

## Additional Resources

- **Full Design Guide**: See `docs/design/content-design-process.md` for detailed step-by-step guidance
- **Validation Rules**: See `.claude/rules/content-design-validation.md` for complete rule definitions
- **File Mapping**: See `docs/design/file-mapping-guide.md` for understanding file structure
- **Development Next**: Once your design is validated, use `/develop-training` to author content files

**Need clarification?** Ask anytime during the process. I'll explain the ABCD framework, modality options, or validation rules in more detail.
