# Pipeline Shakeout Findings

**Date**: 2026-09-04
**Run by**: Peter Francis, with Claude Code
**Question asked**: can a real stakeholder request be driven through this knowledge base into
designed, authored, and built training?

**Answer**: yes, and the result builds. Four defects had to be fixed before the knowledge base
could be loaded at all, and the file contract it prescribes is not the one the build system
consumes, but the designed content assembled into a working publication once authored against the
template instead.

---

## What was run

| | |
| --- | --- |
| Stakeholder request | Process Safety Training, customer-focused program built on Process Safebook 1 (SAFEBK-RM003), plus a supplied role matrix of 31 skill statements against 4 roles |
| Content repo | [RAU-EIT/process-safety-training-claude-test](https://github.com/RAU-EIT/process-safety-training-claude-test), created from `content-starter-pack` |
| Knowledge base | This repo, mounted as a submodule at `.claude/shared-knowledge` |
| Scope designed | The shared core of the role matrix: statements 1, 2, and 27, as one skill with 3 outcomes and 12 objectives |
| Scope authored | Outcome 1 in full (11 files); outcomes 2 and 3 scaffolded |
| Publications designed | 8, of which 3 have build files |
| Skill packaging fix | Merged to `main` in this repo (PR #8) |

Artifacts to read in order: `design/00-intake.md`, `design/01-design.md`,
`design/01-design.json`, `design/02-deliverable-manifest.md`, then
`outcomes/determine-iec-61511-applicability/`.

---

## Findings summary

| # | Finding | Severity | Blocked progress |
| --- | --- | --- | --- |
| 1 | Skills are not in a layout Claude Code can load | Blocker | Yes, fixed |
| 2 | User-facing skills never reference their own implementation files | Blocker | Yes, fixed |
| 3 | The documented submodule wiring cannot work, and does not cover skills | Blocker | Yes, worked around |
| 4 | No intake step exists between a stakeholder request and design Step 1 | Blocker | Yes, invented one |
| 5 | The prescribed file contract is not what the build system consumes | Blocker | Yes, abandoned the contract |
| 6 | `!include` is documented as the assembly mechanism but does not exist | High | No |
| 7 | `docType` values in the rules do not match the template | High | No |
| 8 | Frontmatter uses `chunk:`, not the required `skill:` block | High | No |
| 9 | The ABCD behaviour whitelist rejects valid RAU skill-statement verbs | High | No |
| 10 | `workflow.md` and `engine.md` implement the same rules differently | High | No |
| 11 | Rule 1 cannot fail on a missing ABCD element | High | No |
| 12 | Rule 4 is a no-op but is advertised as enforced | High | No |
| 13 | The delivery strategy tie-break silently favours e-learning | Medium | No |
| 14 | Rule 5 Part D contradicts the template on publishing objectives | Medium | No |
| 15 | The design JSON has two incompatible schemas | Medium | No |
| 16 | `develop-training` generates quiz paths that contradict its own manifest | Medium | No |
| 17 | `getPublicationTypes` emits display labels into a machine-read field | Medium | No |
| 18 | The Step 8 human review gate is missing from the implementation | Medium | No |
| 19 | The design input contract cannot represent an audience and role matrix | Medium | No |
| 20 | Nothing expresses performs versus awareness depth | Medium | No |
| 21 | `legit-blocks.md` documents tab structure for accordion but not for steps | Low | No, caused one authoring error |
| 22 | Build file schema is documented nowhere, and two versions are in the wild | Low | No |
| 23 | `rau-start-here` is not a template repo, but docs point SMEs to it | Low | No |
| 24 | `.claude/settings.json` is 0 bytes | Low | No |
| 25 | The docs-sync hook is advertised as automatic but is not installed | Low | No |
| 26 | Two skill files declare the same `name` | Low | No |
| 27 | Prose typo in `workflow.md` | Low | No |
| 28 | `linkedBuilds` does not fire | High | Yes, worked around |
| 29 | A build reports success while producing no output | High | Yes, cost one run |
| 30 | The default build task only ever builds the root `build.yaml` | Medium | Yes, cost one run |
| 31 | Every build run wipes the entire output directory | High | Yes, combined with 28 |

Findings 28 to 31 are defects in the RAU VSCode extension and the template rather than in this
knowledge base, but they belong here because they cost two of the three build attempts and any
SME will hit them.

---

## Blockers, and what was done about them

### 1. Skills are not in a layout Claude Code can load

**Attempted**: invoke `/design-training`.
**Happened**: nothing. Claude Code discovers skills only as `.claude/skills/<name>/SKILL.md`, and
this repo stored them as flat files (`design-training.md`, `develop-training.md`, ...). No skill
in this repo has ever been loadable.
**Fix**: merged to `main` in PR #8. `git mv` into skill directories. Registration
took effect immediately, with no session restart.
**Not fixed**: `build-training-engine.md` and the five `sync-*` files, deliberately left out of
scope. See finding 26 before moving them.

### 2. User-facing skills never reference their own implementation files

**Attempted**: run the design process after registration.
**Happened**: `design-training.md` never mentioned `design-training-workflow.md` (1,157 lines, the
actual 9-step state machine) or `design-training-engine.md` (1,394 lines, the recommendation and
validation logic). A grep for cross-references returned only self-references. The skill would have
run from a 252-line overview while 2,551 lines of implementation sat unreachable beside it.
**Fix**: each `SKILL.md` now opens with an explicit instruction to read its supporting files
first. Verified working: the skill directed this run into `workflow.md` and `engine.md`.

### 3. The documented submodule wiring cannot work, and does not cover skills

**Attempted**: follow `README.md` Option 1, which says
`ln -s shared-knowledge/.claude/rules/* rules/`.
**Happened**: `core.symlinks=false` in `C:/Program Files/Git/etc/gitconfig`, system-wide on RAU
Windows machines. Symlinks are unavailable, so the instruction cannot be followed by anyone
following the README on a standard build.

Worse, the README says nothing about **skills**, which is the part that matters. Docs and rules
load fine through `@` imports in the content repo's `CLAUDE.md`. Skills have no import mechanism
at all: Claude Code looks only at the project root, never inside a submodule.

**Worked around**: `bootstrap-claude.sh` in the content repo copies the two skill directories out
of the submodule into `.claude/skills/`, rewriting their `../../../docs/` links to point back into
the submodule. `.claude/skills/` is gitignored, so the submodule stays authoritative. Re-run after
`git submodule update --remote`.

**Recommended proper fix**: package this repo as a **Claude Code plugin**. Skills, rules, and docs
then install as one unit from the git repo, no copy step, no symlinks, no `@` import list to keep
in sync, and updates arrive through the normal plugin mechanism. The submodule would become
unnecessary for the Claude Code use case.

**Also update `README.md`**: Option 1 currently gives an instruction that fails silently.

### 4. No intake step exists between a stakeholder request and design Step 1

**Attempted**: begin at design Step 1 with the stakeholder request in hand.
**Happened**: `content-design-process.md` lists "skill statements (already defined), target
audiences and roles, stakeholder requirements" as *inputs* to Step 1. Steps 4 and 5 depend on a
complete role list "from intake", and Rule 5 Part C fails a design if any intake role goes
unserved. But nothing defines what intake is, who does it, or what it produces.

This is not a small gap. On this request, intake is where the real work was:

- The request pre-specified ILT while the evidence supported blended (see finding 13)
- 31 statements across 4 roles do not fit the requested one-week ILT, a conflict that has to be
  raised with the stakeholder rather than designed around
- Four of six factors in the delivery strategy framework were answerable from the request; two
  were not, and one of those inverts the recommendation
- "Level1/2" appears in the request and is not a term in the glossary
- The request also asks for updated reference documentation, which is a publication that derives
  from no learning outcome and therefore cannot flow through the design chain at all

**Worked around**: invented `design/00-intake.md`, structured as: verbatim request, derived
project frame, skill statements and roles, audiences, six-factor evidence with each factor marked
answered or not, conflicts, assumptions taken with their consequences, and open questions for the
stakeholder.

**Recommended fix**: promote this to a documented Step 0 with a template. The discipline worth
keeping is forcing every unknown into either "assumed, assumption stated" or "must ask the
stakeholder", so that no invented fact enters the design silently.

### 5. The prescribed file contract is not what the build system consumes

**Attempted**: author to `docs/design/file-mapping-guide.md`.
**Happened**: the `content-starter-pack` template, which is the current LeGIT project template and
is actively maintained (June 2026 dates throughout), uses a different structure entirely.

| `file-mapping-guide.md` prescribes | The template uses |
| --- | --- |
| `skills/<skill>/` top-level directory | No skill directory. Content is organized by outcome under `outcomes/` |
| `<outcome>/objective-##/lecture.md` | `outcomes/<outcome>/objective-##.md`, a flat fragment with no frontmatter |
| `objective-##/knowledge-check.md` | Knowledge checks are content blocks inside the passive content |
| `objective-##/quiz-questions.md` per objective | One `quiz-questions.md` per outcome, sectioned by objective |
| `outcome-##-lecture.md`, `-lab.md`, `-quiz.md`, `-presentation.md`, `-handout.md`, `-practical.md` | `presentation.md`, `lab.md`, `scorm-section.md`, `scorm-quiz.md`, `practical.md`, `handout.md` |
| `assets/` with a `README.md` inventory | No `assets/` convention exists |
| Build config not described | `courses/<CODE>/builds/*.build.yaml`, multiple named files with `linkedBuilds` |

The template's readme states the same conceptual model this knowledge base teaches (Skill >
Outcome > Objective > Activity, with a passive, an interactive, and a validation activity per
outcome), so the **concepts are aligned and only the file contract has drifted**.

**Worked around**: authored to the template layout, keeping objective-level granularity as
frontmatter-free fragment files assembled by ordered `files:` arrays in the build. This preserves
the knowledge base's authoring principle using the template's mechanism. Full mapping table in the
content repo's `design/02-deliverable-manifest.md`.

**Consequence for Rule 6**: it checks for file names that will never exist. The design satisfies
Rule 6's intent and fails its letter.

**Recommended fix**: rewrite `file-mapping-guide.md` against the template, and rewrite Rule 6 to
match. This is the largest single piece of remedial work the shakeout found. Until it is done,
Rule 6 will fail every real project and SMEs following the guide will produce a repo that does not
build.

---

## Content and rules that are out of date

### 6. `!include` does not exist

`file-mapping-guide.md` teaches an `!include` directive as the way outcome lectures aggregate
objective content. There is **not one occurrence of `!include`** anywhere in the template, in the
sample course, or in DevOpsTraining. Assembly is done by ordered `files:` arrays in build files.
The build's own comment confirms multi-file assembly is intended: "files array can have multiple
markdown files in a single presentation (title slides beyond 1st are ignored)".

### 7. `docType` values do not match

`legit-yaml.md` lists `print`, `revealjs`, `scorm` as the valid set, and `CLAUDE.md` lists
`print`, `revealjs`, `pptx`, `scorm1.2`, `presentation-video`. The template's content uses exactly
three values, counted across all its markdown: **`lab` (19), `scorm` (11), `revealjs` (9)**.
`docType: print` appears nowhere. Every print-bound file (labs, practicals, handouts, question
banks) carries `docType: lab`.

Output format is set separately, by **`outputType`** in the build file, whose values are `print`,
`revealjs`, and `scorm1.2`. The rules conflate two distinct fields.

### 8. Frontmatter uses `chunk:`, not `skill:`

`legit-yaml.md` requires a `skill:` block with `id`, `revisionDate`, and `classification`, and
fails a file without all three. The template uses **`chunk:`** with those same three fields.
Publication-level metadata (`publisher`, `trainingType`, `id`, `revisionDate`, `classification`)
lives in the build file's `publication:` block and in course-level `*-frontcover.yaml` files,
neither of which the knowledge base documents.

`varsLocal` and `varsGlobal` are both real and correctly documented: `varsLocal` in print-bound
files, `varsGlobal` in course frontcover YAML.

---

## Implementation defects in the design skill

All of the following are reproducible against the three outcomes in
`design/01-design.md`.

### 9. The ABCD behaviour whitelist rejects valid RAU skill-statement verbs

Both files test the behaviour element against a fixed verb list. `workflow.md` allows analyze,
troubleshoot, configure, design, explain, demonstrate, identify. `engine.md` adds apply, create,
build, implement.

The verbs in this skill are **determine**, **map**, and **carry out**, taken directly from the
supplied skill statements, which were themselves written to the RAU formula. **All three outcomes
fail the automated check while satisfying ABCD completely.** Rewording to satisfy the regex would
corrupt the skill statements, so the design kept the correct verbs and the check was overridden by
hand.

A whitelist of eleven verbs cannot cover Bloom's taxonomy. Either drop the check, or replace it
with a check that a verb exists and is observable.

### 10. `workflow.md` and `engine.md` implement the same rules differently

Not a documentation duplication; two live implementations that disagree.

- **Audience check.** `workflow.md` requires the literal pattern `the` followed immediately by one
  of five job titles, so "the functional safety practitioner" fails. `engine.md` matches the word
  "engineer" anywhere and passes. Same outcome, opposite results.
- **Delivery strategy.** `workflow.md` uses if/else rules whose first branch sends anything
  needing an instructor or hardware to blended, and which **can never return classroom at all**.
  `engine.md` uses a weighted six-factor score that can. The two files recommend different
  strategies for the same inputs.

Which file governs is undefined. This run followed `engine.md` because it is the more developed
implementation, and recorded the divergence.

### 11. Rule 1 cannot fail on a missing ABCD element

`extractABCD` substitutes the placeholders `unknown`, `perform action`, `standard conditions`, and
`as specified` when it cannot parse an element. `validateRule1` then tests only whether those
fields are truthy. They always are. A design whose audience could not be parsed passes Rule 1
with its audience recorded as "unknown".

### 12. Rule 4 is a no-op but is advertised as enforced

`validateRule4` is an empty `for` loop containing the comment "Will be validated when developing
(for now just note)", and unconditionally returns pass. Rule 7 is honestly labelled a
placeholder; Rule 4 is not, and it is listed among the six enforced rules in `CLAUDE.md`, in the
`README.md`, and in the skill's own output. The Rule 4 pass in this design is a human check.

### 13. The delivery strategy tie-break silently favours e-learning

Scoring this skill's six factors through `engine.md` gives **e-learning 8, blended 8,
classroom 7**. The function initializes the winner to e-learning and compares with strict
greater-than, so a tie resolves to e-learning with no indication that it was a tie.

More seriously, the scoring is highly sensitive to a factor nobody has answered. Setting audience
scale to moderate rather than large, which is entirely possible since the request gives no
headcount, returns **blended 11, classroom 7, e-learning 5**. A single unanswered intake question
inverts the recommendation, and the engine presents both results with equal confidence.

Suggested fixes: surface ties explicitly, refuse to recommend when a factor is unknown rather than
scoring a guess, and report sensitivity when the top two are within a point or two.

### 14. Rule 5 Part D contradicts the template on publishing objectives

Rule 5 Part D: "One publication per standalone objective: **required**, since a standalone
objective is independently completable." Rule 4 says a standalone objective "may be scoped as its
own publication".

The `content-starter-pack` readme states the opposite doctrine: "outcomes are the smallest unit of
training a learner would consume; though it is made up of smaller pieces, we do not publish
objectives or activities on their own outside of the outcome that 'wraps them up'."

This design has one standalone objective and followed Rule 5, producing publication-8, which is
buildable from the same fragment file as publication-1 so no content duplicates. The doctrinal
disagreement still needs an owner's ruling, because the two documents currently tell an SME
opposite things.

### 15. The design JSON has two incompatible schemas

`specifications/CLAUDE_CODE_SKILLS_SPEC.md` defines a schema rooted at
`metadata.{skillId, skillName, audiences[], designedBy, reviewed}` with objectives nested inside
outcomes. `engine.md`'s `generateDesignJSON` emits `skill{}` plus a separate `objectives{}` map
keyed by outcome id.

`develop-training/engine.md` reads `design.skill`, `design.outcomes`, `design.objectives`,
`design.deliveryStrategy.modality`, and `design.deliverableManifest`, which matches `engine.md`
and not the spec. This run followed `engine.md`. The spec is stale and should either be updated or
marked superseded, since it is linked from `CLAUDE.md` as the skills specification.

### 16. `develop-training` generates quiz paths that contradict its own manifest

`develop-training/engine.md` line 598 builds
`` `${design.deliverableManifest.skillFolder}/${outcome.id.split('-').join('-')}/outcome-quiz.md` ``.
The `split('-').join('-')` is a no-op returning `outcome-1`, so the path becomes
`skills/<skill>/outcome-1/outcome-quiz.md`. That disagrees with the manifest it was handed on two
counts: the folder should be the kebab-case outcome title, and the file should be
`outcome-01-quiz.md`. Phase 2 of the same file correctly iterates the manifest and comments "so
the two never drift apart"; this later function does not.

### 17. `getPublicationTypes` emits display labels into a machine-read field

It returns `"SCORM 1.2"`, `"Presentation (RevealJS)"`, `"Lab Manual (PDF)"`, and `"Handouts"` into
`deliveryStrategy.publicationTypes`, while every consumer expects docType values such as
`scorm1.2` and `print`. `requiredPublicationTypesFor` in the same file returns correct docTypes.
This design wrote valid docTypes by hand.

### 18. The Step 8 human review gate is missing from the implementation

`SKILL.md` lists Step 8 as "Review the Generated Set", describing it as "the human gate" where the
team corrects whatever the generator got wrong, with validation as Step 9. Both `workflow.md` and
`engine.md` number Step 8 as "Define Deliverables" and Step 9 as validation, and neither contains
a review gate at all.

`content-design-process.md` and `CLAUDE.md` both treat Step 8 as a human review, and note that
Steps 5 to 7 are auto-generated precisely so a human can correct them. The implementation drops
the one step designed to catch generator error.

### 19. The design input contract cannot represent an audience and role matrix

Step 1 asks for a single `audience` string. This request has 2 audiences crossed with 4 roles, and
the supplied role matrix is the authoritative statement of who needs what. Everything downstream
depends on it: Rule 5 Part C validates that every intake role is served by an offering, and
publications are scoped per role.

The field was filled with a composite sentence, which no longer machine-checks against Part C.
`audiences` should be an array of `{role, audienceType, requirementLevel}`, which is close to what
`CLAUDE_CODE_SKILLS_SPEC.md` already proposes and `engine.md` does not implement.

### 20. Nothing expresses performs versus awareness depth

The role matrix's central distinction is performs (hands-on, accountable, needs practice and a
competence check) versus needs working awareness (must consume or challenge the output). This maps
directly onto learning design: same outcome, different degree.

