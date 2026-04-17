---
name: create-plan
description: Turn discussion, local notes, or codebase context into a high-level feature implementation plan and architectural blueprint saved under workspace/<feature-name>/plan/. Use when user wants to create a feature plan, implementation plan, phased plan, or blueprint for implementation tasks.
---

# Create a plan

Break a feature request into a high-level phased implementation plan using feature slices, plus a durable architectural blueprint. Output both files under `workspace/<feature-name>/plan/`.

## Process

### 1. Confirm the feature source

The feature source may be in the conversation, a local Markdown file, existing codebase context, or a combination of notes. If the source is unclear, ask the user to paste it, point you to the local file, or clarify the intended behavior.

### 2. Choose the feature workspace

Choose a short kebab-case feature slug from the feature name, such as `user-onboarding`. All planning artifacts for the feature go under:

```text
workspace/<feature-slug>/plan/
```

### 3. Explore the codebase

If you have not already explored the codebase, do so to understand the current architecture, existing patterns, and integration layers.

### 4. Identify durable architectural guidelines

Before slicing, identify high-level guidelines that are unlikely to change throughout implementation:

- Route structures / URL patterns
- Database schema shape
- Key data models
- Authentication / authorization approach
- Third-party service boundaries
- Feature organization: actions, domain functions, and UI touchpoints
- Public boundaries between features, shared infrastructure, and framework adapters
- Testing strategy and verification expectations

Write these in `workspace/<feature-slug>/plan/blueprint.md`, not in the main plan. When the repository is TypeScript/JavaScript or otherwise benefits from feature ownership, align the blueprint with the `feature-architecture` skill: keep feature-specific code in feature-owned modules, expose a clear public boundary, keep shared infrastructure outside the feature, and collocate relevant tests.

### 5. Draft feature slices

Break the feature into **feature slice** phases. Each phase is a thin vertical path through the relevant integration layers end-to-end, not a horizontal slice of one layer.

<feature-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Keep the plan high level: describe behavior, sequencing, scenarios, and acceptance criteria
- Do NOT include low-level implementation instructions, specific function names, or volatile file-by-file details in the plan
- DO include durable decisions in the blueprint: route paths, schema shapes, data model names, public boundaries, and architectural constraints
- Every slice must explicitly cover the feature's actions, domain functions, and UI. If one area is not touched, state `None` and explain why.
</feature-slice-rules>

### 6. Quiz the user

Present the proposed breakdown as a numbered list. For each phase show:

- **Title**: short descriptive name
- **Scenarios covered**: which behavior scenarios this addresses
- **Actions**: local actions or commands the feature needs
- **Domain functions**: domain behavior the feature needs
- **UI**: screens, components, or interactions the feature needs

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Should any phases be combined or split further?
- Are the actions, domain functions, and UI touchpoints correct?

Iterate until the user approves the breakdown.

### 7. Write the plan and blueprint files

Create `workspace/<feature-slug>/plan/` if it doesn't exist. Write:

- `workspace/<feature-slug>/plan/plan.md`: high-level implementation phases
- `workspace/<feature-slug>/plan/blueprint.md`: durable architectural guidelines and decisions

Use the templates below.

<plan-template>
# Plan: <Feature Name>

> Source: <conversation, local file path, or brief note identifier>
> Blueprint: `workspace/<feature-slug>/plan/blueprint.md`

## Goal

A short description of the outcome this feature should deliver.

## Phase 1: <Title>

**Scenarios**: <list of behavior scenarios>

### Feature touchpoints

- **Actions**: ...
- **Domain functions**: ...
- **UI**: ...

### What to build

A concise, high-level description of this feature slice. Describe the end-to-end behavior and user/system outcome, not file-by-file implementation.

### Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

---

## Phase 2: <Title>

**Scenarios**: <list of behavior scenarios>

### Feature touchpoints

- **Actions**: ...
- **Domain functions**: ...
- **UI**: ...

### What to build

...

### Acceptance criteria

- [ ] ...

<!-- Repeat for each phase -->
</plan-template>

<blueprint-template>
# Blueprint: <Feature Name>

> Source plan: `workspace/<feature-slug>/plan/plan.md`
> Architecture reference: `feature-architecture` skill, when applicable

## Architectural guidelines

- **Feature ownership**: where the feature-owned domain logic, public API, data access, UI, and tests should live
- **Public boundaries**: what this feature should expose to other features or app-level entrypoints
- **Shared infrastructure**: which concerns must stay outside the feature because they are cross-cutting or already shared
- **Framework adapters**: which routes, server actions, handlers, jobs, or UI entrypoints should remain thin adapters
- **Testing strategy**: expected unit, integration, component, or manual checks

## Durable decisions

- **Routes**: ...
- **Schema**: ...
- **Key models**: ...
- **Actions**: ...
- **Domain functions**: ...
- **UI**: ...
- **External services**: ...
- (add/remove sections as appropriate)

## Constraints

- Architectural constraints implementation tasks must preserve
- Edge cases or invariants that should not drift between tasks
- Compatibility, migration, or rollout constraints
</blueprint-template>
