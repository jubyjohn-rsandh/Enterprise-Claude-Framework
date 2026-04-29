# Claude Code Framework Primitives — RS&H Reference

**Audience:** RS&H Claude Code users at every level. New licensees, domain stewards, the Working Group, and leadership orienting to the framework.
**Purpose:** Define the building blocks of how Claude Code works at RS&H, in a single document, so anyone can read it once and have a shared vocabulary.
**Voice:** Direct, professional, RS&H-internal.

---

## Table of Contents

1. [The mental model: WAT](#the-mental-model-wat)
2. [Tool](#tool)
3. [Agent](#agent)
4. [Workflow](#workflow)
5. [Script](#script)
6. [Skill](#skill)
7. [Hook](#hook)
8. [Memory and CLAUDE.md](#memory-and-claudemd)
9. [Plugin](#plugin)
10. [The GSD pattern](#the-gsd-pattern)
11. [How they fit together — concrete example](#how-they-fit-together-a-concrete-example)
12. [Visual hierarchy](#visual-hierarchy)
13. [Glossary](#glossary)

---

## The mental model: WAT

Claude Code is composed of three primitives that combine to do useful work.

| Primitive | What it is | One-line summary |
|---|---|---|
| **Workflow** | A repeatable parameterized procedure | The recipe |
| **Agent** | A specialized role with isolated context | The worker |
| **Tool** | A capability Claude can call | The verb |

To these, three pieces of glue:

| Glue | What it is | One-line summary |
|---|---|---|
| **Skill** | On-demand knowledge or workflow | The reference |
| **Hook** | Deterministic automation around events | The policy |
| **Memory** | Persistent context across sessions | The institutional knowledge |

Plus the distribution wrapper:

| Wrapper | What it is | One-line summary |
|---|---|---|
| **Plugin** | A bundled, distributable package | The container |

When someone at RS&H says "is that a workflow, an agent, or a tool?" they're using the WAT lens to figure out what kind of thing they're looking at. This document defines each.

---

## Tool

**Definition:** A specific capability Claude can call. Tools are atomic. They do one thing and return a result.

**Examples:**

| Tool | What it does |
|---|---|
| `Read` | Read a file from disk |
| `Write` | Create a new file |
| `Edit` | Modify an existing file |
| `Bash` | Run a shell command |
| `Grep` | Search file contents |
| `WebFetch` | Fetch a URL |
| `Glob` | Find files by pattern |

**Three sources of tools:**

1. **Built-in.** The ones above. They ship with Claude Code.
2. **MCP servers.** Extensions added via the Model Context Protocol. A SharePoint MCP gives Claude tools to read intranet docs. A Microsoft Fabric MCP gives Claude tools to query semantic models. RS&H decides which MCP servers are approved via the managed-settings allowlist.
3. **Custom.** Tools built via the Claude Agent SDK. Reserved for advanced use cases.

**Mental model:** Tool = a verb. What Claude *can do*. The hands.

**RS&H example:** When Claude opens `RSH-Claude/finance/_claude/context/finance-overview.md`, that's the `Read` tool firing. When Claude searches the runbook index for "FDOT," that's `Grep`.

---

## Agent

**Definition:** A specialized role with its own context window and its own tool access. An agent is essentially a sub-Claude with focused instructions.

**Two types:**

1. **Main agent.** Your primary Claude Code session. What you talk to.
2. **Sub-agent.** Invoked by the main agent for a specific job. Runs in a *fresh* context window. Reports a summary back when done.

**Why use sub-agents:**

- **Context isolation.** Exploration and research don't pollute the main thread. The main session stays lean.
- **Specialization.** Each agent has its own system prompt and only the tools it needs. A `security-reviewer` agent only gets `Read` and `Grep`, no `Write` or `Bash`.

**Examples in the RS&H Tech plugin (gsd-lite pattern):**

| Agent | Role |
|---|---|
| `researcher` | Explores code, runbooks, or data and returns findings. Read-only. |
| `planner` | Produces an implementation plan from a problem statement. |
| `checker` | Verifies a plan meets requirements before execution. |
| `verifier` | Confirms work meets stated requirements after execution. |

**Mental model:** Agent = a specialized worker. *Who* is doing the job. A second brain in a separate office.

**RS&H example:** A Tech user asks "what does the Application Matrix dashboard say about asset aging?" Instead of reading the report and clogging the main session, Claude spins up a `researcher` agent. The agent queries Fabric (via MCP, in Phase 2), summarizes, and returns 200 words. The main session never sees the raw report.

---

## Workflow

**Definition:** A parameterized procedure. A recipe Claude follows when invoked.

**In Claude Code:** workflows are slash commands. They live in `.claude/commands/<name>.md` (or are bundled in a plugin's `commands/` folder).

**How they fire:**

1. User types `/X arg`.
2. Claude reads the matching command file.
3. The recipe executes. It can call tools, invoke agents, reference skills, anything.

**Examples in the RS&H Tech plugin:**

| Slash command | What it does |
|---|---|
| `/adr "Postgres 16 upgrade"` | Produces a structured Architecture Decision Record |
| `/postmortem` | Walks the incident postmortem template |
| `/handoff` | Generates an end-of-shift handoff doc |
| `/change "AD migration"` | Drafts a Tech change request |
| `/runbook "Provision new VM"` | Turns ad-hoc steps into a structured runbook |

**Mental model:** Workflow = the recipe. *How* to do a specific common task.

---

So far: **Tools = verbs (what)**, **Agents = workers (who)**, **Workflows = recipes (how).**

Now the supporting concepts.

---

## Script

**Definition:** A file on disk (Python, bash, PowerShell) that performs deterministic computation when run.

**Important distinction from "tool":**

- A *tool* is a primitive Claude calls during reasoning (`Read`, `Edit`, `Bash`).
- A *script* is a *file*. Claude executes a script *via the Bash tool*. The script isn't a tool itself — it's something Claude *runs* using a tool.

**Why scripts matter:**

When you want determinism, don't ask Claude to compute or judge. Have Claude run a script. Math, lookups, validations, formatting, anything where the answer is deterministic should be a script the skill calls, not Claude doing it in its head.

**Examples where scripts are the right answer:**

- Calculating financial ratios from a P&L. Don't ask Claude to do arithmetic on currency. Use a script.
- Validating that a data spec references real semantic models. Don't ask Claude to remember the model list. Use a script that queries the catalog.
- Pre-prompt PII scanning. Don't ask Claude to be careful. Use a regex script that runs deterministically.
- Generating a runbook step that fetches the current state of an asset (e.g., a server's status). Don't ask Claude to guess. Use a script that hits the live API.

**Where scripts live:**

Inside a skill's `scripts/` subfolder. Inside a hook (which is itself a script). Or as a one-off invocation in any session.

---

## Skill

**Definition:** On-demand knowledge or a workflow that Claude pulls in when relevant.

**Where it lives:** `.claude/skills/<name>/SKILL.md` (the file is at the root of a folder named after the skill).

**Two flavors:**

1. **Knowledge skill.** "Here's our review rubric." "Here's our brand voice." Auto-surfaced when Claude detects relevance from the description in the YAML frontmatter.
2. **Workflow skill.** "Here's how to write an ADR." Invoked explicitly (`/skill-name`) or auto-invoked by Claude based on the description.

**Skills can include:**

- A `references/` subfolder (longer documents Claude pulls when needed).
- A `scripts/` subfolder (deterministic Python or bash Claude runs).
- An `assets/` subfolder (templates, sample data, expected outputs).

**Skill structure:**

```
.claude/skills/incident-postmortem/
├── SKILL.md                    # The instruction file with YAML frontmatter
├── references/
│   └── postmortem-template.md
├── scripts/
│   └── timeline-extractor.py
└── assets/
    └── sample-postmortem.md
```

### Skill vs. workflow (slash command)

The line is fuzzy because skills can BE workflows. The practical difference:

| | Slash command | Skill |
|---|---|---|
| Where | `.claude/commands/<name>.md` | `.claude/skills/<name>/SKILL.md` |
| How invoked | Explicit only — user types `/X` | Explicit AND/OR auto when Claude detects relevance |
| Weight | Lightweight, single file | Heavier, can include references/scripts/assets |
| Best for | Quick parameterized recipes | Domain knowledge + multi-step procedures |

Anthropic is consolidating: **skills are becoming the dominant abstraction**. RS&H's recommendation: build skills as the default, with a few exposed as slash commands for muscle-memory invocation.

### Skill vs. tool

- A tool is atomic (`Read` a file).
- A skill is higher-level. Knowledge or a recipe that *uses* multiple tools.

### Skill vs. agent

- A skill is *content* / instructions Claude reads.
- An agent is a *sub-Claude* that runs in its own context.
- A skill might *tell* Claude "use this approach"; an agent *is* another Claude doing the work.
- A skill can invoke an agent (e.g., the skill says "first run the `researcher` subagent to gather context").

---

## Hook

**Definition:** A script that fires automatically on a specific event. No Claude reasoning involved.

**Events Claude Code recognizes:**

| Event | Fires when |
|---|---|
| `PreToolUse` | Before any tool call (best for policy checks, PII redaction) |
| `PostToolUse` | After any tool call (best for audit logging) |
| `OnPromptSubmit` | When user submits a prompt (best for prompt-level scanning) |
| `OnStop` | When the session ends (best for final audit log entries) |

**Why use hooks instead of skills:**

Hooks are *deterministic*. They fire 100% of the time. Skills are *advisory*. Claude reads them and decides how to apply.

Use a hook when "this MUST happen, no exceptions." Use a skill when "this is how we usually do it; Claude exercises judgment."

**Examples in the RS&H setup:**

- `pre-prompt-pii-scan.sh` — checks the prompt for client identifiers, project codes, or PII patterns. Blocks the prompt if matches found. Fires on every prompt, no exceptions.
- `on-stop-audit-log.sh` — writes session metadata (user, duration, tool calls) to an audit log. Feeds the Anthropic Compliance API → SIEM pipeline.

Hooks have a 5-second timeout so they never block the user's flow. Policy violations are logged, not necessarily preventive.

---

## Memory and CLAUDE.md

**Definition:** Persistent context Claude reads at session start. It's the "always loaded" layer that means you don't have to re-explain who RS&H is in every session.

**CLAUDE.md** is the file that holds always-loaded context. Claude Code reads it at session start. Anthropic's prompt cache holds it for ~5 minutes, so subsequent turns in the same session read it at ~10% of the original input cost.

**Hierarchical loading:** When a user opens Claude Code in `RSH-Claude/finance/`, Claude Code walks up the directory tree and auto-loads BOTH:

1. `RSH-Claude/CLAUDE.md` (the org-wide rules)
2. `RSH-Claude/finance/CLAUDE.md` (the Finance domain rules)

The user doesn't have to consciously "be at the root" for org rules to apply. They just open Claude wherever their work is, and the layered context arrives for free.

**Three context layers:**

| Layer | Where | Loaded when | Owner |
|---|---|---|---|
| **Org** | `RSH-Claude/CLAUDE.md` | Every session in any subfolder | Working Group |
| **Domain** | `<domain>/CLAUDE.md` | When cwd is inside that domain | Domain Steward |
| **User** | `~/.claude/CLAUDE.md` | Every session for that user | The user |

**Rule:** RS&H content never lives in `~/.claude/CLAUDE.md`. User-level config is for personal preferences only (verbosity, default model). Anything organizational is repo-tracked, reviewed, versioned, and synced to all licensees via OneDrive.

**Memory at scale (three tiers):**

A single append-only `memory.md` doesn't scale to 50+ users per domain. RS&H uses three tiers:

```
<domain>/_claude/memory/
├── shared.md              # ~500 lines max, curated, distilled — auto-loaded
├── archive/
│   ├── 2026-Q2.md         # raw append-only, current quarter
│   └── ...
└── users/                 # personal scratchpad, NOT synced via OneDrive
    └── <user>.md          # local-only
```

| Tier | Edit rights | Loaded when | Cadence |
|---|---|---|---|
| `shared.md` | Domain Steward only (review-gated) | Auto, every session | Monthly distillation |
| `archive/<quarter>.md` | Anyone in domain (append) | On-demand via `/memory-search` | Daily appends, rolled quarterly |
| `users/<id>.md` | Owner only | Personal sessions | As wanted |

**Format for every memory entry:**

```
[YYYY-MM-DD] [Person] — [What] — [Why it matters]
```

---

## Plugin

**Definition:** A package that bundles skills, slash commands, hooks, agents, and MCP servers into one installable unit.

**Why plugins exist:** at 30+ licenses, you can't have every user copy-pasting skills from a shared folder. Plugins are the distribution mechanism. Update once, all 30 users improve.

**RS&H ships two plugins:**

1. **`rsh-org-plugin`** — cross-domain capability. Skills like `exec-memo`, `comms-tone-check`, `meeting-notes`. Installed by every licensee.
2. **`rsh-tech-plugin`** — Tech-specific capability. The 8 starter skills (`change-request`, `architecture-decision-record`, `incident-postmortem`, etc.), 5 slash commands, 4 sub-agents (gsd-lite), 2 hooks (PII scan, audit log). Installed by Tech members; available to other domains by request.

**Distribution path:**

1. Plugin source lives in private GitHub.
2. Plugin is published to RS&H's private Claude Enterprise marketplace.
3. Marketplace auto-installs to provisioned users (or users install on demand).
4. Updates ship as new plugin versions; users get them automatically.

**Trade-off versus loose files:** plugins are heavier to author and update than dropping a SKILL.md into a shared folder. The trade is worth it past ~10 users — quality stops drifting.

---

## The GSD pattern

**GSD** (Get Shit Done) is an open-source meta-prompting framework for Claude Code. Its core insight: **context rot.** The longer a Claude session runs, the worse it gets, because the context window fills with stale exploration, failed attempts, and noise. GSD solves this by orchestrating fresh sub-agents (Researcher → Planner → Checker → Executor → Verifier), each in an isolated context window, while the main session stays at 30-40% capacity acting as a coordinator.

**What RS&H adopts: gsd-lite.**

The discipline, not the bulk. Full GSD installs 86 skills and 33 sub-agents — too heavy as a default for non-engineers in IT, Data, or Apps. RS&H's `rsh-tech-plugin` includes a lighter version:

- Four sub-agents: `researcher`, `planner`, `checker`, `verifier`.
- The discipline of "main session is a coordinator; sub-agents do the heavy lifting in fresh contexts."
- The phase artifacts (research → plan → execute → verify) for complex work.

Full GSD remains available as an opt-in for software-development users who want it.

---

## How they fit together — a concrete example

A Tech user types `/postmortem` after an outage:

1. The slash command (a **workflow**) lives in `rsh-tech-plugin/commands/postmortem.md`.
2. It activates the `incident-postmortem` **skill** in the same plugin.
3. The skill says: *"First use the `researcher` agent to gather the timeline from logs and chat history. Then walk through the postmortem template. Save the result to `technology/_claude/memory/archive/2026-Q2.md`."*
4. The `researcher` **agent** runs in its own isolated context. It uses **tools** (`Read`, `Grep`, `Bash`) to find log files and ticket data. It returns a 300-word summary.
5. The main Claude takes that summary, fills the postmortem template (using **memory** loaded from `incident-response.md` for format conventions), produces the postmortem doc.
6. A **hook** (`on-stop-audit-log.sh`) fires when the session ends and writes a session audit log entry to RS&H's SIEM.

All of this works because:

- The **plugin** is installed via the Claude Enterprise marketplace.
- The user's **CLAUDE.md** files (org-wide and Tech domain) loaded at session start told Claude what RS&H Tech is.
- The skill carried its own reference material in its folder (the postmortem template, the `references/` subfolder).
- The hook fires regardless of what Claude does.

The user typed five characters. The framework did the rest.

---

## Visual hierarchy

```
PLUGIN (distribution package, ships via Claude Enterprise marketplace)
  │
  ├── SKILLS (knowledge + recipes, on-demand)
  │     └── may invoke agents, may be exposed as slash commands
  │
  ├── SLASH COMMANDS (explicit /workflows)
  │
  ├── AGENTS (specialized sub-Claudes for context isolation)
  │
  ├── HOOKS (deterministic automation, not Claude reasoning)
  │
  └── MCP servers (external tool integrations → expose new tools)

TOOLS (atomic capabilities Claude calls — Read, Edit, Bash, etc.)
        ↑ called by skills, agents, workflows alike

CLAUDE.md (always-loaded context, layered: org → domain → user)
MEMORY (persistent context across sessions, three tiers within each domain)
```

The whole point: **tools are the hands, agents are the brains, workflows are the recipes, skills are the knowledge, hooks are the policy, plugins are the package, and CLAUDE.md is the always-loaded foundation.**

---

## Glossary

**Agent** — A specialized role with its own context window and tool access. Either the main session or an invoked sub-agent.

**CLAUDE.md** — A markdown file that Claude Code reads at session start. Holds always-loaded context. Loads hierarchically (parent + child).

**GSD** — Get Shit Done. An open-source framework for Claude Code that uses sub-agents in isolated contexts to prevent context rot. RS&H adopts the discipline as "gsd-lite."

**Hook** — A script that fires automatically on a specific Claude Code event. Deterministic, no reasoning. Used for policy enforcement and audit logging.

**MCP** — Model Context Protocol. The standard by which external tools are added to Claude. An MCP server exposes new tools (e.g., the Microsoft Fabric MCP exposes tools to query semantic models).

**Memory** — Persistent context across sessions. Tiered: distilled `shared.md` (auto-loaded), quarterly archives (on-demand), per-user scratchpad (private).

**Plugin** — A bundle of skills, slash commands, hooks, agents, and MCP servers. The distribution mechanism for org-wide capability.

**Script** — A file (Python, bash) that performs deterministic computation. Claude runs scripts via the `Bash` tool. Used inside skills and hooks to avoid asking Claude to compute when an algorithm should.

**Skill** — On-demand knowledge or workflow. Lives in `.claude/skills/<name>/SKILL.md`. Two flavors: knowledge (auto-surfaced) and workflow (invoked).

**Slash command** — An explicit user-typed workflow (e.g., `/adr`). Lives in `.claude/commands/<name>.md`.

**Sub-agent** — An agent invoked from the main session. Runs in a fresh context window. Returns a summary.

**Tool** — A primitive capability Claude can call (Read, Edit, Bash, etc.). Atomic. Built-in, MCP-provided, or custom.

**WAT** — Workflow / Agent / Tool. The mental model for understanding what kind of thing you're looking at in Claude Code.

**Workflow** — A parameterized procedure. In Claude Code, slash commands.
