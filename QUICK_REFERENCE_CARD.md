# RAU LeGIT Knowledge Base - New Vision

## One-Page Overview

### The Vision: Two Complementary Tracks

```
INPUT:                          TOOLS:                      OUTPUT:

Skills + Audiences    ────→    /design-training     ────→   design.json
                                (Interactive design)          + validation
                                
                                        ↓
                                  
                     Source Materials (PPTs, ────→   /develop-training   ────→   Markdown Files
                      Docs, Expertise)               (Interactive authoring)        + validation
                                        ↓
                                  
                                   Build System     ────→   PDF, SCORM, 
                                   (Pandoc, Lua)            HTML, Video
```

---

## Content Design Track

**Who**: Instructional Designers, Product Managers, SMEs  
**Input**: "What skills need training and for whom?"  
**Output**: Design decisions (outcomes, objectives, modalities) → design.json

**The 7-Step Design Process**:
1. Define Learning Outcomes (ABCD)
2. Define Learning Objectives (2-5 per outcome)
3. Determine Publication Strategy (e-learning/classroom/blended)
4. Map to Required Deliverables
5. Plan Activity Coverage (Passive + Interactive + Assessment)
6. Enter Design into CDD Workbook
7. Validate Your Design

**Validation**: 6 Rules Checked
- ABCD completeness
- Objective alignment
- Coverage completeness
- Standalone designation
- Modality-deliverable alignment
- File mapping completeness

---

## Content Development Track

**Who**: SMEs, Technical Writers, Content Developers  
**Input**: "Translate your knowledge into training content"  
**Output**: Markdown files ready to publish

**Content Types**:
- **Lectures** (800-1,200 words) - Explain concepts with examples
- **Knowledge Checks** (50-100 words) - Quick ungraded questions
- **Labs** (1,500-2,500 words) - Hands-on guided practice
- **Quiz Questions** (3-5 per objective) - Graded assessment
- **Content Blocks** - Alerts, accordions, hotspots, steps, etc.

**Standards**: Markdown + YAML + LeGIT Blocks + Build-Ready Format

---

## The Two Claude Code Skills

### Skill 1: `/design-training`
```
START: "What skills need training?"
  ↓
ANSWERS: Interactive questionnaire
  ├─ Outcomes (ABCD)
  ├─ Objectives per outcome
  ├─ Modality selection
  └─ Activity coverage
  ↓
VALIDATION: Check 6 rules
  ↓
OUTPUT: design.json + summary report
```

**When to Use**: Starting a new training project  
**Result**: Structured design, ready for authoring

---

### Skill 2: `/develop-training`
```
START: "What are you translating?" (PPT, docs, expertise)
  ↓
LOAD: design.json (what to build)
  ↓
AUTHOR: Interactive guidance
  ├─ Lectures (translate from source)
  ├─ Knowledge checks (validate understanding)
  ├─ Labs (structure hands-on practice)
  └─ Quiz questions (assessment)
  ↓
ADD LEGIT: Blocks, formatting, structure
  ↓
VALIDATE: Markdown standards
  ↓
OUTPUT: Markdown files + validation report
```

**When to Use**: Ready to author content  
**Result**: Formatted, validated, ready to build

---

## Why This Approach Works

| Traditional | RAU LeGIT Approach |
|---|---|
| Design and dev mixed together | Design FIRST, dev SECOND |
| Outcomes unclear | ABCD clarity enforced |
| Modality decided late | Modality drives deliverables |
| Manual authoring | Guided, validated authoring |
| Inconsistent standards | Automated validation |
| High error rate | Errors caught early |

---

## Implementation Timeline

**Phase 1** (Done): Reorganize knowledge base  
**Phase 2** (Done): Sketch skill specs  
**Phase 3** (Next): Implement skills
- Weeks 1-4: Build `/design-training`
- Weeks 5-8: Build `/develop-training`
- Weeks 9+: Refine, integrate, scale

**Total**: 6-8 weeks to full automation

---

## Benefits

### For Instructional Designers
- ✅ Guided design process (no more guessing)
- ✅ Validation prevents errors (catch problems early)
- ✅ Structured output (design.json) is reusable
- ✅ Clear handoff to developers

### For Content Developers
- ✅ Clear specs (outcomes + objectives define scope)
- ✅ Guided authoring (interactive help)
- ✅ Validated formatting (standards enforced)
- ✅ Templates ready (lecture, lab, quiz templates)

### For the Organization
- ✅ Consistent quality (standards enforced everywhere)
- ✅ Faster time-to-publication (automation reduces manual work)
- ✅ Reusable designs (design.json stored, versioned)
- ✅ Scalable (same process works for 1 skill or 100)

---

## Getting Started

### As an Instructional Designer
1. Read: [Content Design Track](./CLAUDE_REORGANIZED.md#content-design-guide)
2. Use: `/design-training` (coming in Phase 3)
3. Output: design.json ready for authoring

### As a Content Developer
1. Read: [Content Development Track](./CLAUDE_REORGANIZED.md#content-development-guide)
2. Use: `/develop-training` (coming in Phase 3)
3. Input: Source materials (PPT, docs)
4. Output: Markdown files ready to build

### As a Project Manager
1. Review: This one-pager + `PHASE_1_2_SUMMARY.md`
2. Decide: Proceed with Phase 3 implementation?
3. Plan: 6-8 week implementation timeline

---

## Key Files

| File | Purpose |
|------|---------|
| `CLAUDE_REORGANIZED.md` | Reorganized knowledge base (Design + Development tracks) |
| `CLAUDE_CODE_SKILLS_SPEC.md` | Complete specifications for both skills |
| `PHASE_1_2_SUMMARY.md` | Detailed summary of Phase 1-2 completion |
| `QUICK_REFERENCE_CARD.md` | This one-pager |

---

## Questions?

**About Design Process?**  
→ See [Content Design Guide](./CLAUDE_REORGANIZED.md#content-design-guide)

**About Development Standards?**  
→ See [Content Development Guide](./CLAUDE_REORGANIZED.md#content-development-guide)

**About the Skills Implementation?**  
→ See [Claude Code Skills Spec](./CLAUDE_CODE_SKILLS_SPEC.md)

**About the Overall Plan?**  
→ See [Phase 1-2 Summary](./PHASE_1_2_SUMMARY.md)

---

**Version**: 1.0  
**Date**: 2026-08-20  
**Status**: Complete - Ready for Leadership Review