The vocabulary has no way to say it. The distinction survives in this design only as an informal
`coverageDepth` field on one offering and in which publications that offering includes. Since the
matrix also notes that awareness-level skills are the first cut under budget pressure and are
where cross-role failures originate, losing the distinction loses the thing most worth protecting.

---

## Smaller findings

### 21. `legit-blocks.md` documents tab structure for accordion but not for steps

It specifies that `rau-accordion` requires `::: tab` children each opening with an H3. It does not
say that `rau-steps` uses the identical structure. Authoring `rau-steps` with bare H3 headings and
no tab wrappers produced a malformed block, caught only by reading the template's own sample.

### 22. Build file schema is documented nowhere, and two versions are in the wild

The knowledge base mentions `build.yaml` once, in `yaml-guide.md`, and never specifies it.
Meanwhile there are two incompatible schemas in use:

| DevOpsTraining (older) | `content-starter-pack` (current) |
| --- | --- |
| `output-type`, `base-directory`, `output-filename` | `outputType`, `baseDirectory`, `outputFilename` |
| `docCourseCode`, `docRevision`, `docClassification` | nested `publication:` block |
| single root `build.yaml` | multiple `courses/<CODE>/builds/*.build.yaml` with `linkedBuilds` |
| no skip mechanism | `skipBuild`, `debug` |

