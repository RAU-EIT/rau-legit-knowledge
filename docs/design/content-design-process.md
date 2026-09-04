# Content Design Process Guide

This guide walks you through the content design phase, where learning intent is defined and everything that must be authored is derived from it. Complete this phase BEFORE starting content development.

**Timeline**: Typically 1-2 weeks per skill set
**Inputs**: Skill statements (already defined), target audiences and roles, stakeholder requirements
**Outputs**: Learning outcomes, learning objectives, delivery strategy, offerings, publications, activities, deliverables list, content database entry
**Validation**: Claude Code checks design completeness; escalates complex questions to Aba Azeem

## How This Process Works: Derivation, Top-Down

Design **derives downward**. Each step's output is the next step's input:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
    (Step 3)          (Step 4)      (Step 5)       (Step 6)      (Step 7)
```

You confirm *how* the skill will be taught, which tells you *what students receive*, which tells you *what must be built*, which tells you *what learners must do*, which tells you *what SMEs must produce*.

The build system later runs this chain in reverse: deliverables build into publications, which are packaged into offerings. Both directions describe the same chain. See [Terminology Glossary: The Two Directions](../terminology-glossary.md#the-two-directions) for why this matters.

### The Nine Steps

| # | Step | Produces |
| --- | --- | --- |
| 1 | Define Learning Outcomes (ABCD) | Outcomes with clear titles |
| 2 | Define Learning Objectives | 2-5 per outcome, with standalone flags |
| 3 | Determine Delivery Strategy | E-learning / classroom / blended + rationale |
| 4 | Define Offerings | Student-facing packages per audience/role |
| 5 | Define Publications | Which publications serve which audiences/roles |
| 6 | Determine Activities | Passive + Interactive + Assessment per outcome |
| 7 | Define Deliverables | Activities + supporting assets |
| 8 | Validate & Refine Design | Reviewed, corrected design |
| 9 | Load into Content Database | LeGIT project files + DevOps export |

Steps 5-7 are intended to be **auto-generated** from business logic. Step 8 is the human gate where the team adds, changes, or removes anything the generator produced.

---

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

Every outcome needs a **clear, concise title** for use in common outputs (PDFs, presentations, SCORM modules). The title also becomes the outcome's folder name in kebab-case.

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

---

## Step 2: Define Learning Objectives

Break each outcome into **2-5 step-level objectives**. Objectives are the building blocks learners work through to reach full outcome competence.

### Why 2-5 Objectives Per Outcome?

**Why at least 2 (not 1)**:

- An outcome with only 1 objective suggests the outcome itself is **too small or granular**
- The outcome should represent a meaningful, substantive skill, not just one simple task
- If you find yourself with 1 objective, consider whether that's really an outcome or just an objective itself
- Multiple objectives allow for logical progression and scaffolding of learning

**Why typically 5 or fewer (not 10+)**:

- **Cognitive load**: Too many objectives (7+) overwhelms learners and becomes hard to manage
- **Course coherence**: 10+ objectives in one outcome suggests the outcome is **too broad and should split into 2-3 outcomes**
- **Instructional design**: Each objective needs adequate lecture, practice, and assessment; more objectives mean more content complexity
- **Practical delivery**: Outcome-level labs and quizzes must assess all objectives; too many makes assessment unwieldy

**Decision guide**:

- **If you have 1 objective**: Reconsider whether this is really an outcome, or if you need to expand the scope
- **If you have 2-5 objectives**: You're in the sweet spot for coherent, manageable outcomes
- **If you have 6-10 objectives**: Consider combining related objectives or splitting the outcome
- **If you have 10+ objectives**: Definitely split into multiple outcomes; the scope is too broad

### Characteristics of Good Objectives

- Represent step-level learning toward outcome competence
- Are specific and observable behaviors
- Support outcomes but do not replace them
- Logically progress from foundational to complex
- Collectively enable the learner to achieve the full outcome

### Standalone vs. Non-Standalone Objectives

This designation matters downstream: it affects publication scoping (Step 5), activity coverage validation (Step 6), and which deliverables get generated (Step 7).

**Non-Standalone Objective (DEFAULT - Recommended)**:

- Authored separately but rolls into parent outcome for delivery
- Each objective has its own **lecture** (included in outcome-level lecture)
- Shares **interactive activities and assessment** at the outcome level with other objectives
- Works for all delivery strategies: e-learning, classroom, blended
- Simpler to manage: lectures are modular, labs assess the whole outcome together

**Standalone Objective (EXCEPTION - Use When Needed)**:

- Designed as an independent learning unit (for specific e-learning micro-credentials)
- Must have its own complete coverage: passive (lecture) + interactive (lab/practice) + assessment (quiz or practical)
- Learner can complete it independently without other objectives
- May generate its own publication (e.g., a separate SCORM package)
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
- You specifically need a separate publication per objective
- Examples: micro-credentials, prerequisite modules, remedial learning units

### Output of This Step: The Design Hierarchy

Steps 1 and 2 together produce the **design hierarchy**: the count and structure of outcomes and objectives, plus standalone flags. This is a required input to publication scoping in Step 5.

---

## Step 3: Determine Delivery Strategy (RAU Recommendation)

**Last synced**: 2026-09-03
**Rule reference**: [.claude/rules/content-design-validation.md Rule 5](/.claude/rules/content-design-validation.md#rule-5-delivery-strategy--deliverable-alignment)

**Delivery strategy** answers one question: *"How will this skill be taught?"*

It is the **first link in the derivation chain**. Once confirmed, it determines the offerings (Step 4), which determine the publications (Step 5), which determine the activities (Step 6), which determine the deliverables (Step 7). Get this wrong and everything downstream is wrong.

**Important**: RAU training professionals make the **delivery strategy recommendation** based on instructional design principles, not on stakeholder requests alone. While we gather input during intake, the final recommendation comes after the design phase when we have full clarity on learning requirements.

> **Terminology note**: this decision is called **delivery strategy**: never "publication strategy" or "offering strategy." Publications and offerings are separate, downstream decisions with their own steps. See [Terms We Do Not Use](../terminology-glossary.md#terms-we-do-not-use).

### Decision Framework (Not a Rigid Rule)

This framework guides discussion and recommendation-making. It's not a checklist or algorithm; it is a tool to ensure consistent evaluation of key factors. The final decision combines:

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
- **Audiences and roles** (who needs this? which job roles?)
- Audience size and distribution (how many? where?)
- Timeline (how quickly needed?)
- Hardware/environment constraints
- Instructor availability
- Budget constraints

Set expectations: "We'll make a delivery strategy recommendation after the design phase."

**Note**: The audiences and roles captured here are a required input to Steps 4 and 5. Offerings and publications are scoped *per audience/role*, so an incomplete role list at intake produces an incomplete publication set later.

**Step 2: Complete Outcomes and Objectives**
Work through Steps 1-2. This clarifies:

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
| --- | --- | --- | --- |
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

### What This Step Determines

Once the delivery strategy is confirmed, you can proceed to define offerings (Step 4). Do not skip ahead to deliverables; the intervening steps are what make the deliverables list correct.

---

## Step 4: Define Offerings

**Inputs**: Delivery strategy (Step 3), audiences and roles (from intake)
**Output**: The set of student-facing offerings, one or more per audience/role

An **offering** is what a student actually enrolls in, purchases, or downloads. Offerings are defined as soon as the delivery strategy is confirmed, because the strategy determines what form they can take.

### Offerings by Delivery Strategy

| Delivery Strategy | Typical Offerings |
| --- | --- |
| **E-Learning** | A self-paced online course enrolled in via the LMS |
| **Classroom (ILT)** | An instructor-led course with a scheduled session and location |
| **Blended** | A self-paced online course **plus** a classroom or regional practicum |

### Scoping Offerings Per Audience/Role

The same skill design can produce **different offerings for different roles**. This is the main reason offerings are their own step: a single skill rarely serves a single audience identically.

**Example**: one hydraulic troubleshooting skill, two roles:

- **Field Technician** → blended offering: self-paced e-learning course + regional hands-on practicum
- **Application Engineer** → e-learning offering only: the same theory content, no practicum, as a reference course

Both draw on the same outcomes and objectives. They differ in which publications they include, which is exactly what Step 5 works out.

### What to Record Per Offering

- Offering name (student-facing)
- Target audience/role
- Delivery strategy it implements
- Which outcomes it covers (all, or a subset)
- Supporting materials beyond the training itself (certificates, job aids, reference guides)

### A Single Publication Is Not an Offering

A lab manual, a reference guide, or a deck is a **publication**: one component of an offering, not an offering in itself. Do not list individual publications here.

The exception is narrow: a publication becomes an offering only when it is **deliberately delivered to students standalone**, for example as tailored training. If that is the plan, record it as an offering and say so explicitly ("lab manual delivered as tailored training"), so it is clear the standalone delivery is intentional rather than a publication misfiled as an offering.

Publications, including TLB (Training Lab Book) and TSL (Training Summary/Lesson Book), are defined in Step 5.

---

## Step 5: Define Publications

**Inputs**: Offerings (Step 4), audiences and roles (from intake), design hierarchy (Steps 1-2)
**Output**: The publication set, each publication tagged with audience/role, scope, and publication type

A **publication** is a specific output the build system will produce. You decide publications during design because **this decision is what tells you which activities and deliverables are required**. Deciding it later means discovering later that you authored the wrong things.

### What Each Publication Needs

| Field | Meaning | Example |
| --- | --- | --- |
| **Name** | What this publication is | "Hydraulic Troubleshooting - Technician SCORM" |
| **Audience/role** | Who it serves | Field Technician |
| **Scope** | What it covers | Outcomes 1-3 |
| **Publication type** | Its build format (`docType`) | `scorm1.2` |
| **Offering** | Which offering(s) it feeds | Technician blended offering |

### Publications by Delivery Strategy

| Delivery Strategy | Publications Required |
| --- | --- |
| **E-Learning** | SCORM module (`scorm1.2`) |
| **Classroom (ILT)** | Instructor presentation (`revealjs` or `pptx`), lab manual (`print`), practical assessment (`print`) |
| **Blended** | All of the above, plus a learner handout (`print`) |

See [Publication Type](../terminology-glossary.md#publication-type) for the full list and its current verification status.

### Named Publication Formats

Some publications have established RAU names:

- **TLB (Training Lab Book)**: a lab-focused publication, typically `print`
- **TSL (Training Summary/Lesson Book)**: a reference-focused publication, typically `print`

These are **publications**, not offerings. They become part of an offering; they are only an offering in their own right if deliberately delivered standalone (see Step 4).

### How the Design Hierarchy Scopes Publications

The design hierarchy from Steps 1-2 determines **how many** publications you need and at what granularity:

- **One publication per skill**: the default. All outcomes covered in one module or manual.
- **One publication per outcome**: when outcomes are large enough to be delivered separately, or when different roles need different outcome subsets.
- **One publication per standalone objective**: required when objectives are marked standalone, since a standalone objective is by definition independently completable.

A skill with 3 outcomes and no standalone objectives, delivered as e-learning, may need just one SCORM publication. The same skill with one standalone objective needs two: the main module plus a separate one for the standalone unit.

### Business Logic: Offerings and Hierarchy to Publications

<!-- TBD: The automated offering + design-hierarchy → publication derivation rules are not
     yet specified. This step's inputs, outputs, and required fields (above) are settled;
     the mapping rules that generate the publication set are to be defined. Until then,
     publications are determined by the design team using the tables above. -->

This step is intended to be automated. The inputs, outputs, and per-publication fields above are settled; the **derivation rules themselves are still to be specified**. Until they are, the design team determines the publication set manually using the tables above.

---

## Step 6: Determine Activities

**Inputs**: Publications (Step 5), design hierarchy (Steps 1-2)
**Output**: The activity set: Passive + Interactive + Assessment coverage per outcome

**Activities** describe what learners will *do*. They are **derived from the publications** that need to be built, not invented freehand; a SCORM module requires a different activity mix than a printed lab manual.

### The Three Activity Types

**Critical Rule**: Every learning outcome must have all three types, collectively covering the outcome.

1. **Passive Activity** (Lecture): Learner gains exposure
   - Explanation, terminology, concepts, demonstrations, examples
   - Includes embedded knowledge checks
   - Can be at objective level (included in outcome) or outcome level

2. **Interactive Activity** (Lab/Practice): Learner applies with guidance
   - Hands-on lab exercise, guided practice, scenario-based activity
   - With feedback or guidance (not assessment)
   - Can be at objective level or outcome level

3. **Assessment Activity** (Quiz/Practical): Learner demonstrates mastery
   - Quiz questions (graded, tied to outcome)
   - Practical exercise or project
   - Graded and scored
   - Can be at objective level (if standalone) or outcome level

### Coverage Validation Location

**Outcome-Level Coverage** (Always checked):

- Every outcome must have Passive + Interactive + Assessment across all its objectives
- Non-standalone objectives contribute their lecture content to outcome-level coverage
- Interactive and assessment activities can be aggregated from multiple objectives

**Objective-Level Coverage** (Only for standalone objectives):

- Standalone objectives must each have their own complete Passive + Interactive + Assessment coverage
- Non-standalone objectives are NOT validated for individual coverage (they roll up to the outcome)

### Business Logic: Publications to Activities

<!-- TBD: The automated publication → activity derivation rules are not yet specified.
     This step's inputs, outputs, and the coverage rules above are settled; the mapping
     matrix that derives the required activity set from each publication type is to be
     defined. Until then, activities are planned by the design team against the coverage
     rules above. -->

This step is intended to be automated: given the publication set, business logic derives which activities each publication requires. The inputs, outputs, and coverage rules above are settled; the **mapping matrix is still to be specified**. Until it is, the design team plans activities manually and validates them against the coverage rules.

---

## Step 7: Define Deliverables

**Inputs**: Activities (Step 6), publications (Step 5), design hierarchy (Steps 1-2)
**Output**: Complete deliverables list: everything SMEs must produce

A **deliverable** is anything an SME must produce. Deliverables are the **superset** of activities plus everything else the skill needs that LeGIT does not author:

```text
Deliverables  =  every Activity  +  supporting assets
```

Most deliverables are authored content. Some are not, and those still have to be planned, assigned, and tracked.

### Part A: LeGIT-Authored Deliverables

These are markdown files with YAML frontmatter, built by the LeGIT pipeline.

#### Structure Rules (All Delivery Strategies)

- One folder per outcome, named with the outcome title in kebab-case
- One subfolder per objective within each outcome
- Outcome-level aggregate files at the outcome folder level

#### Objective-Level Deliverables

| Deliverable | When |
| --- | --- |
| `objective-##/lecture.md` | **Always**: every objective needs lecture content |
| `objective-##/knowledge-check.md` | **Always**: embedded ungraded checks |
| `objective-##/quiz-questions.md` | **Always**: feeds the outcome-level quiz question pool |
| `objective-##/lab.md` | **Only if the objective is standalone** |
| `objective-##/media/` | As needed |

