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

## Step 3: Determine Delivery Strategy (RAU Recommendation)

**Last synced**: 2026-08-20  
**Rule reference**: [.claude/rules/content-design-validation.md Rule 5](/.claude/rules/content-design-validation.md#rule-5-delivery-strategy--deliverable-alignment)

**Delivery strategy** answers: "What delivery modality and packaging best serves this skill, and what will the customer receive?"

**Important**: RAU training professionals make the **delivery strategy recommendation** based on instructional design principles, not on stakeholder requests alone. While we gather input during intake, the final recommendation comes after the design phase when we have full clarity on learning requirements.

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

Set expectations: "We'll make a delivery strategy recommendation after the design phase."

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

**Use these questions in design conversations to determine the delivery strategy:**

1. "What exactly must learners be able to DO?" (Performance)
2. "Could they learn this effectively from a simulation, or do they need physical equipment?" (Hardware)
3. "How many people need this training, and where are they?" (Scale)
4. "Do they need coaching and real-time feedback to master this?" (Instructor)
5. "When do they need to know this?" (Speed)
6. "What's their current knowledge level?" (Complexity)
7. "Is an instructor available to support this training?" (Resources)

**The answers to these questions drive the recommendation.**

## Step 4: Map Design Activities to Development Deliverables

In this step, the design activities you defined (lecture, labs, quizzes, etc.) are translated into **deliverables**—the actual markdown files SMEs will author during development.

**Key principle**: Every activity defined during design maps directly to a deliverable during development:
- **Lecture activity** → `lecture.md` deliverable
- **Knowledge check activity** → `knowledge-check.md` deliverable  
- **Lab activity** → `lab.md` deliverable
- **Quiz activity** → `quiz-questions.md` deliverable

This step formalizes what files need to be created based on your design decisions.

### Decision Rules: Delivery Strategy and Deliverable Generation

Based on your delivery strategy from Step 3, define which deliverables (files) will be authored:

#### E-Learning Delivery Strategy

**Deliverable Structure Rules**:
- One folder per outcome (using outcome title in kebab-case)
- One subfolder per objective within each outcome
- Outcome-level aggregated lecture and quiz at the outcome folder level

**Deliverables (Files SMEs Author)**:
- `objective-##/lecture.md` — Objective-level lecture deliverable (required for all objectives)
- `objective-##/knowledge-check.md` — Knowledge check deliverable (required for all objectives)
- `objective-##/lab.md` — Interactive lab deliverable (required for all objectives in e-learning)
- `outcome-##-lecture.md` — Aggregated outcome lecture (includes all objective lectures)
- `outcome-##-quiz.md` — Outcome-level assessment (pulls from all objective quiz questions)
- `objective-##/quiz-questions.md` — Objective-level quiz question deliverables (required for all objectives)

**Rule**: Every objective in e-learning offering gets a complete P+I+A coverage: lecture, knowledge check, lab, and quiz question deliverables.

#### Classroom Delivery Strategy (ILT)

**Deliverable Structure Rules**:
- One folder per outcome
- One subfolder per objective within each outcome
- Outcome-level presentation and practical assessment

**Deliverables (Files SMEs Author)**:
- `objective-##/lecture.md` — Objective-level lecture deliverable (required for all objectives)
- `outcome-##-presentation.md` — Presentation deliverable for instructor delivery
- `outcome-##-lab.md` — Hands-on practical lab deliverable (required for all outcomes)
- `outcome-##-quiz.md` — Assessment and rubrics deliverable
- `objective-##/quiz-questions.md` — Objective quiz question deliverables (optional for ILT; assessment may be practical)

**Rule**: Classroom delivery emphasizes presentation and practical labs; objectives contribute lecture deliverables rolled into outcome-level presentation.

#### Blended Delivery Strategy

**Deliverable Structure Rules**:
- One folder per outcome
- One subfolder per objective within each outcome
- Both e-learning and classroom deliverables

**Deliverables (Files SMEs Author)**:
- Same as **E-Learning** (for self-paced online component)
- PLUS same as **Classroom** (for synchronous/classroom component)
- `outcome-##-handout.md` — Reference guide deliverable for learners

**Rule**: Blended offering requires all deliverables from both E-Learning and Classroom strategies.

### Decision Rules: Standalone vs. Non-Standalone Objectives

#### Non-Standalone Objectives (Default)

**File Generation Rule**:
- Lecture authored separately but included in outcome-level lecture via `!include()`
- Knowledge checks authored separately, included in outcome lecture
- Labs and quizzes authored at outcome level (not individually)
- Tool generates: lecture.md, knowledge-check.md only at objective level

**Tool Decision**: Default to non-standalone unless explicitly marked otherwise.

#### Standalone Objectives

**File Generation Rule**:
- Each objective is a complete, independent unit
- Must have its own lecture, lab, and quiz (full P+I+A)
- May generate its own SCORM package
- Tool generates: lecture.md, lab.md, quiz-questions.md at objective level

**Tool Decision**: Only generate standalone file sets if objective is explicitly marked as "standalone: true" in design.

### Decision Rules: Labs and Practicals

**Lab Generation Logic**:

- **E-Learning**: Lab file always required (`objective-##/lab.md` or `outcome-##-lab.md`)
- **Classroom**: Lab file required at outcome level (`outcome-##-lab.md`)
- **Blended**: Both objective and outcome labs may be required; clarify in design

**Practical Exercise Generation Logic**:

- **If outcome requires hands-on assessment**: Generate `outcome-##-practical.md` with rubrics
- **If outcome assessment is quiz-based**: Generate `outcome-##-quiz.md` with questions
- **If both**: Generate both files

### Summary: What the Tool Needs to Generate Deliverables

The tool needs to know:
1. **Delivery Strategy**: E-Learning, ILT, or Blended
2. **Outcomes**: Count and titles
3. **Objectives per outcome**: Count and titles, plus standalone designation
4. **Labs required**: Yes/No per outcome
5. **Assessment type**: Quiz-based, practical-based, or both
6. **Standalone exceptions**: Which objectives are standalone vs. non-standalone

Armed with these decisions, the tool generates **deliverables**:
- ✅ Folder structure (folders and subfolders for organization)
- ✅ All required markdown files (.md) with proper YAML frontmatter
- ✅ Template structures (lecture headers, lab setup, quiz structure)
- ✅ Naming conventions applied consistently
- ✅ Include statements pre-populated where needed

These deliverables are the actual files SMEs will author during the development phase.

---

### Publications and Offerings

**Publications** are all outputs generated by the build system from your deliverables:
- SCORM 1.2 modules (.zip files for LMS)
- PDF documents (printable guides, lab manuals)
- HTML presentations (RevealJS)
- Video recordings (MP4 with narration)
- Reference materials

**Offerings** are customer-facing packages built from publications. These are planned separately, after core design is approved:
- **E-learning offering**: Complete SCORM module enrolled in LMS (built from publications)
- **Classroom offering**: Instructor presentation + printed lab manual + handouts (built from publications)
- **Blended offering**: Both online and classroom components (built from publications)

Some publications can be further packaged into customer offerings:
- **TLB (Training Lab Book)** — Standalone lab manual offering (derived from lab publications)
- **TSL (Training Summary/Lesson Book)** — Standalone reference guide offering (derived from lecture publications)

These customer-facing offerings are typically created after the primary skill design and publication strategy are validated.

## Step 5: Generate File Shell (Automated)

**Input**: Your completed design from Steps 1-4  
**Tool**: Claude Code or equivalent automation  
**Output**: Complete file structure + empty activity shells with frontmatter

Ask Claude Code: **"Generate the file shell for this design"**

The tool will:
1. Analyze your modality, outcomes, and objectives
2. Apply the business logic from Step 4
3. Generate folder structure
4. Create all required .md files with YAML frontmatter
5. Pre-populate template structures (lecture headers, lab sections, quiz structure)
6. Output a summary of what was generated

**Example Output**:

```text
skills/troubleshoot-hydraulic-systems/
├── diagnose-pump-failures/
│   ├── objective-01/
│   │   ├── lecture.md (frontmatter: skill, docType, title)
│   │   ├── knowledge-check.md
│   │   ├── lab.md
│   │   └── quiz-questions.md
│   ├── objective-02/
│   │   ├── lecture.md
│   │   ├── knowledge-check.md
│   │   ├── lab.md
│   │   └── quiz-questions.md
│   ├── outcome-01-lecture.md (aggregates objectives via !include)
│   └── outcome-01-quiz.md
└── media/
    └── (ready for images/diagrams)

Generated 7 files with YAML frontmatter and template structures.
```

## Step 6: Review & Refine Generated Structure

**Input**: Generated file shell from Step 5  
**Participants**: Design team (SMEs, instructional designer, technical writer)  
**Output**: Approved, modified file structure ready for content authoring

### Review Checklist

- [ ] File structure matches your mental model of the design
- [ ] All required files are present
- [ ] Naming conventions are consistent
- [ ] YAML frontmatter is correct (title, skill ID, docType)
- [ ] Template structures (lecture headers, sections) make sense
- [ ] Include statements are pre-positioned correctly
- [ ] Any missing files? (e.g., you need a `media/` subfolder?)
- [ ] Any files that shouldn't be there? (e.g., remove a lab you don't need)
- [ ] File count matches expectations

