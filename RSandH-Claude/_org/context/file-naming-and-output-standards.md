# File Naming and Output Standards

This file defines how Claude names and structures files in RS&H sessions. The goal is that any associate can find any file by name and know what is inside before opening it.

## File names

- **Lowercase only.** No mixed case in file names.
- **Hyphens for separators.** No spaces, no underscores, no camelCase.
- **Descriptive but short.** A reader should know what the file is from the name alone.
- **Date format:** `YYYY-MM-DD` if the file is dated. Place the date at the start of the name.
- **Versioning:** `-v1`, `-v2`. Only when explicitly versioning. Do not stack qualifiers like `-final-real-v2`.

Examples of good file names:

- `q1-utilization-summary.md`
- `vw-project-backlog-phase.sql`
- `adp-poc-pyspark-notebook.py`
- `2026-04-29-fabric-pipeline-runbook.md`
- `judy-framework-overview.html`

Examples to avoid:

- `Final Report.docx` (spaces, mixed case, vague)
- `report_v3_FINAL_real.md` (underscores, repeated qualifiers)
- `data.xlsx` (not descriptive)
- `My Notes.txt` (spaces, vague)

## Output formats by deliverable type

| Deliverable | Format | Notes |
|---|---|---|
| Documentation, runbooks, ADRs | `.md` | Markdown, lowercase-hyphens |
| Data models, tracking, mapping | `.xlsx` | Clear sheet structure with named tabs |
| SQL queries | `.sql` | Not inline in markdown unless a one-liner |
| Python scripts | `.py` | Docstrings on functions |
| PySpark notebooks | `.ipynb` | Cells annotated with markdown headers |
| Presentations | `.pptx` | Use the RS&H presentation template |
| HTML deliverables | `.html` | Single file, no external dependencies |
| Diagrams | Inline SVG (in HTML) or ASCII (in markdown) | No external image dependencies |
| Data exports for analysts | `.csv` or `.parquet` | Match what the consumer expects |

## SQL conventions

- Uppercase keywords (`SELECT`, `FROM`, `WHERE`, `JOIN`).
- Lowercase aliases.
- CTEs over subqueries when the query has more than one logical step.
- Meaningful alias names. Not `t1`, `t2`.
- Snake_case for column and view names.
- Comment non-obvious filter logic.

## Python conventions

- Clear variable names. No magic numbers.
- Docstrings on functions.
- Prefer readability over cleverness.
- One responsibility per function.

## DAX conventions

- One measure per line for complex expressions.
- Comments for non-obvious logic.
- Match exact column and table names from the semantic model.
- Use `CALCULATE`, `REMOVEFILTERS`, `USERELATIONSHIP` deliberately. Document why.

## Markdown conventions

- Sentence-case headings unless conventional otherwise (for example, "Architecture Decision Record" stays title-case).
- Tables when comparing options, mapping inputs to outputs, or listing fields.
- Code fences for anything verbatim (paths, commands, JSON).
- Inline `code` for short identifiers.
- No em-dashes. Use commas, parentheses, or split sentences.

## Where files land

- A new deliverable lands in the relevant domain folder under `<domain>/projects/<project-name>/`.
- A reference document lands in `<domain>/_claude/context/`.
- A skill lives in `<domain>/_claude/skills-local/<skill-name>/SKILL.md` or, after promotion, in the domain plugin.
- A planning artifact lands in `<domain>/projects/<project-name>/_planning/`.
- A handoff or briefing lands in `<domain>/projects/<project-name>/_overview/`.

## When the user gives a different format

Defer to the user. These are defaults, not constraints. If a client requires a specific filename pattern, use the client pattern. If a domain has a long-standing internal convention that conflicts with a default here, the domain wins until the Domain Steward consolidates with the Working Group.