An SME told to "create/update `build.yaml`" (CLAUDE.md Section 7 Step 6) has nothing to work from.
Also worth documenting: **build file paths resolve from the repository root, not from the build
file's location**, which is unintuitive and stated only in a sample readme.

### 23. `rau-start-here` is not a template repo

`content-blocks-reference.md` sends SMEs to `rau-start-here` for `rau-md-blocks.md`, and
`yaml-guide.md` sends them there for the complete YAML reference. That repo is contributor
onboarding (`isTemplate: false`, described as "this repo will tell you how to get involved").
Neither pointer names a path, so both are effectively dead ends. The blocks reference an SME
actually needs is `samples/_template-files/` inside their own project.

### 24. `.claude/settings.json` is 0 bytes

An empty file where JSON is expected. Either populate it or delete it.

### 25. The docs-sync hook is advertised as automatic but is not installed

`CLAUDE.md` describes automatic sync validation triggering on commits to `docs/`. The hook exists
at `.claude/hooks/pre-commit-docs-sync` and `.git/hooks/` contains nothing but samples, so it has
never run. The hook body is also advisory only: it prints next steps and exits 0.

### 26. Two skill files declare the same `name`

`sync-docs-to-rules.md` and `sync-docs-to-rules-prototype.md` both declare
`name: sync-docs-to-rules`. Harmless while neither is loadable, but it will collide the moment
anyone moves them into `SKILL.md` directories. Decide which is current and delete the other.

