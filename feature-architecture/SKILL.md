---
name: feature-architecture
description: Guide TypeScript/JavaScript implementation or refactoring toward feature-based architecture across Node.js, Express, Next.js, React, and similar apps. Use when the user explicitly asks for feature-based architecture, feature ownership, or feature modules, and also when implementing or refactoring features where domain logic, public APIs, UI when present, data access, and Vitest tests should be organized around feature ownership.
---

# Feature Architecture

Use feature ownership as the default organizing rule while preserving the repository's existing conventions. Inspect the codebase first, then place new code where the current structure, runtime, framework, and import boundaries make sense.

## Ownership Rules

- Keep feature-specific behavior inside `src/features/<feature>/` or the repo's established feature/module root.
- Move reusable cross-feature primitives outside the feature. Reusable means already shared by multiple features or clearly infrastructure-level, such as database clients, HTTP clients, auth/session helpers, UI primitives, logging, config, and generic validators.
- Do not extract code just because it might become reusable later. Keep it inside the feature until reuse is real or imminent.
- Let a feature own its domain logic, public API interface, data access, tests, and UI components/hooks when the app has a UI and those pieces are specific to that feature.
- Avoid horizontal layer folders for feature-only code, such as global `components/`, `services/`, or `data-access/` buckets, unless the repo already uses them and changing course would create churn.

## Public Boundary

Require each feature to expose a public boundary through `src/features/<feature>/index.ts`.

- Other features and app-level entrypoints should import from the feature's `index.ts`, not from deep internal paths.
- Export only stable, intentional surface area: public functions, types, components, hooks, actions, or adapters.
- Keep internal domain helpers, query builders, component parts, fixtures, and implementation details unexported unless another feature legitimately needs them.
- If a deep import is necessary, treat it as a design smell and either promote the item to the public API or move shared code outside the feature.

## Default Shape

Use this shape when the repo does not already define a stronger convention. Create subfolders only when needed.

```text
src/features/<feature>/
  index.ts
  domain/
    model.ts
    model.test.ts
  data-access/
    queries.ts
    queries.test.ts
  api/
    handlers-or-routes.ts
    handlers-or-routes.test.ts
  components/
    FeatureWidget.tsx
    FeatureWidget.test.tsx
  hooks/
    useFeature.ts
    useFeature.test.ts
```

Suggested responsibilities:

- `domain/`: business rules, state transitions, validation, feature-specific models, pure functions.
- `data-access/`: feature-specific queries, repository functions, persistence adapters, DTO mapping.
- `api/`: the feature's interface to the outside world, such as Express routers/controllers, HTTP handlers, server actions, route-handler logic, RPC procedures, CLI handlers, job handlers, or request/response adapters.
- `components/`: React/Next.js UI that belongs only to this feature. Omit in API-only or worker apps.
- `hooks/`: feature-specific React hooks. Omit when the app has no React UI.
- `index.ts`: the public export surface for the feature.

## Framework Adapters

- For Express apps, keep app-level server setup and middleware wiring outside features when it is shared. Mount feature routers from the feature's public API or feature-owned `api/` code.
- For Next.js apps, keep framework-required route files under `app/` when Next.js requires it, but make them thin adapters that call the feature's public API or feature-owned `api/` code.
- For worker, CLI, queue, or cron entrypoints, keep the runtime entrypoint thin and delegate feature behavior to the feature's public API or feature-owned `api/` code.
- Keep feature-specific client/server components inside the feature. Promote only generic UI primitives to shared UI folders.
- Respect runtime boundaries. Do not export server-only data access into browser/client code; expose safe client-facing functions, types, or components through the feature boundary.

## Vitest Tests

- Collocate Vitest tests next to the files they verify using `*.test.ts` or `*.test.tsx`.
- Prefer fast domain tests for business rules and focused component/hook tests for UI behavior.
- Add or update tests in the same feature folder when changing feature behavior.
- Keep shared test utilities outside features only when they are reused across features.

## Implementation Workflow

1. Inspect existing feature/module patterns, test naming, aliases, and framework constraints.
2. Decide whether each new item is feature-specific or genuinely shared.
3. Put feature-specific code inside the feature folder and shared infrastructure outside it.
4. Add or update the feature's `index.ts` public boundary.
5. Replace deep imports with public-boundary imports when touching related code.
6. Collocate Vitest tests with the files under test.
7. Run the narrowest relevant tests first, then broader checks when the boundary or shared code changed.
