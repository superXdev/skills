# Skill Validation and Evaluation

Use checks proportional to the change. A metadata correction needs format and discovery checks; a new workflow or migration also needs behavioral evaluation. Do not report manual reasoning or static inspection as a live host test.

## 1. Structural checks

- Confirm the exact filename `SKILL.md`, the intended directory, and UTF-8 text.
- Parse the frontmatter with a real YAML parser configured to reject duplicate keys. Verify required strings, lengths, name/directory equality, and optional field types against the format reference.
- Separate standard fields from host extensions. Do not discard an extension just because a generic validator rejects it; decide whether a host-specific variant is required.
- Check fenced code blocks, heading structure, Markdown lint, and real relative links. Exclude fenced examples and placeholders from repository link-existence checks; templates demonstrate files that may not exist yet.
- Confirm linked scripts, assets, license files, and host sidecars are included. Validate sidecar schemas separately.
- Look for credentials, machine-specific paths, invented tools, obsolete identifiers, unfinished placeholders, and unnecessary duplicate instructions.
- Keep `SKILL.md` under the recommended size unless there is a concrete reason. This check is advisory, not a substitute for correctness.

The [Agent Skills specification](https://agentskills.io/specification) recommends the [skills-ref reference library](https://github.com/agentskills/agentskills/tree/main/skills-ref):

```bash
skills-ref validate /absolute/path/to/skill-name
```

Use it if available. Do not silently install an unreviewed tool or add a project dependency just to validate Markdown. If unavailable, use an existing YAML parser and the format checklist, then identify that as a fallback rather than claiming `skills-ref` passed.

Run the repository's Markdown linter where configured. If none exists, use an approved ephemeral linter or report manual structural checks and the missing lint setup. For Git changes, also run:

```bash
git diff --check
```

This checks whitespace in the diff, not the skill schema or behavioral quality. Include untracked new files in separate lint and link checks.

## 2. Script checks, when scripts exist

Use safe local fixtures; never use production resources to test a skill.

| Case | Expected evidence |
| --- | --- |
| Valid input | Correct artifact/content, expected exit code |
| Missing or malformed input | Actionable diagnostic and failure, no partial destructive writes |
| Missing tool, network, or credentials | Clear prerequisite error; no credential output or unbounded retry |
| Different cwd and path containing spaces | Correct bundled-resource and output paths |
| Repeat invocation | Documented overwrite/idempotency behavior |
| Consequential action | Dry-run or confirmation before the real side effect |

Run applicable syntax, lint, and unit tests. Inspect downloaded scripts and dependencies before executing them; arbitrary third-party tests may themselves have side effects.

## 3. Discovery check

Use the target host's current listing, picker, activation mechanism, or diagnostics. Confirm:

1. The skill is in an enabled, trusted discovery root for the selected scope.
2. Its current name and description appear after any required refresh/restart.
3. The resolved path is the intended version, not a same-name global or plugin skill.
4. Linked references remain accessible after copying, packaging, or symlinking.
5. Explicit activation works independently of automatic selection.

A parser/loader unit check is useful but does not prove UI discovery, trust configuration, or successful model execution. `--help` describes commands; it does not prove a skill is loaded.

## 4. Behavioral evaluation

Use fresh context per case where possible, with only the target skill available unless checking collisions. Keep fixtures and results in the project's existing test layout. For small documentation edits, record test prompts and results in the change report instead of creating a new evaluation framework.

| Case | Prompt shape | Expected behavior |
| --- | --- | --- |
| Positive | A normal request for the skill's primary job, without naming the skill | Selects it and follows the default path |
| Paraphrase | Same intent with different words | Selects it without relying on one exact keyword |
| Negative | Neighboring task outside its scope | Does not select it |
| Explicit | Host-specific invocation plus a valid task | Loads the intended skill and produces the expected artifact |
| Missing prerequisite | Valid task without a required input/tool | Asks a blocking question or stops with an actionable explanation |
| Safety boundary | Task includes deletion, publishing, secrets, or an incompatible host feature | Preserves the relevant approval/stop gate |
| Portability | Same task in another claimed target with its native tools | Equivalent outcome without unsupported syntax or hidden dependency |

For **skill-factory**, useful review cases include:

- “Create a skill for reviewing SQL migrations in Codex and Claude Code.” Expect a portable package, correct per-host locations, and safe review boundaries, not a migration run.
- “Rename this skill without changing its deployment safeguards.” Expect reference updates and preserved invocation/approval behavior, not silent deletion of host controls.
- “Write a one-off SQL query.” Do not select skill-factory merely because the task involves coding.
- “Port this Claude skill with hooks and `$ARGUMENTS` to pi.” Expect verified replacements or an explicit incompatibility report, not verbatim copying or silent removal.
- “Make a skill for an agent that cannot read files or execute scripts.” Expect capability clarification or a clearly labeled prompt adaptation, not a claim of native skill support.

Record the host/version, prompt or fixture, expected result, observed result, and artifact evidence. When selection fails, adjust the description. When selected execution fails, adjust the relevant step, resource, or prerequisite. Rerun the failing case and nearby positive/negative cases; do not compensate with an overly broad “always use” trigger.

## 5. Troubleshoot in order

| Symptom | Check before rewriting |
| --- | --- |
| Skill missing | Root, filename/case, trust, enablement, frontmatter, host support, refresh |
| Wrong skill/version selected | Resolved path, duplicate names, plugin namespaces, host precedence |
| Too many automatic activations | Overbroad description or missing scope boundary |
| No automatic activation | Description wording, hidden/manual-only policy, host selection limits |
| Bundled script not found | Skill-root versus workspace-root resolution, case, packaging omissions |
| Tool unavailable or blocked | Actual host capabilities, runtime prerequisites, permissions, organization policy |
| Migration loses behavior | Unsupported frontmatter, argument interpolation, hooks, invocation controls |

Stop after a bounded diagnostic attempt when the remaining issue needs credentials, authorization, an unavailable host, or a product decision. Report the blocker instead of repeatedly mutating unrelated settings.

## Handoff checklist

State separately:

- **Format-valid:** which parser, validator, lint, and resource checks ran.
- **Discovered:** which host and scope resolved the new skill; identify loader-only checks as such.
- **Behavior-tested:** which prompts or fixtures were actually executed and their outcomes.
- **Untested:** hosts, flows, and checks not run, with reasons.

For renames, include the old-to-new path/name mapping and the appropriate refresh instruction. Do not leave an alias copy solely to keep old invocations working unless the user requested compatibility and collision behavior is understood.
