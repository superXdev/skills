---
name: bulletproof-react
description: Plan, build, refactor, or review a production React or Next.js application using scalable feature boundaries, explicit state ownership, typed API layers, testing, security, error handling, and performance practices. Use when users ask for React architecture, feature-based organization, API and state boundaries, production-readiness, or a Bulletproof React-style review. Do not use for a small isolated component change unless architecture, data flow, or quality practices are in scope.
---

# Bulletproof React

**Source:** [alan2207/bulletproof-react](https://github.com/alan2207/bulletproof-react) (MIT licensed).

Apply durable React engineering principles from Bulletproof React without treating its sample stack or directory tree as mandatory. Adapt to the project's existing router, styling, data-fetching, and test tooling unless the user asks for a migration.

## Read first

- Read [architecture](references/ARCHITECTURE.md) for project boundaries, component design, state, and API-layer decisions.
- Read [quality](references/QUALITY.md) when implementing or reviewing tests, security, error handling, performance, standards, or delivery.
- Treat repository instructions and existing conventions as higher priority than this skill.

## Core rules

1. Keep dependencies directional: shared code can serve features, and features can serve the application layer. Do not import implementation details directly from one feature into another.
2. Organize by feature when code represents a product capability. Keep code close to the feature or component that owns it.
3. Define types and validation at boundaries before relying on external data. Keep API client configuration, endpoint fetchers, and cache hooks separate.
4. Classify state before choosing a solution: component state, cross-component application state, server cache state, form state, or URL state.
5. Keep state local by default. Lift it only for shared ownership; use a global store only when state is genuinely cross-cutting. Do not use global client state as a server-data cache.
6. Build small components with one clear responsibility. Prefer composition over prop drilling and oversized prop APIs. Abstract shared components only after repeated, stable use cases appear.
7. Test user-visible behavior and feature workflows. Mock at the network boundary with the project's supported mock server rather than mocking internal implementation details or hardcoding API data.
8. Treat client-side authorization as UX only. Authorization decisions and sensitive validation must be enforced by the server.
9. Add error handling at the API boundary and at meaningful UI boundaries. Provide useful fallback and recovery behavior, including the product-approved response to expired authentication.
10. Measure before optimizing. Prefer route-level splitting, state colocation, appropriate styling choices, and targeted prefetching over speculative memoization or excessive code splitting.
11. Enforce conventions with tooling: formatter, linter, TypeScript, dependency-boundary rules, and focused Git hooks where the project supports them.

## Workflow

### 1. Establish the scope

Identify the request as one or more of: a new feature, architecture/refactor, API work, state work, UI component work, quality review, or production readiness.

Inspect the project's current structure, TypeScript settings, aliases, linting, data layer, tests, and local instructions. Reuse established conventions when they are sound. Ask before a broad migration or a new dependency that changes the project architecture.

### 2. Choose boundaries

For a feature, create only the folders it needs under the project's feature location. A typical shape is:

```text
features/<feature>/
  api/          # endpoint declarations, fetchers, query or mutation hooks
  components/   # feature-private UI
  hooks/        # feature-private behavior
  types/        # feature-specific domain types and schemas
  utils/        # feature-specific pure helpers
```

Do not create empty folders merely to copy a template. Promote code to a shared layer only after it is genuinely reusable across features.

### 3. Implement from boundaries inward

1. Define request and response types, plus runtime validation where external input is untrusted.
2. Add an endpoint fetcher that uses the configured API client. Do not duplicate client setup across endpoints.
3. Add the project's server-state hook around that fetcher, with a stable cache key and clear invalidation behavior for mutations.
4. Build feature UI around the hook's loading, success, empty, error, and mutation-feedback states.
5. Keep simple interaction state local. Use reducers for related transitions and a global store only for cross-cutting client state. Put shareable navigation state in the URL.
6. Wire a real route, parent, or public feature API. Do not expose another feature's private files as a shortcut.
7. Add behavior-focused integration coverage. Use realistic network handlers for success and failure; add E2E coverage if this changes a critical user journey.

### 4. Verify the change

Run the project's relevant type check, lint, unit or integration tests, build, and E2E tests when the change affects a critical journey and the suite is available. Exercise error and empty paths, not just the happy path.

If a check cannot run, report it accurately with the reason and the remaining risk. Never claim a quality, security, or performance result that was not verified.

## Review checklist

Use this checklist for a focused audit. Report findings with file paths, impact, and the smallest credible fix.

- Does code flow from shared layers through features into the application layer, without cross-feature implementation imports?
- Is the feature self-contained and are files colocated with their consumers?
- Are API request and response boundaries typed, validated where needed, and separated into client, fetcher, and server-state hook?
- Does each state live at the smallest appropriate scope, with server data kept out of ad hoc global state?
- Are components cohesive, composed rather than over-configured, and accessible within the project's UI system? Were shared abstractions created only for repeated, stable use cases?
- Are loading, empty, error, and mutation-feedback states represented where data is displayed or changed?
- Are tests behavior-oriented, with critical user journeys covered at an appropriate integration or E2E level?
- Are authentication storage, output rendering, and authorization decisions handled safely?
- Are expensive changes supported by evidence, and are obvious bundle, render, image, styling-runtime, and data-fetching regressions avoided?
- Are direct imports used where the build tool can be harmed by broad barrel exports, and can linting enforce the intended feature and dependency boundaries?

## Boundaries

- Do not replace the project's router, package manager, styling system, state library, API client, or test stack merely to match the example repository.
- Do not add libraries, change authentication storage, alter authorization, delete files, or run deployment commands without explicit approval.
- Do not expose secrets, trust client-side role checks for security, or render untrusted HTML without appropriate sanitization.
- Do not copy source code from Bulletproof React. This skill captures high-level engineering guidance derived from its documentation. See [ATTRIBUTION.md](references/ATTRIBUTION.md).