### 27. Prose typo in `workflow.md`

Step 8 context: "Most are authored content. Some (VMs, project files) are not authored content at
all, LeGIT but are still required, tracked, and exported to DevOps." The words "at all, LeGIT but
are" are garbled.

---

## Where a human was required

The honest measure of how far the automation currently reaches. Every one of these was a judgment
call the repo could not make:

1. **Scoping the program.** 31 statements across 4 roles do not fit a one-week ILT. Choosing to
   design the 3-statement shared core, and to defer the other 28, was a judgment about what would
   be defensible, not something any rule produced.
2. **Overriding the delivery strategy engine.** The engine said e-learning on a silent tie. The
   design says blended, on instructional design grounds, and differs from the stakeholder's stated
   ILT. All three positions had to be recorded and reconciled by hand.
3. **Deciding which file contract to obey.** The knowledge base and the build system disagree.
   Choosing the build system, and inventing the fragment-file compromise that keeps
   objective-level authoring alive, was entirely a human call.
4. **Resolving the standalone objective publication conflict.** Rule 5 says required, the template
   says never. Someone had to pick.
5. **Separating assumption from fact.** The request answers four of six delivery factors. Marking
   the other two as assumptions with stated consequences, rather than filling them in plausibly,
   is the difference between a usable design and a fabricated one. Nothing in the repo enforces it.