**Why `quiz-questions.md` is always authored but `lab.md` is not**: quiz questions are authored per objective so the outcome quiz can draw a question pool traceable to each objective. Labs are authored at the outcome level because a non-standalone objective shares its interactive activity with the rest of the outcome. A standalone objective is the exception: it must be independently completable, so it needs its own lab.

#### Outcome-Level Deliverables

| Deliverable | When |
| --- | --- |
| `outcome-##-lecture.md` | **Always**: aggregates objective lectures via `!include()` |
| `outcome-##-quiz.md` | **Always**: draws from objective quiz question pools |
| `outcome-##-lab.md` | When the outcome has non-standalone objectives (the usual case) |
| `outcome-##-presentation.md` | Classroom or blended delivery |
| `outcome-##-practical.md` | When outcome assessment is hands-on rather than quiz-based |
| `outcome-##-handout.md` | Blended delivery, or when a learner reference is planned |

#### Assessment Type Logic

- **If outcome assessment is quiz-based**: `outcome-##-quiz.md`
- **If outcome requires hands-on assessment**: `outcome-##-practical.md` with rubrics
- **If both**: both files

### Part B: Supporting Assets

These are required by the skill and produced by the SME, but **not authored or built by LeGIT**. They are still first-class deliverables: planned here, tracked, and exported to DevOps.

