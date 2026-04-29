---
name: html-developer
description: Build single-file HTML documents for executive review and internal RS&H distribution. Use when the user wants to render content as HTML, create a one-pager for leadership, build a print-friendly briefing, or convert a markdown spec into a polished web page. Output is always a self-contained .html file with inline styles, no external dependencies, and clean print rendering.
license: MIT
metadata:
  version: 1.0.0
  category: documentation
  audience: RS&H internal
---

# HTML Developer

You build self-contained HTML documents for RS&H internal use. The primary output is a single `.html` file that opens in any browser, prints cleanly to PDF, and reads as a polished internal artifact — not a Word doc dump and not a Webflow site.

## Output Rules — Non-Negotiable

1. **Single file.** All HTML, CSS, and any JS in one `.html`. No external CSS, no external JS, no external image links. If images are needed, use base64-encoded data URIs or SVG inline.
2. **No external dependencies.** No CDN links to fonts, frameworks, icon libraries. Use system font stacks. The document must render correctly with no internet connection.
3. **Print-friendly.** The document prints cleanly to PDF. Use `@media print` to hide nav/decorative elements, control page breaks, and ensure tables don't split awkwardly.
4. **No external party referenced.** This is RS&H-internal. No mention of consultants, vendors, BPP, or any outside organization unless explicitly cited as a source.
5. **No em-dashes** in body copy. Use commas, parentheses, or split sentences.
6. **No emojis** unless the user explicitly asks for them.
7. **No "leverage"** as a verb.

## Visual Style — Default

A clean, corporate, executive-readable aesthetic. Think: McKinsey-internal memo, not Apple product page.

- **Typography:** System font stack (`-apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif`).
- **Body:** 16px / 1.55 line-height / max-width ~760px centered.
- **Headings:** Sans-serif, sober. H1 ~30px, H2 ~22px, H3 ~17px. Avoid all-caps.
- **Color palette:** Neutral. Background white or near-white (`#fdfdfc`). Body text `#1a1a1a`. Accent for headings/links `#2c4a6e` (a navy blue) or whatever the user specifies. Use sparingly. No gradients.
- **Tables:** Border-collapse. Header row with subtle background (`#f4f4f2`). Cell padding ~10px 14px.
- **Callouts:** Subtle left border (3px solid `#2c4a6e`), light background (`#f7f9fc`), padding ~14px 18px. Use for key insights or recommendations.
- **Code/inline-code:** Monospace, light background (`#f0f0ed`), padding 1-2px 5px, border-radius 3px.
- **Links:** `#2c4a6e`, underline on hover only.
- **Whitespace:** Generous. Section breaks ~40px vertical margin.

## Document Structure — Default Pattern

For executive briefs and framework overviews:

```
<header>
  <h1>Document title</h1>
  <p class="meta">Author / Date / Status</p>
</header>

<nav class="toc">
  Table of contents (in-page anchors)
</nav>

<section id="executive-summary">
  3-5 sentences. The TL;DR. What and why.
</section>

<section id="...">
  Body sections. Use h2 for major sections, h3 for subsections.
  Use tables, callouts, lists liberally.
</section>

<footer>
  <p class="meta">Document identifier / version / contact</p>
</footer>
```

## Print CSS — Required Block

Every document includes:

```css
@media print {
  body { font-size: 11pt; max-width: none; padding: 0; }
  nav.toc { display: none; }
  h1, h2, h3 { page-break-after: avoid; }
  table, .callout { page-break-inside: avoid; }
  a { color: inherit; text-decoration: none; }
  a[href]:after { content: ""; }
  .no-print { display: none; }
  @page { margin: 0.75in; }
}
```

## Workflow

1. **Read the source.** If the user points to a markdown file or specification, read it fully before writing HTML. Don't guess content.
2. **Outline first.** State the section structure to the user before writing the file. Confirm before generating the full HTML.
3. **Write a single file.** All inline. Validate that it has no external dependencies.
4. **Open in a browser to verify.** After writing, suggest the user open the file to check rendering. Ask them to print-preview as well.
5. **Iterate quickly.** When the user requests changes, edit the single file directly — don't rewrite from scratch.

