---
name: angular-commit-messages
description: Write, validate, or review Git commit messages that follow the Angular conventional commit format, including allowed types, Angular package scopes, line-length limits, breaking changes, issue closing references, and reverts. Use when users request an Angular-style commit message, Conventional Commit validation for an Angular repository, changelog-compatible commits, or a correctly formatted revert. Do not use when a repository defines a different commit convention.
---

# Angular Commit Messages

Produce changelog-compatible commit messages using the Angular commit convention.

## Required format

```text
<type>(<scope>): <subject>

<body>

<footer>
```

- The header is required.
- The scope is optional: `<type>: <subject>` is valid.
- Keep every line at or below 100 characters.
- Omit the body and footer when they add no useful information.
- Use a blank line between header, body, and footer.

## Header rules

### Type

Use exactly one of:

- `build`: build system or external dependency changes
- `ci`: CI configuration or scripts
- `docs`: documentation-only changes
- `feat`: new feature
- `fix`: bug fix
- `perf`: performance improvement
- `refactor`: code change that is neither a fix nor feature
- `style`: formatting-only change with no meaning change
- `test`: add or correct tests

### Scope

The scope should be the affected npm package as understood by changelog readers. Supported package scopes:

```text
animations
common
compiler
compiler-cli
core
elements
forms
http
language-service
platform-browser
platform-browser-dynamic
platform-server
platform-webworker
platform-webworker-dynamic
router
service-worker
upgrade
zone.js
```

Additional accepted scopes:

- `packaging`: package layout, shared package metadata, declarations, or bundles
- `changelog`: release-note changes in `CHANGELOG.md`
- `docs-infra`: documentation application changes
- `ivy`: Ivy renderer changes
- `ngcc`: Angular Compatibility Compiler changes

Use no scope for broad `style`, `test`, or `refactor` work, or documentation not related to one package.

### Subject

- Write in imperative, present tense: `fix`, not `fixed` or `fixes`.
- Begin with lowercase.
- Do not end with a period.
- Describe the change succinctly.

## Body and footer

Use the body to explain motivation and contrast the behavior before and after the change. Use imperative, present tense and wrap lines at 100 characters or fewer.

Use the footer for issue-closing references and breaking changes:

```text
Closes #123
```

```text
BREAKING CHANGE: explain what changes for consumers and how they should migrate
```

Use the literal `BREAKING CHANGE:` marker followed by a space and the explanation, or place the explanation after two newlines.

## Reverts

For a revert, use this exact structure:

```text
revert: <header of reverted commit>

This reverts commit <full SHA>.
```

Do not use a normal type and scope before `revert:`.

## Workflow

1. Identify the actual change and its affected package. Do not infer a scope when the repository does not use these Angular package names.
2. Select the narrowest valid type.
3. Write a lowercase imperative subject without terminal punctuation.
4. Add a body only when it explains why the change is needed or how behavior differs.
5. Add closing issue references and a `BREAKING CHANGE:` footer when applicable.
6. Check every line is 100 characters or fewer, sections have blank-line separation, and no unsupported type or scope is used.

## Examples

```text
docs(changelog): update changelog to beta.5
```

```text
fix(release): depend on latest rxjs and zone.js

Copy the package version into the published artifact so consumers receive the required versions.
```

```text
feat(router): support redirect functions

Closes #12345
```

```text
revert: feat(router): support redirect functions

This reverts commit 0123456789abcdef0123456789abcdef01234567.
```

## Validation checklist

- [ ] Header matches `<type>(<scope>): <subject>` or `<type>: <subject>`.
- [ ] Type is in the allowed list.
- [ ] Scope is omitted or is an allowed package or exception scope.
- [ ] Subject is lowercase, imperative, concise, and has no final period.
- [ ] No line exceeds 100 characters.
- [ ] Body explains motivation and previous behavior when one is needed.
- [ ] Footer includes `Closes #<issue>` and `BREAKING CHANGE:` when applicable.
- [ ] Reverts use the required `revert:` format and full commit SHA.