| Category | Examples |
| --- | --- |
| **Virtual machines / test environments** | A hydraulic simulator VM; a preconfigured controller test bench |
| **Project files** | Studio 5000 `.ACD` files, configuration exports |
| **Lab start and finish files** | Sequential lab checkpoints so learners can resume or verify |
| **Externally produced media** | Professionally shot video, vendor-supplied diagrams |
| **Supporting documentation** | Setup guides for instructors, equipment lists |

**Why these must be captured at design time**: a lab activity that depends on a VM is not deliverable without the VM. Omitting it from the deliverables list means the lab cannot run and the gap surfaces during development instead of during design.

For each supporting asset, record what it is, which activity or deliverable depends on it, and who owns it.

### Part C: What the Generator Needs

To generate the deliverables list, the tool needs:

1. **Delivery strategy**: E-Learning, ILT, or Blended
2. **Offerings**: Names, audiences/roles, outcome coverage
3. **Publications**: Names, audiences/roles, scope, publication type
4. **Outcomes**: Count and titles
5. **Objectives per outcome**: Count, titles, and standalone designation
6. **Assessment type per outcome**: Quiz-based, practical-based, or both
7. **Supporting-asset dependencies**: Which activities require VMs, project files, or other assets

Given these, the generator produces the complete deliverables list. See [File Mapping Guide](./file-mapping-guide.md) for the resulting file structure.

