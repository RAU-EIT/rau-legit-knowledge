# Content Design Validation Rules

Claude Code checks learning designs against these rules.

**Terminology**: this file uses the canonical terms defined in [docs/terminology-glossary.md](../../docs/terminology-glossary.md). In particular: the modality decision is **delivery strategy** (never "publication strategy" or "offering strategy"), and a **deliverable** is the superset of every activity plus supporting assets. "Task" is not a term in this system.

**Design derivation chain**: each rule below validates one link:

```text
Delivery Strategy  →  Offering  →  Publication  →  Activity  →  Deliverable
    (Step 3)          (Step 4)      (Step 5)       (Step 6)      (Step 7)
```

---

## Rule 1: ABCD Outcome Completeness

Every learning outcome must have:

- **Title**: Clear, concise outcome title for use in common outputs (e.g., "Analyze Hydraulic Components")
- **A (Audience)**: WHO is learning
- **B (Behavior)**: WHAT observable action demonstrates learning
- **C (Condition)**: WHEN/WHERE/HOW the behavior occurs
- **D (Degree)**: TO WHAT STANDARD

Example Valid:

- Title: "Analyze Hydraulic System Components"
- Statement: "Given a schematic diagram, the field technician will analyze components and explain function with 90% accuracy."

Example Invalid:

- "Outcome 1" (missing clear title)
- "Understand hydraulic principles." (missing A, B, C, D, vague title)

## Rule 2: Objective-to-Outcome Alignment

Every learning objective must support a documented learning outcome.

- Each objective traces back to its outcome
- Objectives represent step-level learning toward outcome
- No orphaned objectives (not tied to any outcome)

## Rule 3: Coverage Completeness (Outcome Level)

**Every learning outcome must have three types of activities that collectively cover the outcome**:

1. **Passive** (Lecture) - Learner gains exposure
2. **Interactive** (Lab) - Learner applies knowledge with guidance
3. **Assessment** (Quiz/Practical) - Learner demonstrates mastery

Coverage validation applies at the **outcome level** only:

- Coverage can be satisfied directly (outcome has all three types), OR
- Coverage can be satisfied through aggregation of objective-level activities
- **Objective-level coverage is NOT checked for non-standalone objectives** (they roll up to outcome)
- **Objective-level coverage IS checked for standalone objectives** (they must be independent and complete)

## Rule 4: Standalone Objective Designation

**Standalone objectives must have their own complete P+I+A coverage**; non-standalone objectives feed into outcome-level coverage:

**Standalone Objective**:

- Independent learning unit (e-learning, self-paced module)
- Must have its own complete coverage: Passive + Interactive + Assessment
- Learner can complete it independently without other objectives
- May be scoped as its own publication
- **Coverage is validated at the objective level**

**Non-Standalone Objective**:

- Authored separately but rolls into parent outcome for delivery
- Requires lecture content at objective level (included in outcome lecture)
- Shares interactive activities and assessment with other objectives at outcome level
- **Coverage is NOT validated at objective level** (only at outcome level)

## Rule 5: Delivery Strategy & Deliverable Alignment

**Last synced**: 2026-09-03
**Source**: docs/design/content-design-process.md Steps 3-7

> **Canonical rule name**: "Delivery Strategy & Deliverable Alignment". This rule has previously been cited as "Modality-Deliverable Alignment," "Offering strategy-deliverable alignment," and "Delivery strategy-task alignment." Those names are retired.

### Overview

**Delivery strategy** determines the modality (e-learning, classroom, blended) used to teach a skill. It is the first link in the derivation chain, and this rule validates that the chain holds all the way to deliverables:

- Delivery strategy → offerings (per audience/role)
- Offerings → publications (scoped by design hierarchy)
- Publications → activities
- Activities + supporting assets → deliverables

A design that names a delivery strategy but has no offerings, or publications that serve no offering, or deliverables that trace to no activity, fails this rule.

<!-- SYNC NOTE: Part A below intentionally duplicates the six-factor decision framework in
     docs/design/content-design-process.md Step 3. Per docs/repo-info/KEEPING_DOCS_IN_SYNC.md,
     .claude/rules/ is the primary source consumed by Claude Code skills, while docs/ carries
     the human-facing narrative. Both copies must be updated together. Do not collapse one
     into the other: the git hook's docs-to-rules sync validation exists to police exactly
     this pair. -->

