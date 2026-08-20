# Content Design Process Guide

This guide walks you through the content design phase—where learning intent is defined, independent of delivery method. Complete this phase BEFORE starting content development.

**Timeline**: Typically 1-2 weeks per skill set  
**Inputs**: Skill statements (already defined), target roles, stakeholder requirements  
**Outputs**: Learning outcomes, learning objectives, publication strategy, deliverables list, CDD Workbook entry  
**Validation**: Claude Code checks design completeness; escalates complex questions to Aba Azeem

## Step 1: Define Learning Outcomes Using ABCD

Every skill must have **at least one learning outcome**. A **learning outcome** is an observable, measurable statement of what learners must accomplish to demonstrate skill attainment.

### How Many Outcomes Per Skill?

- **Minimum**: At least 1 outcome per skill (every skill requires at least one measurable competency)
- **Multiple outcomes**: A complex skill may have 2+ outcomes representing different competency areas within the same skill
- **Guidance**: If you have 5+ outcomes, ensure they're all part of the same coherent skill domain, not separate skills
- **Example**: 
  - Simple skill: 1 outcome (e.g., "Interpret pressure gauge readings")
  - Complex skill: 3 outcomes (e.g., "Troubleshoot hydraulic systems" could have: diagnose failures, identify root causes, recommend repairs)

### ABCD Method for Outcomes

Use the **ABCD method** to write clear, measurable outcomes:

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

### Outcome Title

Every outcome needs a **clear, concise title** for use in common outputs (PDFs, presentations, SCORM modules).

**Good titles**:
- "Analyze Hydraulic System Components"
- "Interpret Pressure Gauge Readings"
- "Diagnose Pump Failures"

**Bad titles**:
- "Outcome 1" (too vague)
- "Hydraulic Troubleshooting" (too broad, should be a skill name)
- "Understand pressure relationships" (vague verb)

**Format**: `[Action Verb] [Object] [Context if needed]`

### Example ABCD Outcome

**Title**: "Analyze Hydraulic System Components"

**ABCD Statement**: "Given a schematic diagram of a hydraulic system, the field technician will analyze the components and explain the function of each **with 90% accuracy**."

- **Audience**: field technician
- **Behavior**: analyze components and explain function
- **Condition**: given a schematic diagram
- **Degree**: with 90% accuracy

## Step 2: Define Learning Objectives

Break each outcome into **2-5 step-level objectives**. Objectives are the building blocks learners work through to reach full outcome competence.

### Why 2-5 Objectives Per Outcome?

**Why at least 2 (not 1)**:
- An outcome with only 1 objective suggests the outcome itself is **too small or granular**
- The outcome should represent a meaningful, substantive skill—not just one simple task
- If you find yourself with 1 objective, consider whether that's really an outcome or just an objective itself
- Multiple objectives allow for logical progression and scaffolding of learning

**Why typically 5 or fewer (not 10+)**:
- **Cognitive load**: Too many objectives (7+) overwhelms learners and becomes hard to manage
- **Course coherence**: 10+ objectives in one outcome suggests the outcome is **too broad and should split into 2-3 outcomes**
- **Instructional design**: Each objective needs adequate lecture, practice, and assessment—more objectives = more content complexity
- **Practical delivery**: Outcome-level labs and quizzes must assess all objectives; too many makes assessment unwieldy

**Decision guide**:
- **If you have 1 objective**: Reconsider whether this is really an outcome, or if you need to expand the scope
- **If you have 2-5 objectives**: You're in the sweet spot for coherent, manageable outcomes
- **If you have 6-10 objectives**: Consider combining related objectives or splitting the outcome
- **If you have 10+ objectives**: Definitely split into multiple outcomes—the scope is too broad

### Characteristics of Good Objectives
- Represent step-level learning toward outcome competence
- Are specific and observable behaviors
- Support outcomes but do not replace them
- Logically progress from foundational to complex
- Collectively enable the learner to achieve the full outcome

### Standalone vs. Non-Standalone Objectives

