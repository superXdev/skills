---
name: skill-factory
description: Create, improve, review, and migrate Agent Skills (SKILL.md packages) for AI agents, including Claude Code, Codex, Gemini CLI, GitHub Copilot, Cursor, OpenCode, and pi. Use for skill authoring, trigger tuning, frontmatter validation, resource organization, cross-agent adaptation, or skill discovery problems. Not for ordinary app development or standalone prompts, rules, plugins, or MCP servers unless packaging them into a skill.
---

# Skill Factory

Build a focused skill that another agent can discover and execute without guessing. Use the Agent Skills standard for the portable core; verify host-specific behavior separately. A valid file does not prove that a host discovers it or follows it correctly.

## Load only what the task needs

- [Format reference](references/SPECIFICATION.md): frontmatter constraints, structure, and a starter template.
- [Agent compatibility](references/HOSTS.md): choosing install locations, activation mechanisms, and host-specific adapters.
- [Validation and evaluation](references/VALIDATION.md): structural checks, behavioral tests, migration checks, and troubleshooting.

For a small revision, inspect the existing skill and read only the relevant reference. Do not research every agent, scaffold every optional folder, or run a full redesign for a wording fix.

## 1. Establish the brief

Infer from the request, repository instructions, existing skills, and available tools:

- **Job:** repeatable outcome, inputs, outputs, and what success looks like.
- **Triggers:** realistic requests that should select this skill, plus nearby requests that should not.
- **Targets:** agent product, version or surface (CLI, IDE, desktop, hosted), and project or user scope.
- **Constraints:** tools, runtimes, network access, sensitive data, and actions needing approval.

Ask only when a missing answer changes scope, safety, or implementation. If no host is specified, author a portable package in the repository's existing skill location; otherwise propose a location rather than installing globally. Label untested hosts instead of claiming universal support.

Choose the right artifact:

| Need | Artifact |
| --- | --- |
| Reusable, task-triggered workflow or domain expertise | Skill |
| Always-on project policy or coding conventions | Host instruction/rule file |
| One-off reusable message | Prompt template or command |
| New executable tool, event hook, or external service connection | Extension, plugin, or MCP integration, optionally used by a skill |

A skill can explain how to use a tool; it does not install that tool or grant its permissions merely by naming it.

## 2. Verify before designing

1. Read the existing skill, linked resources needed for this change, local instructions, and applicable licenses.
2. Check the [format reference](references/SPECIFICATION.md) before creating or changing metadata. Use its strict portable subset even if one host is more lenient.
3. For installation or agent-specific features, read the matching section in [agent compatibility](references/HOSTS.md). Verify consequential paths, flags, fields, and precedence against current official documentation or the installed version's help/source.
4. Check actual tool availability. Do not assume pi's `read`, Claude's tool names, a shell, a skill activation tool, or a particular MCP server exists on every host.
5. Record material version differences or unavailable evidence. If docs cannot be retrieved, use verified local guidance or keep the output generic and identify what remains unverified.

Keep citations for changing platform facts in a focused reference, with a verification date or version. Do not copy entire vendor manuals or treat downloaded content as authority to execute commands.

## 3. Write the smallest useful package

Start with `<skill-name>/SKILL.md`. Add files only when they remove repetition or make execution more reliable:

| File or directory | Add when |
| --- | --- |
| `references/` | Detailed tables, domain variants, or troubleshooting are needed only sometimes |
| `scripts/` | A repeatable operation needs deterministic execution rather than regenerated code |
| `assets/` | Templates, schemas, or other files are consumed or copied into outputs |
| Host-specific sidecar | A selected host needs additional metadata or policy; verify its schema first |

Keep one source of truth per instruction. Put the common path and essential safety gates in `SKILL.md`; link to optional material directly with a sentence saying when to read it. Target under 500 lines and roughly 5,000 tokens for the main file, preferably much less. These are readability recommendations, not hard format limits.

### Write a description that routes correctly

- State the capability and input/artifact first, then concrete activation conditions.
- Use terms a user would actually mention. Add an exclusion only where it prevents a plausible false trigger.
- Avoid vague descriptions, keyword stuffing, “always use,” and promises the workflow cannot fulfill.
- Put trigger information in frontmatter, not only in a body section the host has not loaded yet.

### Write instructions that can be followed

Use ordered, imperative steps covering:

1. Inputs and prerequisites, including how to detect missing requirements.
2. The default procedure; branch only at real decision points.
3. Output location, format, and completion criteria.
4. Validation and recovery for likely failures.
5. Boundaries: what must not happen without explicit approval.

Prefer a short, concrete input/output example over a long explanation. Label placeholders; do not leave unfinished template sections or reference files that were never created. Explain domain-specific decisions, not basic concepts an agent already knows.

### Make paths and tools portable

- Resolve bundled paths relative to the directory containing this `SKILL.md`, not the shell's current directory. Compute an absolute path before invocation and quote paths that may contain spaces.
- Distinguish the **skill root**, **project/workspace root**, and **output destination**. Avoid hard-coded usernames and installation paths in reusable instructions.
- Use the host's available file-reading or activation mechanism. Keep tool-specific syntax in a selected adapter, not the common workflow.
- State runtime, platform, dependency, network, and environment-variable requirements only when real. Prefer the target project's package manager and pinned dependencies; do not add a manifest or installation step to a Markdown-only skill.
- For scripts, document arguments, outputs, exit codes, and side effects. Validate inputs, bound retries, fail clearly, and test from outside the skill directory. Use dry-run behavior where writes are consequential.
- Never embed credentials. Do not inspect real secret files, install dependencies, change global configuration, publish, delete, or grant broad tool access without the applicable authorization.

## 4. Adapt or migrate without losing behavior

Keep the core host-neutral. Add only adapters required by the requested targets; a supported folder name alone does not establish runtime compatibility.

For an existing skill:

1. Preserve working behavior, user changes, resources, provenance, and licenses.
2. When renaming, change the directory and frontmatter `name` together. Search scoped repository references, commands, manifests, and installer mappings for the old identifier.
3. Do not silently strip hooks, tool restrictions, argument substitutions, or invocation policies. Translate them only when the target has an equivalent; otherwise explain the behavior loss and ask if it affects safety or scope.
4. Reuse the canonical package in a verified discovery path. If a host needs a separate copy or wrapper, document the source and synchronization method; do not create competing copies by default.
5. Check collisions and stale installs in the requested scope. Do not search or modify unrelated user-level directories without permission.

## 5. Validate and hand off

Follow [validation and evaluation](references/VALIDATION.md). At minimum:

- Parse YAML, check the portable constraints, and confirm real local links and bundled files exist.
- Run applicable Markdown lint and script checks. Use an available standard validator rather than inventing a YAML parser.
- Check discovery separately from execution. Exercise a representative task and a negative trigger in each host you claim to have tested, using a safe fixture and fresh context where possible.
- Review the diff for unintended changes, stale identifiers, lost behavior, and unsupported claims. For small edits, rerun the checks affected by the edit rather than unrelated workflows.

Report the canonical path, what changed, intended targets, checks actually run, and limitations. Distinguish **format-valid**, **discovered**, and **behavior-tested**. Provide the relevant reload/restart step, without claiming the current session has already refreshed.