### Part A: Decision Framework for Choosing Delivery Strategy

Evaluate these six factors to determine the appropriate delivery strategy:

#### 1. Performance Type & Complexity

What type of performance is required?

- **Knowledge/Recognition** (identify components, recall procedures)
  → Recommend: E-learning

- **Procedural Performance** (follow step-by-step process)
  → Recommend: E-learning with virtual labs or simulations; ILT if physical equipment essential

- **Physical Performance** (hands-on assembly, manipulation, tactile feedback)
  → Recommend: ILT or Blended (e-learning theory + classroom hands-on)

- **Complex Troubleshooting** (diagnose failures, recommend solutions)
  → Recommend: Blended or ILT (instructor coaching essential for complex reasoning)

#### 2. Instructor Involvement & Coaching

Is instructor coaching, facilitation, feedback, or mentoring required?

- **High Instructor Involvement** (real-time feedback, complex reasoning, individual coaching)
  → Recommend: ILT or Blended
  → Examples: Advanced troubleshooting, certification labs, new technician onboarding

- **Moderate Instructor Involvement** (setup, facilitation, Q&A, guided practice)
  → Recommend: Blended (e-learning + synchronous support)
  → Examples: New feature training, hands-on labs with instructor available

- **Low Instructor Involvement** (self-paced, embedded guidance, peer support)
  → Recommend: E-learning with guided practice and embedded videos
  → Examples: Reference guides, foundational knowledge, compliance training

#### 3. Audience Scale & Distribution

What is the audience size and geography?

- **Large, Geographically Distributed** (50+ learners, multiple locations)
  → Recommend: E-learning (most cost-effective and accessible)
  → Considerations: Travel costs, scheduling logistics, scale economics

- **Moderate, Regional** (20-50 learners, regional clusters)
  → Recommend: Blended (e-learning self-paced + periodic classroom sessions)
  → Considerations: Combine local clusters for cost-effective ILT days

- **Small, Co-located** (Under 20 learners, same location)
  → Recommend: ILT or Blended (practical to gather for hands-on training)
  → Considerations: Feasibility of classroom space and instructor time

#### 4. Speed & Accessibility

How quickly do learners need the skills? What access patterns matter?

- **High Speed Needed** (urgent skill gap, just-in-time training)
  → Recommend: E-learning (on-demand, immediate access, no scheduling delays)
  → Examples: Urgent product updates, emergency procedures, new field deployment

- **Moderate Speed** (training needed within weeks)
  → Recommend: Blended or ILT (allows development time)
  → Examples: New product training, feature updates

- **Later/On-Demand** (training built for ongoing reference)
  → Recommend: E-learning (evergreen, always available)
  → Examples: Reference guides, certification prep

#### 5. Hardware & Environment Requirements

What equipment or environment does learning require?

- **Physical Equipment Required** (hands-on equipment, lab setup, tactile feedback)
  → Recommend: ILT or Blended (access to physical equipment)
  → Considerations: Can equipment be accessed remotely? VR simulation viable?

- **Can Simulate with Software** (virtual labs, simulations, digital replacements)
  → Recommend: E-learning with simulations
  → Examples: Circuit simulation, hydraulic system modeling, virtual field scenarios

- **No Special Equipment Needed** (knowledge, procedures, concepts)
  → Recommend: E-learning
  → Examples: Compliance training, conceptual knowledge, soft skills

#### 6. Content Complexity

How complex is the learning content? How much practice and feedback is needed?

- **Simple** (straightforward content, one-off learning)
  → Recommend: Can use any delivery strategy; consider learner preference
  → Examples: Brief policy updates, simple procedures

- **Moderate** (some interactive elements, some practice needed)
  → Recommend: E-learning with interactive practice and embedded feedback
  → Examples: Standard procedures, feature training, troubleshooting basics

- **Complex** (requires extensive practice, scenario-based learning, real-time coaching)
  → Recommend: Blended or ILT (scaffolded practice + instructor feedback)
  → Examples: Complex troubleshooting, system design, leadership development

### Part B: Delivery Strategy Selection

