# Agent Skills Format

Source: [Agent Skills specification](https://agentskills.io/specification), checked 2026-09-12. The standard defines the package format, not universal discovery paths, slash commands, or permission enforcement.

## Portable frontmatter

Start `SKILL.md` with `---` on the first line, a YAML mapping, a closing `---`, then Markdown instructions. Use UTF-8 and keep both required fields explicit, even when a host accepts defaults.

| Field | Status | Portable constraint |
| --- | --- | --- |
| `name` | Required | 1–64 characters; lowercase alphanumeric with single hyphen separators; match the containing directory |
| `description` | Required | Non-empty string, 1–1024 characters; describe both capability and when to use it |
| `license` | Optional | License name or reference to a bundled license file; do not invent licensing terms |
| `compatibility` | Optional | Non-empty string, 1–500 characters if present; actual environment requirements |
| `metadata` | Optional | Map of string keys to string values; quote versions, dates, or numeric identifiers |
| `allowed-tools` | Experimental | Space-separated string of pre-approved tools; support and tool names vary by host |

Use the conservative ASCII name pattern `^[a-z0-9]+(-[a-z0-9]+)*$` for cross-agent compatibility. Valid: `api-review`, `pdf2text`. Invalid: `API-review`, `-review`, `review--api`, or a name different from its directory.

The specification's detailed name text mentions Unicode lowercase alphanumeric characters, but its summary and some hosts are narrower. The ASCII subset avoids relying on that ambiguity.

Use a real YAML parser. Quote scalars containing YAML-significant text, such as a colon followed by a space or a space followed by `#`, or use a folded block (`>-`) for descriptions. Reject duplicate keys and unexpected value types; an ordinary YAML parser may silently accept duplicate keys unless configured otherwise. Check lengths on parsed strings, not source lines.

Do not put host-only fields such as `model`, `context`, `agent`, `hooks`, or `disable-model-invocation` into a supposedly universal header. They may be ignored, rejected, or interpreted differently. `allowed-tools` is part of the standard but experimental, not a portable security boundary. Omit it unless the selected host needs it and its behavior is verified.

## Minimal starter

Adapt this example into `<skill-name>/SKILL.md`. Replace placeholders and remove sections that do not apply; the placeholders below are not a finished skill.

````markdown
---
name: skill-name
description: "<Capability and concrete input/artifact>. Use when <specific task or request>."
---

# Skill Name

## Workflow

1. Confirm <required input> and <prerequisite>. If missing, <safe next step>.
2. Perform <specific procedure>, using <verified tool or convention>.
3. Save <output artifact> to <agreed destination>.
4. Verify <observable success condition>; report <useful result or blocker>.

## Example

- Input: <representative request or fixture>.
- Output: <expected artifact, structure, or result>.

## Boundaries

- Do not <consequential action> without <required approval>.
- If <likely failure>, <recovery action or stopping condition>.
````

## Resource conventions

```text
skill-name/
├── SKILL.md
├── references/       # Optional, read on demand
├── scripts/          # Optional, executable helpers
└── assets/           # Optional, output templates or data
```

These directory names are conventions, not a closed schema. Existing `resources/` folders need not be renamed just for style. A host may define extra files, for example Codex's `agents/openai.yaml`; those are not required by the standard.

- Keep essential execution and safety instructions in the main file.
- Link references directly from `SKILL.md`; avoid chains of references that require loading unrelated material.
- Give each optional reference a specific purpose and a “read when” instruction.
- Keep executable helpers separate from large reference text; scripts still need inspection before execution.
- Do not include generated caches, real credentials, or unrelated project files in the distributed package.
- Preserve upstream copyright and license files when adapting third-party content.

## Format versus quality

The body has no mandated heading schema. Under 500 lines and roughly 5,000 tokens is guidance for progressive loading, not a parser requirement. A short skill still needs executable steps, useful boundaries, and a verifiable output; a long skill should move optional detail out of the common path rather than delete essential safeguards.
