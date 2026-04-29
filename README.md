# Enterprise Claude Framework

A structured way to govern, distribute, and scale Anthropic's Claude Code across an enterprise. Built initially for RS&H (architecture, engineering, consulting). The structure is reusable for any firm rolling Claude Code out beyond a handful of users.

## What's in this repo

| Path | Purpose |
|---|---|
| [`_overview/judy-framework-overview.html`](_overview/judy-framework-overview.html) | Executive overview document. Self-contained HTML, prints clean. The full leadership brief covering architecture, rollout, risks, and Phase 2 (Microsoft Fabric integration). |
| [`RSandH-Claude/`](RSandH-Claude/) | The deployable framework. Org-level rules and shared context files that auto-load every session. |
| [`RSandH-Claude/CLAUDE.md`](RSandH-Claude/CLAUDE.md) | Firm-wide rules. Loaded at the start of every session. |
| [`RSandH-Claude/_org/context/`](RSandH-Claude/_org/context/) | Cross-org context: brand voice, security and confidentiality, file naming and output standards. |
| [`RSandH-Claude/_org/templates/`](RSandH-Claude/_org/templates/) | Starter scaffolds. Domain CLAUDE.md template (one per domain). Skill template (one per new skill). |
| [`_learning/claude-framework-primitives.md`](_learning/claude-framework-primitives.md) | Long-form explainer of the WAT mental model (Workflow, Agent, Tool) and every framework primitive: skill, hook, plugin, memory, CLAUDE.md. |
| [`.claude/skills/`](.claude/skills/) | Reusable skills: `html-developer`, `technical-writer`, `document-reviewer`, `diagram-builder`. |

## How the framework works

Three context layers auto-load every session:

| Layer | Where | Loaded when | Owner |
|---|---|---|---|
| **Org** | `CLAUDE.md` at repo root | Every session anywhere in the framework | Working Group |
| **Domain** | `<domain>/CLAUDE.md` | When working inside that domain folder | Domain Steward |
| **User** | `~/.claude/CLAUDE.md` | Every session for that user | The user |

Skills, scripts, and templates load on demand. Settings and hooks enforce policy without burning a Claude turn. The result is that users open Claude Code, type their actual question, and the framework provides the rest.

For the full architecture, the seven domains, the access model, the phased rollout plan, the failure modes from other large Claude rollouts, and the Phase 2 Microsoft Fabric integration plan, see the [executive overview](_overview/judy-framework-overview.html).

## Status

Phase 0 (Foundations). Spec and templates ready. Leadership review before Phase 1 build kickoff.

## Voice and writing rules

The framework enforces:

- No em-dashes
- No "leverage" as a verb
- No marketing language
- Active voice
- Short sentences
- Define jargon on first use

These rules apply to every artifact produced inside the framework. The full voice spec is at [`RSandH-Claude/_org/context/brand-voice.md`](RSandH-Claude/_org/context/brand-voice.md).

## Security baseline

The framework assumes managed settings deployed via Intune. The non-negotiable setting at enterprise scale is `disableBypassPermissionsMode: true`, which prevents any user from running `--dangerously-skip-permissions` and bypassing every guardrail. Full policy at [`RSandH-Claude/_org/context/security-and-confidentiality.md`](RSandH-Claude/_org/context/security-and-confidentiality.md).
