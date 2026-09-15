# Agent Compatibility

Read only the section for the requested target. This is an authoring and installation reference, not a claim that every listed host has been runtime-tested. Official web sources were checked 2026-09-12; pi details were also checked against installed version **0.84.4**. Recheck version-sensitive behavior before relying on it.

## Choose the product and surface

“Claude,” “Codex,” and “Copilot” can mean different desktop, CLI, IDE, or hosted surfaces. A local filesystem path does not install a skill in a hosted service. Confirm the actual surface and available execution tools before promising support.

The table lists documented local roots, not exhaustive search order. Put a skill at `<root>/<name>/SKILL.md`. Prefer one canonical location recognized by the selected hosts; do not install into every row.

| Host | Project roots | User roots | Discovery or activation check |
| --- | --- | --- | --- |
| Claude Code | `.claude/skills/` | `~/.claude/skills/` | `/name` or matching request |
| Codex CLI / IDE | `.agents/skills/` from cwd through repository root | `~/.agents/skills/` | `/skills`, `$name`, or matching request |
| Gemini CLI | `.gemini/skills/`, `.agents/skills/` | `~/.gemini/skills/`, `~/.agents/skills/` | `/skills list`; matching request invokes `activate_skill` with consent |
| GitHub Copilot | `.github/skills/`, `.claude/skills/`, `.agents/skills/` | `~/.copilot/skills/`, `~/.agents/skills/` on supported local surfaces | Matching request; inspect the chosen surface's skill listing or use evidence |
| Cursor | `.cursor/skills/`, `.agents/skills/` | `~/.cursor/skills/`, `~/.agents/skills/` | `/` skill picker or matching request |
| OpenCode | `.opencode/skills/`, `.claude/skills/`, `.agents/skills/` | `~/.config/opencode/skills/`, `~/.claude/skills/`, `~/.agents/skills/` | Native `skill` tool; inspect availability and permissions |
| pi | `.pi/skills/`, `.agents/skills/` | `~/.pi/agent/skills/`, `~/.agents/skills/` | `/skill:name` or matching request followed by file read |

`.agents/skills/` is a useful shared root for several hosts, not a universal requirement of the standard. Do not assume Claude Code discovers it without a documented configuration or mapping. Confirm symlink support, synchronization, trust, and duplicate-name behavior for the selected hosts before sharing one folder through multiple paths.

## Claude Code

