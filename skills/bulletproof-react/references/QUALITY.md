# Quality reference

Read this reference when implementing or reviewing test coverage, failure behavior, security, performance, project standards, or release readiness.

## Testing strategy

Use several test levels, with emphasis based on risk:

- **Unit tests:** pure utilities, transformations, and complex isolated logic.
- **Integration tests:** feature workflows across components, data hooks, and realistic user interactions. This is usually the most valuable default for application behavior.
- **End-to-end tests:** a small set of critical user journeys through the running application.

Test outcomes a user can observe rather than component internals, implementation-specific hooks, or incidental markup. At the network boundary, use the project's API mocking approach to return realistic responses and failures. A request-intercepting mock server is particularly useful for frontend development before a backend endpoint exists. Avoid mocking `fetch` or internal hooks in a way that makes the test pass while the user flow is broken.

Cover success, loading, empty, authorization-denied, and error paths when they apply. Run browser E2E tests locally when practical and headless in CI when configured.

## Error handling

Handle failures at several boundaries:

- Normalize transport and API errors in the configured API client.
- Translate expected errors into actionable UI feedback near the affected workflow, using the project's notification pattern where appropriate.
- Handle unauthorized responses through the approved session-refresh or sign-out flow. Do not retry authentication failures indefinitely.
- Place error boundaries around meaningful sections or features so one failure does not unnecessarily remove the entire application.
- Preserve a recovery path: retry, return to a safe screen, or explain the next action.
- Send production error reports only through an approved monitoring service, without leaking sensitive user data.

Do not replace errors with silent failure, generic success messages, or fabricated fallback data.

## Security

Security controls belong primarily on the server. The client helps users understand what they can do but cannot enforce access control.

- Authenticate with the project-approved mechanism. Prefer secure, HTTP-only cookies when the server and product architecture support them; changing token storage needs explicit approval and server coordination.
- Enforce roles and permissions on every protected server operation. Client-side role checks are UX, not authorization.
- Validate at input boundaries, including forms, route parameters, and API payloads.
- Escape or sanitize untrusted content before rendering it as HTML. Avoid dangerous HTML rendering unless it is required and the sanitization policy is clear.
- Keep credentials and secrets out of source, logs, tests, browser bundles, and AI prompts.
- Do not log tokens, passwords, personally identifiable information, or full sensitive API responses.

## Performance

Start from user impact and evidence. Profile or measure before adding memoization, a new state library, or a broad code-splitting scheme.

Useful defaults:

- Keep state close to the components that consume it to limit avoidable renders.
- Split at route or substantial feature boundaries when it reduces initial work; avoid splitting every small component and adding network waterfalls.
- Use composition and stable component boundaries before reaching for memoization.
- Initialize expensive local state lazily when the computation is only needed once.
- Size and lazy-load images appropriately, using the project's image pipeline and responsive formats where available.
- Prefetch data only when a likely next navigation justifies the bandwidth and cache cost.
- Track web-vital regressions with the project-approved measurement tool.
- When a screen updates frequently, assess whether runtime CSS generation is contributing meaningful work; consider a build-time styling approach only with measurement and an approved migration plan.

## Delivery checks

Before completing relevant React work:

1. Run formatting, linting, the TypeScript check, and configured dependency-boundary checks.
2. Run focused unit or integration tests for changed behavior.
3. Run the production build when the toolchain permits.
4. Run or update E2E coverage for critical flows affected by the change.
5. Manually exercise user-visible loading, empty, error, keyboard, and responsive paths where automated checks do not cover them.
6. Review the diff for accidental dependency changes, cross-feature coupling, unsafe data handling, and unverified claims.

If a check is unavailable or fails for an unrelated existing reason, identify the exact command, result, and residual risk in the handoff.