### Modifications

If the generated structure doesn't match your design:

**Common Changes**:
- Rename files to better reflect content
- Add additional folders for media organization
- Merge or split objectives if structure feels wrong
- Add standalone designation to objectives that need independence
- Remove lab or quiz files if those activities won't be authored

**Make changes before moving to Step 7.** It's easier to adjust the structure now than to reorganize files once content authoring begins.

## Step 7: Validate Your Design

### Self-Check Checklist
- ☐ All outcomes follow ABCD method
- ☐ All objectives support outcomes
- ☐ Coverage matrix complete (passive + interactive + assessment per outcome)
- ☐ Standalone designations documented
- ☐ Delivery strategy selected with rationale
- ☐ Deliverables mapped to design activities
- ☐ File structure reviewed and approved by team
- ☐ All deliverable files named correctly and in place
- ☐ CDD Workbook entry complete

### Claude Code Validation

Ask Claude Code: **"Validate my content design"**

Claude Code will check for ABCD completeness, objective alignment, coverage matrix, delivery-strategy-deliverable alignment, AND file structure completeness.

### Escalation to Aba Azeem

If Claude Code finds complex design questions, it escalates to **Aba Azeem** (aba.azeem@rockwellautomation.com) for guidance.

### Design Approval & Export Prep

Once validation passes:
1. Design is **approved for content authoring**
2. File structure is **locked** (no more structural changes without escalation)
3. Team begins **content development** using the generated shells
4. Optional: Export design to build.yaml or other publishing systems

For more details, see the complete guide in CLAUDE.md Section 0.

## Step 8: Plan Activity Coverage

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