---

## Step 8: Validate & Refine Design

**Inputs**: The generated publications, activities, and deliverables from Steps 5-7
**Participants**: Design team (SMEs, instructional designer, technical writer)
**Output**: A reviewed, corrected, approved design

Steps 5-7 are generated. This step is the **human gate**: the team reviews what the generator produced and adds, changes, or removes anything that is wrong. The generator applies business logic; it does not have judgment about your specific skill.

### Review the Generated Set

Walk through each generated layer and confirm it matches the team's intent:

**Publications**:

- [ ] Every audience/role from intake is served by at least one publication
- [ ] Publication scope matches the design hierarchy
- [ ] Publication types are correct for the delivery strategy
- [ ] No publication is missing, and none is superfluous

**Activities**:

- [ ] Every outcome has Passive + Interactive + Assessment coverage
- [ ] Standalone objectives each have their own complete coverage
- [ ] Activities are appropriate to their publications
- [ ] Activity level (objective vs. outcome) is right for each

**Deliverables**:

- [ ] Every activity has at least one corresponding deliverable
- [ ] Naming conventions are applied consistently
- [ ] Objective folders contain `lecture.md`, `knowledge-check.md`, `quiz-questions.md`
- [ ] `lab.md` appears at objective level only for standalone objectives
- [ ] Supporting assets (VMs, project files) are captured with owners
- [ ] Nothing needed is missing; nothing listed is unnecessary

