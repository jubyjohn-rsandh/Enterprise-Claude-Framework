# [Domain Name] - Domain Rules

This file is auto-loaded when the working directory is inside the `[domain-name]/` folder. It extends the firm-wide rules in `RSandH-Claude/CLAUDE.md`. The Domain Steward maintains this file.

> **Domain Steward:** Replace every bracketed `[placeholder]` with the actual content for this domain. Delete this blockquote when done.

## Identity

[One or two sentences on what this domain does. Who it serves. What its core mission is.]

Example for Tech: "Technology covers IT, security, digital, data, apps, and emerging software development. The Tech domain is the framework's pilot. Patterns proven here propagate to other domains."

## Scope

What is in this domain:

- [Workstream 1]
- [Workstream 2]
- [Workstream 3]

What is NOT in this domain (handle in another domain):

- [Boundary 1]
- [Boundary 2]

## Key tools and systems

| Tool | How it is used here |
|---|---|
| [Tool 1, for example: Microsoft Fabric] | [Notes on which workspaces, lakehouses, semantic models] |
| [Tool 2] | [Notes] |
| [Tool 3] | [Notes] |

## Key reports, deliverables, and artifacts

- [Report 1: name, purpose, audience, refresh cadence]
- [Report 2]
- [Report 3]

## Key stakeholders

| Role | Who | What they need |
|---|---|---|
| [Role 1] | [Name or team] | [What they consume from this domain] |
| [Role 2] | [Name or team] | [What they consume] |

## Domain-specific conventions

Beyond the org-wide rules, this domain follows:

- [Code style addition, if any]
- [Documentation pattern specific to this domain]
- [Vocabulary: domain-specific terms and their meanings]
- [Data classification or handling that is stricter than the org default]

## Common workflows

The most frequent jobs in this domain:

1. [Workflow 1: what it produces, what triggers it]
2. [Workflow 2]
3. [Workflow 3]

If any of these are repeated more than five times per quarter, consider promoting to a slash command or skill.

## Memory pointers

- Auto-loaded: `_claude/memory/shared.md` (curated by the Steward, distilled monthly)
- Archive: `_claude/memory/archive/<YYYY-Q[N]>.md` (append-only)
- Personal: `_claude/memory/users/<id>.md` (local only, not synced)

## Related context files

- `_claude/context/[domain]-overview.md` - full identity statement, audience, history
- `_claude/context/data-handling.md` - domain-specific data classification
- `_claude/context/[domain-stack].md` - tool inventory and integration notes

## Skills

- Local skills (not yet promoted): `_claude/skills-local/`
- Plugin-distributed skills: installed via `rsandh-[domain]-plugin`

## Cross-references

- Org rules: `RSandH-Claude/CLAUDE.md` (loads automatically)
- Brand voice: `RSandH-Claude/_org/context/brand-voice.md`
- Security: `RSandH-Claude/_org/context/security-and-confidentiality.md`
- File naming: `RSandH-Claude/_org/context/file-naming-and-output-standards.md`

## Domain Steward

- **Name:** [Steward name]
- **Contact:** [email or Teams handle]
- **Backup:** [name of secondary contact when steward is out]
- **Hotfix path:** [how to escalate urgent fixes to a skill or hook in this domain]
- **Review cadence:** Monthly memory distillation. Quarterly skill audit. Annual full review of this file.