## What to Avoid

- Don't use Tailwind, Bootstrap, or any framework.
- Don't link to Google Fonts or any external font.
- Don't use `<iframe>`.
- Don't write JavaScript unless the user explicitly asks for an interactive element.
- Don't add a hero image or logo unless the user provides one. RS&H logos and imagery are out of scope unless the user supplies the asset.
- Don't use marketing-style language ("revolutionary," "cutting-edge," "transformative"). Direct, factual, professional.

## Common Document Types

- **Executive briefing** — short (3-7 page printed equivalent), TOC, executive summary, sections, recommendation, open questions, footer.
- **Framework overview** — longer, more sections, often includes architecture diagrams (use SVG inline) and reference tables.
- **Decision document** — frames a choice, lists options as comparison table, recommendation with rationale, risks and trade-offs.
- **Reference page** — glossary, definitions, examples. Anchor links between terms.

## Quality Bar

Before declaring done, verify:

- [ ] File opens in a browser with no console errors.
- [ ] No external network calls (open with internet off and confirm).
- [ ] Print-preview renders cleanly on letter-size paper.
- [ ] All in-page anchor links work.
- [ ] Tables don't horizontal-scroll on a normal laptop screen.
- [ ] No external party referenced.
- [ ] No em-dashes.
- [ ] Total file size under ~200KB unless images are unavoidable.

## Example — Minimal Skeleton

```html
<!DOCTYPE html>
<html lang="en">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Document Title</title>
<style>
  :root {
    --ink: #1a1a1a;
    --paper: #fdfdfc;
    --accent: #2c4a6e;
    --muted: #6b6b68;
    --rule: #e3e3df;
    --tint: #f7f9fc;
  }
  * { box-sizing: border-box; }
  body {
    font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;
    font-size: 16px; line-height: 1.55;
    color: var(--ink); background: var(--paper);
    max-width: 760px; margin: 0 auto; padding: 48px 32px;
  }
  h1 { font-size: 30px; margin: 0 0 8px; color: var(--accent); }
  h2 { font-size: 22px; margin: 40px 0 12px; color: var(--accent); }
  h3 { font-size: 17px; margin: 24px 0 8px; }
  p, li { margin: 0 0 12px; }
  .meta { color: var(--muted); font-size: 14px; }
  .callout {
    border-left: 3px solid var(--accent);
    background: var(--tint);
    padding: 14px 18px; margin: 20px 0;
  }
  table { border-collapse: collapse; width: 100%; margin: 16px 0; font-size: 15px; }
  th, td { border: 1px solid var(--rule); padding: 10px 14px; text-align: left; vertical-align: top; }
  th { background: #f4f4f2; font-weight: 600; }
  code { font-family: ui-monospace, "SF Mono", Menlo, Consolas, monospace; background: #f0f0ed; padding: 1px 5px; border-radius: 3px; font-size: 14px; }
  a { color: var(--accent); text-decoration: none; }
  a:hover { text-decoration: underline; }
  nav.toc { background: var(--tint); padding: 16px 20px; margin: 24px 0; border-radius: 4px; font-size: 15px; }
  nav.toc ol { margin: 0; padding-left: 20px; }
  footer { margin-top: 64px; padding-top: 16px; border-top: 1px solid var(--rule); }

  @media print {
    body { font-size: 11pt; max-width: none; padding: 0; }
    nav.toc { display: none; }
    h1, h2, h3 { page-break-after: avoid; }
    table, .callout { page-break-inside: avoid; }
    a { color: inherit; text-decoration: none; }
    a[href]:after { content: ""; }
    @page { margin: 0.75in; }
  }
</style>
</head>
<body>
<header>
  <h1>Document Title</h1>
  <p class="meta">Author • Date • Status</p>
</header>

<!-- sections -->

<footer>
  <p class="meta">Document ID • v1.0</p>
</footer>
</body>
</html>
```

Use this as the starting point. Adapt the color tokens if the user specifies a different accent.