6. **Judging every validator failure.** Three outcomes failed the ABCD behaviour check. Deciding
   that the validator was wrong and the outcomes were right required knowing what ABCD is for.
7. **Recognising that reference documentation is not training.** The request asks for updated
   documentation. It derives from no outcome, so it cannot flow through the chain and needed to be
   split out as separate work.

The pipeline's genuine strength is the derivation chain: strategy to offerings to publications to
activities to deliverables held up well, and forced useful questions in the right order. Its
weakness is that the automation around that chain is either unimplemented or wrong often enough
that it cannot be trusted without review.

---

## Build verification

Run with the RAU VSCode extension v1.0.1.

### The central question is answered: objective-level fragments build correctly

`courses/PST001/builds/presentations.build.yaml` produced a 42 KB revealjs deck from
`presentation.md` plus four frontmatter-free `objective-0N.md` fragments. Verified in the
artifact, not just the build log:

- All 23 authored slides are present **in source order**: the framing slides from
  `presentation.md`, then each objective's divider, teaching slides, and knowledge check, ending
  with the outcome summary
- 21 speaker-notes blocks and 6 `::: script` blocks survived
- All four knowledge-check `ANSWER:` markers landed in the notes, not on the slides
- `rau-slide-divider` and `rau-slide-question` were correctly transformed into reveal.js section
  attributes such as `data-state="RACTimportance RACTtop"`

