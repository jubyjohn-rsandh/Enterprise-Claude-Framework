# Security and Confidentiality

This file is loaded at the start of every RS&H session. It sets the rules for what data can appear in prompts, memory, and skill outputs. When in doubt, escalate to the Working Group before submitting the prompt.

## Out-of-scope data

The following data must not appear in Claude Code prompts, memory entries, or skill outputs:

- **Federally regulated data.** CUI (Controlled Unclassified Information), ITAR-controlled technical data, FedRAMP-protected workloads, and any data governed by an FDOT or federal Data Use Agreement.
- **Client confidential data** unless the engaging client has authorized AI processing in writing.
- **PII** (Social Security numbers, home addresses, dates of birth, drivers license numbers, financial account numbers) of associates, clients, or third parties.
- **Credentials.** API keys, passwords, certificates, private keys, OAuth tokens, connection strings with embedded secrets. Never paste a credential into a prompt.
- **Healthcare data.** Claude Code is not covered under Anthropic's HIPAA-ready Enterprise plan today. Only Claude chat is. Healthcare data belongs in a HIPAA-covered tool.

## Pre-prompt PII scanning

The `pre-prompt-pii-scan.sh` hook fires before every prompt. It blocks prompts that contain client identifiers, project codes, or PII patterns. If the hook blocks:

1. Do not work around the block.
2. Remove the sensitive content and resubmit, or
3. Escalate to the Working Group if the data is needed for the task.

## Memory hygiene

Before appending to a memory archive, check for:

- PII or client identifiers
- Federally regulated data
- Credentials
- Specific project codes that could identify a client

The `memory-append` skill runs a PII scan before write. If the scan flags content, do not bypass it.

## Audit trail

Every session is audited via the `on-stop-audit-log.sh` hook. The log feeds the Anthropic Compliance API and routes to RS&H's SIEM (Splunk or Datadog). Assume every session is reviewable.

## When you find a problem

If you discover regulated data in a prompt, memory file, or skill output:

1. Stop work.
2. Notify the Working Group.
3. Do not redact silently. Do not delete the file.

The Working Group needs to assess scope before remediation.

## Federal and FDOT context

RS&H performs federal and FDOT engineering work. If a domain is asking Claude Code to help with federally regulated content:

1. Check the data classification before submitting any prompt.
2. If unsure, escalate. One extra question is cheaper than one compliance breach.
3. The Working Group has standing authority to decide whether a data class is in scope.

## Permission posture

The managed settings (deployed via Intune) enforce:

- `disableBypassPermissionsMode: true` - users cannot run `--dangerously-skip-permissions`. Non-negotiable at the firm level.
- `allowManagedMcpServersOnly: true` - only approved MCP servers can run.
- Deny list: `curl`, `wget`, reads of `.env`, `~/.ssh`, `~/.aws`, `git push --force`.
- Ask list: `Edit`, `Write`, `Bash` outside the allowlist.
- Allow list: read-only tools, scoped Bash (`git status`, `git diff`, `npm test`, `pytest`).

If a permission prompt appears unexpectedly, that is the system working as intended. Do not attempt to disable or work around it.

## Escalation

| Issue | Contact |
|---|---|
| Compliance question, data classification | Working Group, [contact placeholder] |
| Suspected data leakage in a prompt or memory | Working Group + IT Security |
| Permission prompt that seems wrong | Working Group |
| Anthropic platform issue (Compliance API, marketplace) | Working Group |
| Federal contract data classification | RS&H Legal + Working Group |

Working Group, IT Security, and Legal contacts are confirmed during Phase 1 build.
