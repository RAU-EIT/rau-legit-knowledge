# LeGIT Best Practices Guide

This guide documents recommended practices for writing effective learning content in LeGIT.

## Semantic Markdown

Write for **structure and meaning**, not for appearance.

**Good:**
```markdown
# Document Title

## Section

Content here.

- List item 1
- List item 2
```

**Not:**
```markdown
# Document Title {style="color:blue"}

## Section {.special-styling}

Content here.

- List item 1 {.emphasized}
```

Let CSS handle appearance. Markdown should describe **what** it is, not **how** it looks.

## Content Reusability (DRY Principle)

- Write at the **objective level** for modularity
- Use `!include()` directives to reference content
- Use variables (`%varsGlobal.name%`) for shared information
- Avoid duplicating content across files

## Media Management

- **Use SVG images** when possible (scalable, versionable)
- **Use relative paths** for all media references
- Store media in a `media/` folder within the skill directory
- Use **human-readable file names** (not auto-generated IDs)

## Markdown Style Standards

- Use **consistent heading hierarchy** (no skipped levels)
- Maintain **consistent list formatting**
- Use **code fences** (``` ```) for code blocks, not inline HTML
- **Avoid inline HTML** — use markdown syntax instead
- **Avoid inline styles** — let CSS handle appearance

## Accessibility and Readability

- Write in **active voice** when possible
- Keep sentences **short and clear**
- Use **meaningful headings** that describe content
- Include **alt text** for all images
- Use **proper heading hierarchy** for screen readers

## Slide Design (Presentations)

For presentations, follow these guidelines:

- **Start with structure**, not visuals
- Keep slides **simple**: heading + 3-4 bullet points
- Use **relevant images** to support concepts
- Limit layouts to **3 columns or rows maximum**
- Focus on **concise messaging**, not fancy effects

## File and Naming Conventions

- Use **kebab-case** for file names (e.g., `objective-01-lecture.md`)
- Name based on content type: `objective-##`, `outcome-##`, `skill-##`
- Keep folder structure **shallow and logical**
- Organize by skill, not by document type

## Best Practices Summary

✓ Write at objective level for reusability  
✓ Use variables instead of duplicating text  
✓ Let CSS handle styling, not markdown  
✓ Use relative paths and human-readable file names  
✓ Keep presentations simple and clear  
✓ Write for meaning, not appearance

---

For more detail, see the full style guides in the LeGIT documentation.
