# LeGIT System Fundamentals

LeGIT is a markdown-based learning content creation system designed around skills-based learning. It enables authors to write learning content in markdown, organize it by skill, preview it in real-time, and build it into multiple output formats (PDFs, presentations, e-learning modules).

## What is LeGIT?

LeGIT stands for "Learning Environment with Git Infrastructure Technology." It's a system that:

- Allows SMEs to write learning content in markdown (plain text)
- Stores content in GitHub for version control
- Provides live preview in VSCode
- Builds content into multiple formats automatically:
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

Content is organized by **skill** — the smallest reusable unit of learning content. Skills contain outcomes, which contain objectives, which contain content files.

```
skills/
├── skill-name-1/
│   ├── [outcome-title-1]/                    (e.g., analyze-system-components)
│   │   ├── objective-01/
│   │   │   ├── lecture.md                   (Core instruction)
│   │   │   ├── knowledge-check.md           (Embedded ungraded checks)
│   │   │   ├── lab.md                       (Only if standalone)
│   │   │   ├── quiz-questions.md            (Graded assessment pool)
│   │   │   └── media/                       (Objective-specific images)
│   │   ├── objective-02/
│   │   │   └── [same structure]
│   │   ├── outcome-01-lecture.md            (Aggregates objectives via !include)
│   │   └── outcome-01-quiz.md               (Outcome-level assessment)
│   │
│   ├── [outcome-title-2]/                    (e.g., diagnose-failures)
│   │   └── [same structure as outcome-title-1]
│   │
│   ├── skill-01-practical.md                (Skill-level capstone, optional)
│   ├── skill-01-presentation.md             (Classroom presentation, optional)
│   └── media/                                (Shared media across outcomes)
│
├── skill-name-2/
│   └── [same structure]
```

Each skill can be published to:
- **PDF** — Printable lab manuals
- **RevealJS** — Web presentations
- **SCORM** — E-learning modules
- **Video** — Recorded presentations

---

For detailed information on how to author content in LeGIT, see the content design and development guides.