Outcome 2 and 3 decks skipped as configured, since their content is scaffolded rather than
authored.

**Therefore the compromise in finding 5 is sound.** An ordered `files:` array is a working
replacement for the `!include` directive that does not exist, and objective-level authoring is
reachable in the current build system. This should be documented as the recommended pattern when
`file-mapping-guide.md` is rewritten.

### The print path also works

`courses/PST001/builds/lab-manual.build.yaml` produced a valid 20 page PDF (5.2 MB, PDF 1.7),
assembling the course front cover YAML, the template's `user-info.md` and `comments.md`, the
authored lab, and the back cover. The authored section headings are present in the extracted text,
including the outcome title, "Before You Begin", and both exercise headings.

This exercises a wholly separate toolchain from revealjs (WeasyPrint) and a different block
vocabulary, so it is independent evidence that the authored content is sound: `docType: lab`,
`varsLocal` substitution, `::: steps`, `{.beforeBegin}` style heading attributes, and
`rau-input-singleline` inside tables all survived.

`scorm1.2` verification is still outstanding.

### 31. Every build run wipes the entire output directory

Each invocation begins with `cleaning <repo>/output`, deleting all previously built artifacts, not
just the ones the current build file produces. Building the lab manual destroyed the revealjs deck
built minutes earlier.

