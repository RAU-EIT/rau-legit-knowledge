# SME Workflows for LeGIT Content Development

This guide describes the standard workflows for subject matter experts (SMEs) developing content in LeGIT.

## Content Development Lifecycle

LeGIT content development follows a design-first workflow:

1. **Design Phase** — SME completes instructional design
2. **Development Phase** — SME writes and structures content
3. **Review Phase** — Content review and validation
4. **Publication Phase** — Build and deliver to end users

## Design Phase Workflow

The design phase establishes learning outcomes, objectives, and modality decisions **before** any content is written.

**Deliverable:** Content Design Document (CDD)

### Step 1: Define ABCD Outcomes

Using the ABCD method, define measurable learning outcomes:
- **Audience** — Who will take this training
- **Behavior** — What they will do (measurable verb)
- **Condition** — Under what circumstances
- **Degree** — How well (metric for success)

### Step 2: Design Objectives

Break each outcome into 3-5 specific learning objectives.

### Step 3: Make Modality Decision

Determine **how** this content will be delivered:
- **E-Learning (SCORM)** — Self-paced, online, interactive
- **Classroom (ILT)** — Instructor-led, hands-on labs
- **Blended** — Mix of online and classroom

### Step 4: Define Deliverables

Based on modality, determine what content packages are needed:

| Modality | Required Deliverables |
|----------|------------------------|
| E-Learning | Lecture + Quiz + Knowledge checks |
| Classroom | Presentation + Lab manual + Practicals |
| Blended | Presentation + Lecture + Lab manual + Quiz |

### Step 5: Validate Coverage

Every outcome must have:
- **Passive:** Lecture content
- **Interactive:** Lab/hands-on practice
- **Assessment:** Quiz/assessment

### Step 6: Complete CDD Workbook

Document the design in the CDD Workbook:
- Outcomes and objectives
- Modality decision with rationale
- Coverage matrix
- Deliverables list

### Step 7: Validation

Have the CDD reviewed by:
- Instructional designer (Aba Azeem)
- Subject matter expert (peer review)
- Stakeholder (management approval)

## Development Phase Workflow

After design is approved, content is authored in markdown.

### File Organization

```
skills/
├── skill-123/
│   ├── objective-01-lecture.md
│   ├── objective-02-lecture.md
│   ├── outcome-01-lecture.md
│   ├── outcome-01-lab.md
│   ├── outcome-01-quiz.md
│   └── media/
```

### Writing Objectives

Write at the **objective level** for reusability:
- Each objective file focuses on one learning objective
- Files are small (3-8 pages) for easier maintenance
- Content can be reused across multiple courses

### Composing at Outcome Level

For delivery, outcomes **aggregate** objectives using `!include()`:

```markdown
!include(./objective-01-lecture.md)
!include(./objective-02-lecture.md)
```

### Using Content Blocks

Use RAU markdown blocks to structure content:
- **Accordion** — Expand/collapse sections
- **Steps** — Procedure walkthrough
- **Quiz** — Knowledge assessment
- **Video** — Embed multimedia
- **Alerts** — Important notes

### Media Management

Store media files in a `media/` folder and use relative paths:

```markdown
![Network architecture](./media/network-diagram.svg)
```

## Review Phase Workflow

After drafting, content goes through review cycles.

### Self-Review Checklist

Before submitting for review:
- [ ] All outcomes have lecture + lab + quiz
- [ ] YAML frontmatter is complete
- [ ] All images have alt text
- [ ] All links are relative
- [ ] No linter warnings
- [ ] Content is in sentence case

### Peer Review

Content is reviewed by:
- Peer SME (technical accuracy)
- Instructional designer (pedagogical soundness)
- Quality assurance (format and style compliance)

## Publication Phase Workflow

Once review is complete, content is built for delivery.

### Build Process

```bash
make pdf          # Build PDF
make presentation # Build HTML presentation
make scorm        # Build SCORM module
```

### Quality Assurance Testing

Test the built output:
- [ ] All content renders correctly
- [ ] All images load properly
- [ ] All links work
- [ ] Interactive elements function
- [ ] PDFs print correctly
- [ ] SCORM modules launch in LMS

## Escalation Path

For questions or issues:

1. **First:** Check the LeGIT documentation in CLAUDE.md
2. **Design questions:** Contact Aba Azeem (instructional designer)
3. **Content questions:** Ask in the RAU learning team Slack channel
4. **Technical issues:** File an issue in the repository

---

For detailed information on specific topics, see:
- [Content Design Process](./content-design-process.md)
- [File Mapping Guide](./file-mapping-guide.md)
- [Best Practices](./best-practices.md)
