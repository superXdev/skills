# Architecture reference

Read this reference when planning a feature, refactoring project structure, designing a component boundary, or choosing a state or API-layer approach.

## Directional dependencies

Use a one-way dependency flow:

```text
shared code → features → application layer
```

- **Shared code** includes generic UI primitives, libraries, configuration, common hooks, shared types, and utilities.
- **Features** own a product capability and its private API code, UI, hooks, types, and helpers.
- **Application layer** owns bootstrapping, providers, routing, page composition, and application-wide configuration.

A feature must not import private implementation code from another feature. If two features need the same behavior, either extract a deliberately generic shared module or compose the features at the application layer.

## Feature boundaries

Start with the smallest directory structure that makes ownership clear. Use a feature directory for code that changes together because it delivers one product capability. Keep small, one-off code with its component rather than creating a global folder prematurely.

A feature can expose a narrow public surface, such as a route component or a high-level component. Treat its internal directories as private. Feature-local assets and stores belong with that feature; shared assets and application-wide stores belong in their respective shared layers.

Avoid broad barrel files that re-export an entire feature by default. Depending on the bundler, they can obscure dependencies or weaken tree shaking. Prefer direct imports, unless the project has measured and documented a safe public-module pattern.

Where the project uses ESLint or equivalent architecture tooling, encode the dependency rules so cross-feature imports and reverse dependencies fail in CI rather than relying on review alone.

## Components

- Give each component one clear responsibility.
- Colocate components, styles, tests, stories, and supporting hooks when they serve the same UI.
- Split a component when rendering branches or data transformations obscure its purpose, not because of an arbitrary line limit.
- Prefer composition, children, and focused subcomponents to a broad component with many mutually dependent props.
- Move a component into a shared UI layer only when it has multiple real consumers and an interface that is independent of one feature's business rules.
- Wrap a third-party primitive behind a project-owned component when doing so gives the product a stable accessible interface or protects the application from vendor-specific APIs.
- Use a fully styled component library when its design system is an intentional fit. Prefer accessible headless primitives when implementing a distinct product design system. Do not introduce either casually.
- Preserve the project's accessibility conventions and semantic HTML expectations.
- Use isolated component stories or a component catalog for complex, reusable UI when the project already has that workflow or its maintenance value is clear.

## API layer

Keep three responsibilities separate:

1. **Configured API client:** base URL, credentials policy, headers, interceptors, common error normalization, and transport behavior.
2. **Endpoint fetcher:** a typed function for one endpoint or operation. It should be testable without rendering React.
3. **Server-state hook:** the query or mutation wrapper that owns caching, request lifecycle, invalidation, and UI-facing state.

At boundaries, define request and response types. Add runtime schemas when data comes from an untrusted or weakly typed source, such as a third-party API, form submission, URL parameters, or persisted data.

For mutations, state which cached data becomes stale and invalidate or update it deliberately. Avoid network calls directly inside presentation components when the project has an API layer.

## State selection

Choose the smallest state scope that fits the owner and update frequency.

| Kind | Use for | Prefer |
| --- | --- | --- |
| Component state | A component's independent interaction | Local state |
| Related component transitions | Multiple values changed by named actions | Reducer |
| Shared UI/application state | Theme, global dialog, notification, or similarly cross-cutting client state | Existing context or store |
| Server state | Remote data, cache, retries, synchronization | Project query/cache library |
| Form state | Inputs, validation, submission feedback | Project form solution |
| URL state | Shareable navigation, filters, pagination | Router/search params |

Before moving state globally, try component composition, lifting state to the nearest common parent, or a feature-local context. Do not duplicate server responses into global client state unless a clear synchronization strategy requires it.

For forms with repeated validation, error-display, and submission patterns, use the project's form abstraction and schema validation tools rather than implementing divergent behavior in every form. Keep the server as the final validator.

Use URL state for values users should be able to share, bookmark, restore, or navigate through, such as filters, pagination, selected resources, and search terms.

## Naming and standards

Follow repository conventions first. If the project has no convention, apply one consistently:

- Use clear, predictable names for files and folders.
- Use component names that describe the UI responsibility.
- Use hook names beginning with `use`.
- Prefer TypeScript strictness and explicit public types at module boundaries. Remember that static types do not validate runtime data.
- Configure absolute import aliases only when the project's toolchain resolves them consistently in the compiler, test runner, editor, and production build.
- Configure and run formatting, linting, type checking, and dependency-boundary checks rather than relying on manual consistency.
- Use pre-commit checks for fast local feedback and CI for authoritative verification. Keep hooks focused so they do not make normal development impractically slow.