On its own this is defensible. Combined with finding 28 it is not: because `linkedBuilds` does not
fire, a complete publication set cannot be assembled at all. Building each publication separately
destroys the previous one, and the aggregator that exists to solve that problem does not work.

The workaround is to put every publication for a course in a single build file, which conflicts
with the template's own convention of separate named build files per publication type. Until
`linkedBuilds` is fixed, one build file per course is the only pattern that yields a full set.

### 28. `linkedBuilds` does not fire

Building `courses/PST001/builds/all.build.yaml` reported "Building 1 documents", skipped the
deliberately empty aggregator entry, and never followed its three linked build files.

The aggregator copies the pattern from the template's own
`courses/LGT001/builds/all.build.yaml` verbatim, and the template readme advertises the feature:
"The `all.build.yaml` file also demonstrates **linked builds**, referring to other build files to
be built as well." Suggestively, the template's root `build.yaml` ships with its `linkedBuilds`
block commented out.

A control run against the template's own unmodified aggregator will establish whether the feature
is broken in the extension or misused here. Until then, treat one build file per set of
publications as the safe pattern, rather than an aggregator plus links.

### 29. A build reports success while producing no output

The first build run reported "4 build(s) completed, 1 skipped, 2 aborted" and wrote **zero files**
to `output/`. The cause was an uninitialized `style-rau-base` submodule: every CSS lookup logged
`File not found - ...\style-rau-base\*.*` and the builds then failed silently while still being
counted as completed.

Two problems worth separating:

1. **The exit status is misleading.** "Completed" should mean an artifact exists. A build that
   emits nothing should be counted as aborted, and the missing CSS should be a hard error rather
   than a logged line the run continues past.
2. **A fresh clone is not ready to build.** `git clone` without `--recurse-submodules` leaves
   `style-rau-base` empty, and nothing in the build output says so in those terms. The template
   readme does mention cloning the style submodule, but the failure mode gives no hint that this
   is the cause. Worth adding to the knowledge base's getting-started material, since this will
   catch every SME once.

### 30. The default build task only ever builds the root `build.yaml`

The VSCode build task pipes the repository-root `build.yaml` into `RAU_builder.py`. In a repo
created from `content-starter-pack`, that file contains only the template's sample regex and SCORM
builds, so pressing the default build shortcut builds the samples and none of the project's own
content, with nothing in the output indicating that the project was skipped.

Real project builds must be launched by right-clicking the specific `*.build.yaml`. Either
document that clearly, or have new projects replace the root `build.yaml` with one that points at
their own course builds.

---

## Recommended order of remedial work

1. ~~**Package the skills so they load.**~~ **Done**, merged in PR #8. Nothing else in the repo
   was usable until this landed.
2. **Rewrite `file-mapping-guide.md` and Rule 6 against `content-starter-pack`** (finding 5). This
   is the biggest job and the one that currently sends SMEs down a path that does not build.
3. **Correct `legit-yaml.md`**: `chunk:` not `skill:`, the real `docType` set, and the separation
   between `docType` and `outputType` (findings 7, 8).
4. **Add a documented Step 0 intake with a template** (finding 4).
5. **Fix or remove the broken validators** (findings 9, 11, 12). A validator that cannot fail is
   worse than none, because it is cited as evidence of quality.
6. **Reconcile `workflow.md` and `engine.md`** into one implementation (findings 10, 18).
7. **Decide the standalone objective publication question** (finding 14), and record the ruling in
   both documents.
8. **Package as a Claude Code plugin** and fix `README.md` Option 1 (finding 3).
9. **Document the build file schema** (finding 22).
10. The remaining low-severity findings, as housekeeping.

Nothing in `docs/` or `.claude/rules/` was changed by this shakeout. These are recommendations for
review, not applied edits.
