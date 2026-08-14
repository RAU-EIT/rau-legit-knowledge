# LeGIT System Fundamentals

LeGIT is a markdown-based learning content creation system designed around skills-based learning. It enables authors to write learning content in markdown, organize it by skill, preview it in real-time, and build it into multiple output formats (PDFs, presentations, e-learning modules).

## What is LeGIT?

LeGIT stands for "Learning Environment with Git Infrastructure Technology." It's a system that:

- Allows SMEs to write learning content in markdown (plain text)
- Stores content in GitHub for version control
- Provides live preview in VSCode
- Builds content into multiple output formats automatically:
  - PDFs (lab manuals, guides)
  - HTML presentations (RevealJS)
  - SCORM modules (e-learning)
  - Presentation videos

## Core Design Principles

LeGIT is built on three foundational principles:

### 1. Human-Readable Content at Rest

Learning content should always be in clear text. At no point should you need proprietary software to see what you wrote.

- **Development:** Plain markdown text
- **Editing:** Any text editor
- **Previewing:** VSCode webview (HTML)
- **Publishing:** Clear text (SCORM, RevealJS) or readable format (PDF)

### 2. Separate Style from Content

CSS stylesheets, not embedded formatting, control appearance.

- Markdown describes **structure** (headings, lists, blocks)
- CSS files describe **appearance** (colors, spacing, fonts)
- Changes to style don't require content edits

### 3. Automate as Much as Possible

Manual document building is time-consuming and error-prone. LeGIT automates:
- Converting markdown to multiple formats
- Generating tables of contents
- Applying consistent styles
- Assembling multi-file documents
- Generating SCORM packages

## Tools and Technologies

**Pandoc** — Converts markdown to HTML and other formats  
**Lua Filters** — Transform Pandoc output to implement RAU-specific blocks  
**WeasyPrint** — Converts HTML to PDF  
**CSS Stylesheets** — Define visual appearance (style-rau-base)  
**VSCode Extension** — Provides markdown editing with live preview

## Project Structure

Following RAU's instructional design framework, content is organized hierarchically by **skill**, **learning outcome**, and **learning objective**. This structure enables modular reuse while keeping related files organized together.

**Design Hierarchy:**
- **Skill** → Job- or product-level capability; serves as the course-level container
- **Learning Outcome** → Measurable evidence of skill attainment; minimum complete unit of skill verification
- **Learning Objective** → Step-level actions supporting an outcome
- **Activities** → Passive (lecture), Interactive (lab), Assessment (quiz)

**Folder and File Organization:**

```
skills/
├── skill-name-1/                    # Container for one skill
│   ├── outcome-01/                  # Container for one outcome
│   │   ├── objectives/              # Folder for objective-level content
│   │   │   ├── objective-01/
│   │   │   │   ├── lecture.md       # Objective-level lecture content
│   │   │   │   └── media/           # Images, diagrams, etc.
│   │   │   ├── objective-02/
│   │   │   │   ├── lecture.md
│   │   │   │   └── media/
│   │   │   └── objective-03/
│   │   │       └── ...
│   │   ├── lecture.md               # Aggregated outcome-level lecture
│   │   ├── lab.md                   # Interactive practice
│   │   ├── quiz.md                  # Assessment questions
│   │   └── media/                   # Outcome-level media
│   ├── outcome-02/
│   │   └── ...
│   ├── outcome-03/
│   │   └── ...
│   └── media/                       # Skill-level media (shared across outcomes)
├── skill-name-2/
│   └── ...
└── skill-name-3/
    └── ...
```

**Key Design Patterns:**

1. **Objective-Level Content:**
   - Each objective has its own lecture file (`lecture.md`) containing explanation, terminology, concepts, and key teaching points
   - Media (images, diagrams) can be co-located in each objective's `media/` folder

2. **Outcome-Level Aggregation:**
   - Objective-level lectures are authored separately, then aggregated into `lecture.md` at the outcome level
   - This aggregated lecture is presented to students
   - Labs and quizzes are typically designed at the outcome level

3. **Coverage Requirements:**
   - Each outcome **must** include passive (lecture), interactive (lab), and assessment (quiz) coverage
   - Objectives may share the outcome-level lab and assessment
   - Objective-level lectures should be modular and reusable

4. **Media Organization:**
   - Objective-level `media/` folders: specific to that objective's content
   - Outcome-level `media/` folders: shared across multiple objectives within that outcome
   - Skill-level `media/` folders: shared across all outcomes in the skill

Each skill can be published to multiple formats while maintaining instructional integrity:
- **PDF** — Printable lab manuals and student guides
- **RevealJS** — Web-based presentations for instructors
- **SCORM** — E-learning modules for online delivery
- **Video** — Recorded presentations with narration

---

For detailed information on how to author content in LeGIT, see the content design and development guides.
