---
name: diagram-builder
description: Create architecture and concept diagrams for RS&H framework documentation. Default output is inline SVG (embeddable in HTML) or ASCII (for markdown). Use when visualizing the layered context model, access flow, plugin distribution, phased rollouts, or any structural concept.
license: MIT
metadata:
  version: 1.0.0
  category: documentation
  audience: RS&H internal
---

# Diagram Builder

You produce diagrams that argue visually, not just label boxes. The output is either inline SVG (for HTML documents) or ASCII (for markdown). No external dependencies, no image files.

## Core Philosophy

A diagram should communicate something words alone can't. If removing the labels would leave you with no meaning, redesign. The shape itself should carry the argument.

## Default Style

**Visual tokens (match the html-developer skill's palette):**
- Ink: `#1a1a1a`
- Accent: `#2c4a6e` (navy blue)
- Muted: `#6b6b68`
- Tint background: `#f7f9fc`
- Rule: `#e3e3df`

**Typography in SVG:** System sans-serif. 14px body, 12px labels, 16px titles.

**Lines:** 1.5px for primary structure, 1px for secondary. Solid for definite relationships, dashed for proposed/conditional.

**Shapes:**
- Rectangles with 4px radius for entities/containers.
- Rounded rectangles (12px+) for processes/actors.
- Plain text with arrow for relationships.
- Avoid 3D, gradients, drop shadows.

## Diagram Types

### Layered Context (RS&H access model)

Three horizontal bands stacked: Org → Domain → User. Arrows showing what loads when. Use for explaining hierarchical CLAUDE.md.

### Access Flow

A user → SharePoint group membership → which folders sync → what Claude sees on disk. Use for explaining the access model to Judy or admins.

### Plugin Distribution

Plugin source repo → marketplace → user machines. Show the path and the gatekeeping points (PR review, marketplace admin approval, user install).

### Phased Rollout

Horizontal timeline with phase boxes. Each phase has scope and exit metric. Use for the rollout section of leadership briefs.

### Primitive Hierarchy (WAT + Skills + Plugins)

Plugin as outer container. Inside: skills, slash commands, hooks, agents, MCP servers. Below: tools as the foundation. CLAUDE.md as a sidebar always-loaded layer. Use for the framework explainer.

## SVG Skeleton

```svg
<svg viewBox="0 0 720 400" xmlns="http://www.w3.org/2000/svg" font-family="-apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, sans-serif">
  <defs>
    <marker id="arrow" viewBox="0 0 10 10" refX="9" refY="5" markerWidth="6" markerHeight="6" orient="auto">
      <path d="M0,0 L10,5 L0,10 z" fill="#1a1a1a"/>
    </marker>
  </defs>
  <!-- background -->
  <rect width="720" height="400" fill="#fdfdfc"/>
  <!-- shapes -->
  <rect x="40" y="40" width="200" height="60" rx="4" fill="#f7f9fc" stroke="#2c4a6e" stroke-width="1.5"/>
  <text x="140" y="76" text-anchor="middle" font-size="14" fill="#1a1a1a">Label</text>
  <!-- arrow -->
  <line x1="240" y1="70" x2="380" y2="70" stroke="#1a1a1a" stroke-width="1.5" marker-end="url(#arrow)"/>
  <text x="310" y="64" text-anchor="middle" font-size="12" fill="#6b6b68">relationship</text>
</svg>
```

Always include `viewBox` for responsive scaling. Embed directly in the HTML document.

## ASCII Pattern (for markdown)

```
+-----------------+        +-----------------+
|  Org CLAUDE.md  |  -->   | Domain CLAUDE.md|
|  (always loads) |        |  (loads in dir) |
+-----------------+        +-----------------+
         |                          |
         v                          v
                  +--------------+
                  | User session |
                  +--------------+
```

Use box-drawing characters or simple ASCII. Don't try to be clever with Unicode if it might not render in all viewers.

## Workflow

1. **Confirm the argument.** What is this diagram supposed to make obvious? State it in one sentence before drawing.
2. **Sketch in ASCII first** even if the final output is SVG. Forces you to commit to structure.
3. **Choose the smallest viable diagram.** 5 boxes is usually plenty. 12+ boxes means you're documenting, not arguing.
4. **Render in SVG** if going into HTML. ASCII if going into markdown.
5. **Verify in context.** Embed in the actual document and check it scales, prints, and reads at the intended size.

## Anti-Patterns

- Boxes with no relationships drawn between them. That's a list, not a diagram.
- Decorative arrows that don't carry meaning.
- Color used for decoration rather than meaning.
- Tiny text that's unreadable when printed.
- Diagrams that need a paragraph of explanation. If they do, the diagram failed.

## Quality Bar

- [ ] One-sentence argument the diagram makes is clear.
- [ ] Removing labels would still leave the structure meaningful.
- [ ] Renders correctly inline in the host document (HTML or markdown).
- [ ] Prints legibly at the intended size.
- [ ] Uses the standard color palette.
- [ ] No external assets.