Source: [Extend Claude with skills](https://code.claude.com/docs/en/skills).

- Use explicit `name` and `description` for portability even though Claude Code accepts defaults.
- `disable-model-invocation: true` restricts the skill to manual invocation. `user-invocable: false` controls user visibility/invocation; it is not the same setting.
- `allowed-tools`, `model`, `context: fork`, `agent`, and `hooks` have host-specific behavior. Verify syntax and permissions before adding them.
- `allowed-tools` grants pre-approval in Claude Code; it is not an allowlist that removes other tools. Preserve explicit confirmation gates and host permission settings.
- `$ARGUMENTS`, `${CLAUDE_SKILL_DIR}`, and inline shell preprocessing are Claude Code features, not standard Markdown interpolation. Do not copy them unchanged into a portable workflow.
- Plugin, nested, enterprise, and synced skills add discovery and naming rules. Check the source for that surface rather than assuming project definitions always win.
- Claude Code extensions to frontmatter may be rejected by Claude web uploads or packaging validators. Treat those as separate targets.

## Codex

Source: [Build skills](https://developers.openai.com/codex/skills/) (redirects to the shared ChatGPT/Codex authoring guide).

- Keep a portable `SKILL.md`; add `agents/openai.yaml` only for needed UI metadata, dependencies, or invocation policy.
- Manual-only selection is configured in the sidecar, not by assuming Claude's frontmatter field works:

```yaml
policy:
  allow_implicit_invocation: false
```

- This permits explicit `$name` use while disabling implicit selection. Preserve other sidecar fields when updating it; verify their schema in current docs.
- Codex documents repository ancestor discovery, user roots, admin skills, system skills, and symlinked skill folders. Do not treat an old `.codex/skills/` recommendation as the only current install location.
- Skill changes are detected automatically in current docs; restart if they do not appear. Same-name skills can both appear rather than merge. Avoid relying on collisions for overrides.
- ChatGPT skill selection and distribution differ from Codex CLI. Do not promise that a local folder alone makes a skill available in ChatGPT web or a plugin catalog.

## Gemini CLI

Source: [Agent Skills](https://geminicli.com/docs/cli/skills/).

- Gemini activates a selected skill through `activate_skill`, with a user consent flow; it does not share pi's `/skill:name` interface.
- Workspace skills outrank user skills, which outrank extension and built-in skills. Within user/workspace tiers, `.agents/skills/` takes precedence over `.gemini/skills/`.
- Use `/skills list` to inspect discovery and `/skills reload` after changes. Use current `gemini skills` help before recommending install/link or scope flags.
- Do not disable consent or change user-level skill settings as a side effect of authoring a project skill.

## GitHub Copilot

Source: [About agent skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills).

- The docs cover cloud agent, code review, CLI, app, and IDE surfaces. Verify feature availability, organization policy, discovery, and allowed tools for the chosen surface.
- Use project skills for repository-shared workflows; a home-directory skill is not automatically present in a remote runner.
- Skills are on-demand workflows. They are not interchangeable with `.github/copilot-instructions.md` or path-specific instruction files.
- Do not assume Claude Code's extra fields, shell preprocessing, or invocation controls work in Copilot merely because `.claude/skills/` is a supported root.

## Cursor

Source: [Agent Skills](https://cursor.com/docs/skills).

- Cursor supports standard skills, manual selection through `/`, and `disable-model-invocation` for explicit-only skills.
- It also documents Claude/Codex-compatible directories. Prefer the established project root instead of creating duplicate installs.
- Nested discovery and file-scoped behavior are host-specific; verify them before using grouping folders or `paths` metadata.
- Local user skills are not automatically available to Cloud Agents, remote SSH sessions, or self-hosted workers. Check the documented sync/distribution mechanism or ship project skills.
- `.cursor/rules/*.mdc` files are rules, not skill packages. Migration must preserve or deliberately change their activation behavior.

## OpenCode

Source: [Agent Skills](https://opencode.ai/docs/skills/).

- OpenCode exposes available skills through the native `skill` tool and loads their bodies on demand.
- It recognizes `name`, `description`, `license`, `compatibility`, and string-valued `metadata`; unknown fields are ignored. A copied `allowed-tools` field is therefore not an access-control mechanism.
- Names must match their directories. Project discovery walks upward to the Git worktree.
- `permission.skill` in OpenCode configuration can allow, deny, or ask for access. A disabled skill tool or denied permission can make a valid skill unavailable. Diagnose this before rewriting the content.

## pi

Sources: [skills](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/skills.md), [settings](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/settings.md), and [packages](https://github.com/badlogic/pi-mono/blob/main/packages/coding-agent/docs/packages.md). Prefer the matching installed package's `docs/` for a pinned version.

- In 0.84.4, project resources require trust. `.agents/skills/` is searched in cwd and ancestors up to the Git root; skill directories are discovered recursively. Root loose Markdown discovery differs by location; use `<name>/SKILL.md` for portability.
- pi reads selected skills with its file-reading tool; it does not require Claude's skill tool. Invoke explicitly with `/skill:name`; use `/reload` after editing.
- `--skill <path>` is repeatable and additive, even with `--no-skills`. It is not `--skills <glob>`.
- The `skills` setting is an array of file/directory paths, not an object with `enabled`, `ignoredSkills`, or `includeSkills`. For example, in project `.pi/settings.json`:

```json
{
  "skills": ["../.claude/skills"]
}
```

- Settings paths resolve from the settings file's directory. Package manifests can supply skills too; do not edit global settings just to validate a local package.
- Claude/Codex-specific roots are not automatically imported by this version; add explicit paths only when needed. Do not teach a universal “later wins” ordering: pi warns on name collisions and keeps the first discovered skill.
- pi permits a directory/name mismatch, unlike the standard. Keep them equal in shared skills anyway. Missing descriptions prevent loading; several other format violations only warn.
- `disable-model-invocation` hides the skill from model selection while retaining explicit commands. Unknown frontmatter fields are ignored; do not infer permission enforcement from a field being accepted.

## Other agents and custom runtimes

1. Find current official documentation or inspect the installed loader for that agent and surface.
2. Verify `SKILL.md` support, required metadata, discovery roots, activation, tool access, trust, and refresh behavior.
3. If it supports Agent Skills, keep the core unchanged and add only necessary documented adapters.
4. If it does not, offer a prompt/rule adaptation as a separate artifact. Do not label it a natively supported skill or claim automatic discovery.
5. Test the adapted workflow in the target host before calling it behavior-compatible. If unavailable, report documentation-based support only.
