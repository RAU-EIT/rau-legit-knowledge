---
name: content-design-validation
description: Validation rules for learning content design
type: soft-guidance
---

# Content Design Validation Rules

Claude Code checks learning designs against these rules.

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
- **Coverage is validated at the objective level**

**Non-Standalone Objective**:
- Authored separately but rolls into parent outcome for delivery
- Requires lecture content at objective level (included in outcome lecture)
- Shares interactive activities and assessment with other objectives at outcome level
- **Coverage is NOT validated at objective level** (only at outcome level)

## Rule 5: Modality-Deliverable Alignment & Selection

**Last synced**: 2026-08-20  
**Source**: docs/design/content-design-process.md Step 3

### Part A: Decision Framework for Choosing Modality

Evaluate these six factors to determine appropriate modality:

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
  → Recommend: Can use any modality; consider learner preference
  → Examples: Brief policy updates, simple procedures
  
- **Moderate** (some interactive elements, some practice needed)
  → Recommend: E-learning with interactive practice and embedded feedback
  → Examples: Standard procedures, feature training, troubleshooting basics
  
- **Complex** (requires extensive practice, scenario-based learning, real-time coaching)
  → Recommend: Blended or ILT (scaffolded practice + instructor feedback)
  → Examples: Complex troubleshooting, system design, leadership development

### Part B: Modality Selection

Based on the six-factor evaluation above, select the best modality:

- **E-Learning**: For largely theoretical, self-paced content with low hardware requirements and high accessibility needs
- **Classroom (ILT)**: For hands-on, equipment-dependent content, or high-coaching scenarios
- **Blended**: For complex content requiring both e-learning and hands-on practice; balances cost and effectiveness

### Part C: Deliverable Alignment

Ensure selected modality produces required output formats with correct content components:

**E-Learning Modality**:
- Output Format: SCORM 1.2 module (1 file)
- Module Contents: Lectures + Knowledge Checks + Labs + Quiz Questions
- All content aggregated into single SCORM package

**Classroom Modality**:
- Output Formats: Presentations (RevealJS/PowerPoint) + Labs + Practicals + Handouts
- Each format is separate, instructor-facilitated delivery

**Blended Modality**:
- Output Formats: SCORM module + Presentations + Labs + Handouts
- Combines e-learning and classroom formats

### Example: Large Audience, Complex Troubleshooting Skill

**Factors Evaluated**:
- Performance Type: Complex Troubleshooting → Suggests Blended/ILT
- Instructor Involvement: Moderate → Suggests Blended
- Audience Scale: 100+ technicians, geographically distributed → Suggests E-learning
- Speed: 3-month rollout acceptable → Allows time for development
- Hardware: Field equipment available at service centers → Suggests ILT/Blended
- Complexity: High (multiple scenarios, decision trees) → Suggests Blended/ILT

**Analysis**: Multiple factors point to Blended

**Recommendation: BLENDED**
- E-learning core: Knowledge foundation, scenario walkthrough, embedded practice
- Regional classroom practicum: Hands-on practice with local field equipment
- Benefit: Reduces travel costs while enabling hands-on practice

**Output Formats**:
- SCORM module: Theory and scenario-based practice
- PowerPoint presentations: Classroom instructor materials
- Lab exercises: Regional practicum activities
- Handouts: Job aids and reference materials

## Rule 6: File Mapping Completeness

For each outcome/objective, corresponding markdown files must be planned:
- Outcome folders use outcome title in kebab-case (e.g., `analyze-hydraulic-components/`)
- Every objective needs: `[outcome-folder]/objective-##/lecture.md`
- Every outcome needs: `[outcome-folder]/outcome-##-lecture.md`
- Standalone objectives need: `[outcome-folder]/objective-##/lab.md` and `[outcome-folder]/objective-##/quiz-questions.md`
- All file names follow naming convention and outcome title folder structure

## Validation Workflow

### Step 1: Self-Check
- ☐ All outcomes have clear, concise titles
- ☐ All outcomes have complete ABCD elements
- ☐ All objectives tied to outcomes
- ☐ **Outcome-level** coverage complete (Passive + Interactive + Assessment per outcome)
- ☐ **Standalone objectives** have individual P+I+A coverage
- ☐ **Non-standalone objectives** marked and rolled into outcomes
- ☐ Standalone designations documented
- ☐ Publication modality selected with rationale
- ☐ Deliverables list complete
- ☐ File mapping complete with outcome title folder structure
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