Based on the six-factor evaluation above, select the delivery strategy:

- **E-Learning**: For largely theoretical, self-paced content with low hardware requirements and high accessibility needs
- **Classroom (ILT)**: For hands-on, equipment-dependent content, or high-coaching scenarios
- **Blended**: For complex content requiring both e-learning and hands-on practice; balances cost and effectiveness

A recommendation without a recorded rationale fails this rule.

### Part C: Offering Alignment

Every delivery strategy must produce at least one offering, and **every audience/role captured at intake must be served by at least one offering**.

| Delivery Strategy | Expected Offerings |
| --- | --- |
| **E-Learning** | A self-paced online course enrolled in via the LMS |
| **Classroom (ILT)** | An instructor-led course with a scheduled session |
| **Blended** | A self-paced online course **plus** a classroom or regional practicum |

Each offering records: name, target audience/role, delivery strategy, outcome coverage, supporting materials.

### Part D: Publication Alignment

Every offering must be supported by at least one publication, and every publication must feed at least one offering. Publication scope must be consistent with the design hierarchy.

| Delivery Strategy | Required Publications | Publication Types |
| --- | --- | --- |
| **E-Learning** | SCORM module | `scorm1.2` |
| **Classroom (ILT)** | Instructor presentation, lab manual, practical assessment | `revealjs` or `pptx`, `print`, `print` |
| **Blended** | All of the above, plus a learner handout | `scorm1.2`, `revealjs`/`pptx`, `print` |

Each publication records: name, audience/role, scope, publication type, and the offering(s) it feeds.

**Scoping by design hierarchy**:

- One publication per skill: the default
- One publication per outcome: when outcomes are delivered separately or roles need different subsets
- One publication per standalone objective: required, since a standalone objective is independently completable

### Part E: Deliverable Alignment

Every activity must map to at least one deliverable, and every deliverable must trace to an activity or be recorded as a supporting asset.

**content deliverables**: see Rule 6 for the file-level requirements.

**Supporting assets** must be captured in the deliverables list with what they are, which deliverable depends on them, and who owns them. Categories: VMs and test environments, project files, lab start/finish files, externally produced media, supporting documentation.

A lab activity that depends on a VM which is absent from the deliverables list fails this rule: the lab is not deliverable.

### Example: Large Audience, Complex Troubleshooting Skill

**Factors Evaluated**:

- Performance Type: Complex Troubleshooting → Suggests Blended/ILT
- Instructor Involvement: Moderate → Suggests Blended
- Audience Scale: 100+ technicians, geographically distributed → Suggests E-learning
- Speed: 3-month rollout acceptable → Allows time for development
- Hardware: Field equipment available at service centers → Suggests ILT/Blended
- Complexity: High (multiple scenarios, decision trees) → Suggests Blended/ILT

**Analysis**: Multiple factors point to Blended

**Delivery Strategy: BLENDED**

- E-learning core: Knowledge foundation, scenario walkthrough, embedded practice
- Regional classroom practicum: Hands-on practice with local field equipment
- Rationale: Reduces travel costs while enabling hands-on practice

**Offerings**:

- Field Technician: blended offering (self-paced course + regional practicum)
- Application Engineer: e-learning offering (theory only, as reference)

**Publications**:

- SCORM module (`scorm1.2`), Technician + Engineer, all outcomes: theory and scenario practice
- Instructor presentation (`revealjs`), Technician, all outcomes: classroom materials
- Lab manual (`print`), Technician, all outcomes: regional practicum activities
- Handout (`print`), Technician, all outcomes: job aids and reference

**Supporting Assets**:

- Hydraulic simulator VM (required by the e-learning interactive activity)
- Field equipment checklist for service centers (required by the practicum)

## Rule 6: Deliverable File Completeness

For each outcome and objective, the corresponding markdown files must be planned.

**Structure**:

- Outcome folders use the outcome title in kebab-case (e.g., `analyze-hydraulic-components/`)
- Each objective gets a subfolder (`objective-01`, `objective-02`, ...)

**Objective level**: every objective needs:

- `[outcome-folder]/objective-##/lecture.md`: **always**
- `[outcome-folder]/objective-##/knowledge-check.md`: **always**
- `[outcome-folder]/objective-##/quiz-questions.md`: **always** (feeds the outcome quiz pool)
- `[outcome-folder]/objective-##/lab.md`: **only if the objective is standalone**