### Common Refinements

- Add a publication for a role that intake under-specified
- Remove a lab deliverable for an outcome that is assessed by practical instead
- Mark an objective standalone after realizing it will be taken in isolation
- Add a supporting asset the generator could not infer (a VM, a project file)
- Split an outcome whose objective count grew past 5
- Rename a deliverable to better reflect its content

**Make changes at this step.** It is far easier to correct the design now than after content authoring has begun.

### Self-Check Checklist

- ☐ All outcomes have clear, concise titles
- ☐ All outcomes follow the ABCD method
- ☐ All objectives support a documented outcome
- ☐ Standalone designations documented
- ☐ Delivery strategy selected with rationale
- ☐ Offerings defined per audience/role
- ☐ Publications defined with audience, scope, and type
- ☐ Coverage complete (Passive + Interactive + Assessment per outcome)
- ☐ Standalone objectives have individual coverage
- ☐ Deliverables list complete, including supporting assets
- ☐ Design reviewed and approved by the team

### Claude Code Validation

Ask Claude Code: **"Validate my content design"**

Claude Code checks ABCD completeness, objective alignment, coverage, delivery-strategy-to-deliverable alignment, and deliverable completeness. See [content-design-validation.md](/.claude/rules/content-design-validation.md) for the full rule set.

Result:

- ✅ **Passes**: Ready to load into the content database
- ⚠️ **Warnings**: Functional but has minor issues
- ❌ **Blockers**: Must fix before proceeding

### Escalation to Aba Azeem

If Claude Code finds complex design questions, it escalates to **Aba Azeem** (<aba.azeem@rockwellautomation.com>) for guidance. Response time: 1-2 business days.

---

## Step 9: Load into Content Database

**Input**: The validated, refined design from Step 8
**Output**: LeGIT project files created; deliverables exported to DevOps

The **content database** is the system of record for design data. Once the design is validated, it is loaded into the database, which drives the two downstream handoffs.

### What the Content Database Does

1. **Creates the LeGIT project files**: the folder structure and file shells with YAML frontmatter, following the conventions in the [File Mapping Guide](./file-mapping-guide.md)
2. **Exports the deliverables list to DevOps**: so authoring work can be tracked, assigned, and scheduled

SMEs do not hand-create the folder structure, and Claude Code does not scaffold it. The content database owns file creation.

> **Transitional note**: the content database is still being built. Until it is live, the **CDD Workbook remains the interim system of record** and its entry is still required. Record outcomes, objectives, delivery strategy, offerings, publications, coverage, and the deliverables list there. Treat the CDD Workbook as a fallback, not the target state.

### After the Load

1. Design is **approved for content authoring**
2. File structure is **locked**: no more structural changes without escalation
3. Team begins **content development** in the generated shells
4. Deliverables appear in DevOps for assignment and tracking

For authoring guidance, see the Content Development track documentation.

---

## Related Documents

- [Terminology Glossary](../terminology-glossary.md): definitions for every term used here
- [File Mapping Guide](./file-mapping-guide.md): the file structure deliverables take
- [Content Design Validation](/.claude/rules/content-design-validation.md): rules Claude Code checks

---

**Last Updated**: 2026-09-03
