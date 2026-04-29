---
name: skill-name-here
description: One-sentence description of what this skill does and when to use it. This text is what Claude reads when deciding whether to invoke the skill, so be specific about the use case.
license: MIT
metadata:
  version: 1.0.0
  category: documentation
  audience: RS&H internal
---

# Skill Name (Title Case)

One paragraph: what this skill produces, who reads the output, and when Claude should invoke it.

## Core Rules

These apply to every skill in the RS&H framework.

1. **No external party referenced** unless explicitly cited as a source.
2. **No em-dashes.** Use commas, parentheses, or split sentences.
3. **No "leverage" as a verb.** Use "use" or rephrase.
4. **No marketing language** ("revolutionary," "best-in-class," "transformative").
5. **No emojis** unless the user explicitly requests them.

[Add skill-specific rules below, if any.]

## Inputs

What the skill needs from the user before it runs:

- [Input 1, for example: a topic, a file path, a date range]
- [Input 2]
- [Input 3, marked optional if applicable]

If a required input is missing, ask once. Do not guess.

## Output

What the skill produces:

- **Format:** [for example: markdown file, HTML file, structured table, JSON]
- **Length target:** [word count or rough size]
- **Default location:** [where the file lands, for example: `<domain>/projects/<project>/_overview/`]
- **File naming:** lowercase-hyphens, follow `file-naming-and-output-standards.md`

## Workflow

1. [Step 1: what Claude does first, for example: "Read the source material"]
2. [Step 2: for example: "Outline the section structure"]
3. [Step 3: for example: "Draft section by section"]
4. [Step 4: for example: "Run the document-reviewer skill on the draft"]
5. [Step 5: for example: "Save to the default location"]

## What to avoid

Patterns that make this skill produce bad output:

- [Anti-pattern 1]
- [Anti-pattern 2]
- [Anti-pattern 3]

## Quality bar

Before declaring done, verify:

- [ ] [Check 1, specific to this skill]
- [ ] [Check 2]
- [ ] No em-dashes
- [ ] No "leverage" as a verb
- [ ] No external party referenced
- [ ] No emojis (unless requested)
- [ ] File name follows the lowercase-hyphens convention
- [ ] File saved to the default location

## References

If this skill includes longer reference material, list it here:

- `references/<file>.md` - [purpose, for example: "Postmortem template structure"]
- `references/<file>.md` - [purpose]

Reference files load on demand. Keep the SKILL.md itself short.

## Scripts

If this skill includes deterministic scripts (Python, bash), list them here:

- `scripts/<file>.py` - [purpose, for example: "Validate input format"]

Scripts run via the `Bash` tool. Use scripts when the answer is deterministic. Do not ask Claude to compute when an algorithm should.

## Assets

If this skill includes templates, sample data, or expected outputs:

- `assets/<file>` - [purpose]

## Examples

A short worked example of a successful invocation:

> **User:** [example user prompt]
>
> **Skill output:** [brief description of what the skill produced]

## Promotion path

A skill starts in `<domain>/_claude/skills-local/`. Promotion to the domain plugin requires:

- Three successful uses by the author
- Domain Steward review
- No PII or client identifiers in examples
- Hooks (if any) reviewed for safety

Cross-domain promotion to `rsandh-org-plugin` requires two Domain Steward signoffs.