**Outcome level**: every outcome needs:

- `[outcome-folder]/outcome-##-lecture.md`: **always**
- `[outcome-folder]/outcome-##-quiz.md`: **always**
- `[outcome-folder]/outcome-##-lab.md`: when the outcome has non-standalone objectives
- `[outcome-folder]/outcome-##-presentation.md`: classroom or blended delivery
- `[outcome-folder]/outcome-##-practical.md`: when assessment is hands-on
- `[outcome-folder]/outcome-##-handout.md`: blended delivery or planned learner reference

**Scope note**: presentations and practicals are **outcome-level**. A skill-level `skill-##-practical.md` is optional and only for a deliberate cross-outcome capstone; it does not satisfy any outcome's assessment coverage on its own.

**Supporting assets** live under `assets/` with a required `assets/README.md` inventory.

All file names follow the naming convention and outcome-title folder structure. See [docs/design/file-mapping-guide.md](../../docs/design/file-mapping-guide.md).

## Rule 7: Publication-to-Activity Derivation (Not Yet Enforced)

<!-- TBD: The publication → activity derivation rules are not yet specified. This rule slot
     exists so the mapping can be encoded here once the business logic is defined. Claude
     Code does NOT currently enforce this rule. Until it is specified, activities are planned
     by the design team and checked against Rule 3 coverage only. -->

**Status**: Placeholder. **Claude Code does not enforce this rule yet.**

**Intent**: Activities should be derivable from the publication set via business logic; a `scorm1.2` publication requires a different activity mix than a `print` lab manual. When those mapping rules are specified, they belong here, and this rule will validate that the planned activities match what the publications require.

**Until then**: activity planning is a design-team judgment, validated against Rule 3 (outcome-level coverage) only.

---

## Validation Workflow

### Step 1: Self-Check

Design steps 1-2 (outcomes and objectives):

- ☐ All outcomes have clear, concise titles
- ☐ All outcomes have complete ABCD elements
- ☐ All objectives tied to outcomes
- ☐ Standalone designations documented

Design step 3 (delivery strategy):

- ☐ Delivery strategy selected with recorded rationale

Design step 4 (offerings):

- ☐ Offerings defined, each with audience/role and outcome coverage
- ☐ Every audience/role from intake is served by at least one offering

Design step 5 (publications):

- ☐ Publications defined with name, audience/role, scope, and publication type
- ☐ Every offering is supported by at least one publication
- ☐ Publication scope is consistent with the design hierarchy
- ☐ Standalone objectives that need their own publication have one

Design step 6 (activities):

- ☐ **Outcome-level** coverage complete (Passive + Interactive + Assessment per outcome)
- ☐ **Standalone objectives** have individual P+I+A coverage
- ☐ **Non-standalone objectives** marked and rolled into outcomes

Design step 7 (deliverables):

- ☐ Every activity maps to at least one deliverable
- ☐ Objective folders contain `lecture.md`, `knowledge-check.md`, `quiz-questions.md`
- ☐ `lab.md` at objective level only for standalone objectives
- ☐ Outcome-level files present per Rule 6
- ☐ Supporting assets (VMs, project files) captured with owners in `assets/README.md`

Design steps 8-9 (validation and load):

- ☐ Generated design reviewed by the team; corrections applied
- ☐ Design loaded into the content database
- ☐ *(Transitional)* CDD Workbook entry complete, required until the content database is live

### Step 2: Claude Code Validation

Ask Claude Code: "Validate my content design"

Result:

- ✅ **Passes** - Ready to load into the content database
- ⚠️ **Warnings** - Functional but has minor issues
- ❌ **Blockers** - Must fix before proceeding

### Step 3: Escalation

Complex questions escalate to Aba Azeem (<aba.azeem@rockwellautomation.com>)

### Step 4: Approval & Development

Once approved, the design is loaded into the content database, which creates the LeGIT project files and exports deliverables to DevOps. Content development then proceeds in the generated shells.

---

**Escalation Contact**: Aba Azeem (<aba.azeem@rockwellautomation.com>)
**Response Time**: 1-2 business days