**Non-Standalone Objective (DEFAULT - Recommended)**:
- Authored separately but rolls into parent outcome for delivery
- Each objective has its own **lecture** (included in outcome-level lecture)
- Shares **interactive activities and assessment** at the outcome level with other objectives
- Works for all delivery methods: e-learning, classroom, blended
- Simpler to manage: lectures are modular, labs/quizzes assess the whole outcome together

**Standalone Objective (EXCEPTION - Use When Needed)**:
- Designed as an independent learning unit (for specific e-learning micro-credentials)
- Must have its own complete coverage: passive (lecture) + interactive (lab/practice) + assessment (quiz or practical)
- Learner can complete it independently without other objectives
- Creates separate SCORM packages per objective
- Use only when objectives must be truly independent and self-contained

### Decision Guide

**Choose Non-Standalone (Default) when**:
- Building a skill with multiple related objectives (most cases)
- Creating classroom or blended delivery
- Objectives build on each other logically
- You want flexible assessment across related objectives

**Choose Standalone only when**:
- The objective must be completable independently
- Learners might take this objective in isolation from others
- You specifically need separate SCORM packages per objective
- Examples: micro-credentials, prerequisite modules, remedial learning units

## Step 3: Determine Publication Strategy (RAU Recommendation)

**Last synced**: 2026-08-20  
**Rule reference**: [.claude/rules/content-design-validation.md Rule 5](/.claude/rules/content-design-validation.md#rule-5-modality-deliverable-alignment--selection)

**Publication strategy** answers: "What delivery modality best serves this skill?"

**Important**: RAU training professionals make the **modality recommendation** based on instructional design principles, not on stakeholder requests alone. While we gather input during intake, the final recommendation comes after the design phase when we have full clarity on learning requirements.

### Decision Framework (Not a Rigid Rule)

This framework guides discussion and recommendation-making. It's not a checklist or algorithm—it's a tool to ensure consistent evaluation of key factors. The final decision combines:
- Framework guidance
- SME expertise
- Design phase insights
- Organizational constraints

### Key Decision Criteria

#### 1. Performance Type & Complexity
**What type of performance is required?**

- **Knowledge/Recognition** (e.g., identify components, recall procedures)
  - Recommendation: Can be effectively delivered via e-learning
  - Considerations: Ensure interactive practice, not passive reading

- **Procedural Performance** (e.g., follow a step-by-step process)
  - Recommendation: E-learning with virtual labs or simulations preferred; ILT if physical equipment essential
  - Considerations: Can virtual labs, OnCourse labs, or simulations replicate the procedure?

- **Physical Performance** (e.g., hands-on assembly, manipulation, tactile feedback)
  - Recommendation: ILT or Blended (e-learning theory + classroom hands-on)
  - Considerations: Is remote hardware access or VR simulation sufficient, or is in-person required?

- **Complex Troubleshooting** (e.g., diagnose failures, recommend solutions)
  - Recommendation: Blended or ILT (instructor coaching essential for complex reasoning)
  - Considerations: Does learner need real-time feedback and guidance?

---

#### 2. Instructor Involvement & Coaching
**Is instructor coaching, facilitation, feedback, or mentoring required?**

- **High Instructor Involvement** (real-time feedback, complex reasoning, individual coaching)
  - Recommendation: ILT or Blended
  - Examples: Advanced troubleshooting, certification labs, new technician onboarding
  
- **Moderate Instructor Involvement** (setup, facilitation, Q&A, guided practice)
  - Recommendation: Blended (e-learning + synchronous support)
  - Examples: New feature training, hands-on labs with instructor available
  
- **Low Instructor Involvement** (self-paced, embedded guidance in content, peer support)
  - Recommendation: E-learning with guided practice and embedded videos
  - Examples: Reference guides, foundational knowledge, compliance training, refresher training

---

#### 3. Audience Scale & Distribution
**What is the audience size and geography?**

- **Large, Geographically Distributed Audience** (50+ learners, multiple locations)
  - Recommendation: **E-learning** (most cost-effective and accessible)
  - Considerations: Travel costs, scheduling logistics, scale economics
  - Note: Can include synchronous support (web sessions) if needed

- **Moderate, Regional Audience** (20-50 learners, regional clusters)
  - Recommendation: **Blended** (e-learning self-paced + periodic classroom sessions)
  - Considerations: Combine local clusters for cost-effective ILT days

- **Small, Co-located Audience** (Under 20 learners, same location)
  - Recommendation: **ILT or Blended** (practical to gather for hands-on training)
  - Considerations: Feasibility of classroom space and instructor time

---

#### 4. Speed & Accessibility
**How quickly do learners need the skills? What access patterns matter?**

- **High Speed Needed** (urgent skill gap, just-in-time training)
  - Recommendation: **E-learning** (on-demand, immediate access, no scheduling delays)
  - Examples: Urgent product updates, emergency procedures, new field deployment

- **Moderate Speed** (training needed within weeks)
  - Recommendation: **Blended** (e-learning self-paced + instructor sessions scheduled)
  - Allows flexibility while maintaining timeline

- **Ongoing/Scheduled** (regular training, certification, compliance)
  - Recommendation: **ILT or Blended** based on other factors
  - Can schedule classroom sessions at convenient intervals

**On-Demand Value**: E-learning enables:
- Learners to access content when needed (not tied to class schedules)
- Refresher training without waiting for next cohort
- New hires to ramp up independently

---

#### 5. Hardware & Environment Requirements
**What equipment or environment is needed to learn?**

- **Specialized Hardware Required** (equipment, machines, installations)
  - Questions to ask:
    - Can virtual labs or OnCourse remote labs simulate the equipment?
    - Can learners access hardware remotely?
    - Is hands-on touch/tactile feedback essential?
  
  - **If simulation/remote access sufficient** → E-learning
  - **If in-person essential** → ILT or Blended

- **Standard Computer/Network Access** (software, systems, cloud-based)
  - Recommendation: **E-learning** (virtual labs and simulations work well)
  - Examples: Software training, network troubleshooting, remote system administration

- **No Special Equipment** (conceptual knowledge, procedures, soft skills)
  - Recommendation: **E-learning** (most cost-effective)
  - Can include videos, simulations, guided practice

---

#### 6. Content Complexity & Audience Level
**How advanced is the content? What's the learner's starting point?**

- **Foundational/Introductory** (new to topic, no prerequisites)
  - Recommendation: **E-learning** (scales well, self-paced learning curve works)
  - Supports learners moving at own pace through new concepts
  - Examples: Product overview, basic concepts, compliance training

- **Intermediate** (some experience, building on existing knowledge)
  - Recommendation: **E-learning or Blended**
  - Depends on other factors (scale, instructor involvement, etc.)
  - Can work as guided e-learning with optional instructor support

- **Advanced/Specialized** (expert-level, complex reasoning)
  - Recommendation: **ILT or Blended** (instructor expertise and real-time feedback valuable)
  - Learners benefit from guided discovery and coaching
  - Examples: Advanced troubleshooting, design decisions, complex system analysis

---

### E-Learning Includes More Than SCORM

When considering "e-learning," think beyond passive SCORM modules. Modern e-learning can include:

- **Guided Practice**: Step-by-step interactive walkthroughs
- **Simulations**: Virtual equipment or system behavior
- **Virtual Labs**: Remote access to real equipment via OnCourse or similar
- **Embedded Videos**: Demonstrations, procedures, expert walkthroughs
- **Scenario-Based Activities**: Real-world situations to solve
- **Spaced Repetition**: Reinforcement over time
- **Knowledge Checks**: Embedded formative assessment

This means many traditionally "hands-on" topics can still be effectively delivered as e-learning with the right interactive elements.

---

### Making the Recommendation

**Step 1: Gather Input During Intake**
During the initial project discussion, collect:
- Performance requirements (what learners must do?)
- Audience size and distribution (how many? where?)
- Timeline (how quickly needed?)
- Hardware/environment constraints
- Instructor availability
- Budget constraints

Set expectations: "We'll make a modality recommendation after the design phase."

**Step 2: Complete Design Phase**
Work through outcomes, objectives, and activity coverage. This clarifies:
- Complexity of what learners must learn
- Type of practice needed (knowledge check, hands-on, coached practice?)
- Assessment requirements

**Step 3: Make the Recommendation**
Use the decision criteria above to recommend:
- **E-Learning** (self-paced, scalable, on-demand)
- **ILT** (Instructor-Led Training, synchronous, personalized coaching)
- **Blended** (e-learning + classroom, combines benefits)

Include rationale: "We recommend e-learning because [factors]. This enables [outcomes]."

**Step 4: Confirm with Stakeholders**
Present recommendation with reasoning. Address any conflicts with original request:
- "You asked for classroom, but we recommend blended because..."
- Explain why the recommendation better serves learners and organization

---

### Decision Framework Summary Table

| Factor | E-Learning | ILT | Blended |
|--------|-----------|-----|---------|
| **Performance Type** | Knowledge, procedural (with sims) | Physical, complex troubleshooting | Mixed complexity |
| **Instructor Involvement** | Low to moderate | High | Moderate |
| **Scale** | Large, distributed | Small, co-located | Moderate |
| **Speed** | Immediate/on-demand | Scheduled | Flexible |
| **Hardware** | Simulatable or remote | Physical access needed | Both possible |
| **Audience Level** | Foundational, intermediate | Advanced, specialized | Any level |
| **Cost** | Lower (scales well) | Higher (per-learner) | Moderate |
| **Flexibility** | High (self-paced) | Low (scheduled) | High |

---

### Questions to Guide Discussion

**Use these questions in design conversations to determine modality:**

1. "What exactly must learners be able to DO?" (Performance)
2. "Could they learn this effectively from a simulation, or do they need physical equipment?" (Hardware)
3. "How many people need this training, and where are they?" (Scale)
4. "Do they need coaching and real-time feedback to master this?" (Instructor)
5. "When do they need to know this?" (Speed)
6. "What's their current knowledge level?" (Complexity)
7. "Is an instructor available to support this training?" (Resources)

**The answers to these questions drive the recommendation.**

## Step 4: Map to Required Deliverables

Once you know the publication strategy, the required deliverables are determined.

### E-Learning Modality

**Output Format**: SCORM 1.2 module (1 output file)

**Module Contents** (components aggregated into the SCORM):
- Lectures (organized by objective)
- Knowledge checks (embedded within lectures)
- Labs (if applicable, interactive scenarios)
- Quiz questions (graded assessment)

### Classroom Modality

**Output Formats**: 
- Instructor presentation (RevealJS or PowerPoint)
- Student labs and practicals
- Assessment quizzes and rubrics
- Handouts/reference guides (PDF)

### Blended Modality

**Output Formats**: All of the above
- SCORM module (e-learning component)
- Instructor presentation (classroom component)
- Lab manuals and practicals
- Reference guides

### Customer-Facing Adds
- TLB (Training Lab Book)
- TSL (Training Summary/Lesson Book)

## Step 5: Plan Activity Coverage

**Critical Rule**: Every learning outcome must have three types of activities that collectively cover the outcome:

1. **Passive Activity** (Lecture) — Learner gains exposure
   - Explanation, terminology, concepts, demonstrations, examples
   - Includes embedded knowledge checks
   - Can be at objective level (included in outcome) or outcome level

2. **Interactive Activity** (Lab/Practice) — Learner applies with guidance
   - Hands-on lab exercise, guided practice, scenario-based activity
   - With feedback or guidance (not assessment)
   - Can be at objective level or outcome level

3. **Assessment Activity** (Quiz/Practical) — Learner demonstrates mastery
   - Quiz questions (graded, tied to outcome)
   - Practical exercise or project
   - Graded and scored
   - Can be at objective level (if standalone) or outcome level

### Coverage Validation Location

**Outcome-Level Coverage** (Always checked):
- Every outcome must have P+I+A across all its objectives
- Non-standalone objectives contribute their lecture content to outcome-level coverage
- Interactive and assessment activities can be aggregated from multiple objectives

**Objective-Level Coverage** (Only for standalone objectives):
- Standalone objectives must each have their own complete P+I+A coverage
- Non-standalone objectives are NOT validated for individual P+I+A coverage (they roll up to outcome)

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
