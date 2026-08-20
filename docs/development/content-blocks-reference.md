# Content Blocks Reference Guide

RAU markdown content blocks provide simple markdown structures that are rendered as complex objects when previewed and built into output documents. This guide documents all available content blocks, their syntax, use cases, and outputs.

## Table of Contents

- [Quick Start with Snippets](#quick-start-with-snippets)
- [Fenced Div Fundamentals](#fenced-div-fundamentals)
- [Basic Blocks](#basic-blocks)
- [Complete Block Reference](#complete-block-reference)

**Note**: This is a quick reference guide. For complete documentation of all content blocks including print, SCORM/e-learning, and presentation-specific blocks, see [rau-md-blocks.md](https://github.com/RAU-EIT/rau-start-here) in the rau-start-here repository.

## Quick Start with Snippets

The easiest way to insert content blocks is through VSCode snippets:

1. Press `Ctrl+Space` in a markdown file
2. Type `rau`
3. Browse available snippets
4. Press Enter or Tab to insert

## Fenced Div Fundamentals

Almost all RAU content blocks use [Pandoc fenced divs](https://pandoc.org/MANUAL.html#extension-fenced_divs), a markdown extension for custom labeled blocks.

### Basic Structure

```markdown
::: block-name

standard markdown goes here

:::
```

This renders as HTML div elements that can be styled with CSS and transformed with Lua filters.

### With Attributes

Fenced divs can have multiple classes and additional attributes:

```markdown
::: {.block-name .additional-class attribute="value"}

content here

:::
```

Note: When using attributes, the block name requires a dot prefix (`.block-name`).

### Nesting

Fenced divs can be nested for complex structures. Use more colons to indicate nesting level.

## Basic Blocks

### Video Embed

Embed video content from local files, HTTP/HTTPS, YouTube, or Vimeo.

**Markdown:** `%[alt text](path/to/video.mp4)`

With transcript: `%[alt text](path/to/video.mp4){transcript='path-to-transcript.vtt'}`

**Intended Output:** SCORM

**Use Cases:**
- Demonstrations
- Procedural walkthroughs
- Expert interviews
- Equipment operation videos

### Audio Embed

Embed audio content from local files or HTTP/HTTPS sources.

**Markdown:** `~[alt text](path/to/audio.mp3)`

With transcript: `~[alt text](path/to/audio.mp3){transcript='path-to-transcript.vtt'}`

**Intended Output:** SCORM

**Use Cases:**
- Podcast integration
- Narration tracks
- Expert commentary

### Alert Blocks

Draw attention to important information with five alert types: Reference, Warning, Attention, Important, Tip.

**Available Types:**
- `.reference` - Reference material
- `.warning` - Important warnings
- `.attention` - Call for attention
- `.important` - Critical information
- `.tip` - Helpful hints

**Intended Output:** SCORM

## Complete Block Reference

This guide provides quick reference for the most commonly used content blocks. For comprehensive documentation of all available blocks organized by output type, see:

**[rau-md-blocks.md](https://github.com/RAU-EIT/rau-start-here)** in the rau-start-here repository includes:

**Print Output Blocks**:
- Page breaks
- Input fields (with fillable forms)
- Column layouts

**SCORM/E-Learning Blocks**:
- Accordion (expandable sections)
- Hero (banner/intro blocks)
- Image hotspots (clickable images)
- Flip cards (interactive cards)
- Steps (procedural sequences)
- Quiz blocks (interactive assessment)
- Knowledge checks (ungraded inline quizzes)

**Presentation Blocks**:
- Slide layouts (title, content, two-column)
- Speaker notes
- Transition effects
- Fragment reveals

All blocks use the fenced div syntax documented above.

