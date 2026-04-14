---
name: create-plan
description: Turn discussion, local notes, or codebase context into a feature implementation plan saved as a local Markdown file in ./plans/. Use when user wants to create a feature plan, implementation plan, or phased plan with feature slices.
---

# Create a plan

Break a feature request into a phased implementation plan using feature slices. Output is a Markdown file in `./plans/`.

## Process

### 1. Confirm the feature source

The feature source may be in the conversation, a local Markdown file, existing codebase context, or a combination of notes. If the source is unclear, ask the user to paste it, point you to the local file, or clarify the intended behavior.

### 2. Explore the codebase

If you have not already explored the codebase, do so to understand the current architecture, existing patterns, and integration layers.

### 3. Identify durable architectural decisions

Before slicing, identify high-level decisions that are unlikely to change throughout implementation:

- Route structures / URL patterns
- Database schema shape
- Key data models
- Authentication / authorization approach
- Third-party service boundaries
- Feature organization: actions, domain functions, and UI touchpoints

These go in the plan header so every phase can reference them.

### 4. Draft feature slices

Break the feature into **feature slice** phases. Each phase is a thin vertical path through the relevant integration layers end-to-end, not a horizontal slice of one layer.

<feature-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Do NOT include specific file names, function names, or implementation details that are likely to change as later phases are built
- DO include durable decisions: route paths, schema shapes, data model names
- Every slice must explicitly cover the feature's actions, domain functions, and UI. If one area is not touched, state `None` and explain why.
</feature-slice-rules>

### 5. Quiz the user

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

### 6. Write the plan file

Create `./plans/` if it doesn't exist. Write the plan as a Markdown file named after the feature (e.g. `./plans/user-onboarding.md`). Use the template below.

<plan-template>
# Plan: <Feature Name>

> Source: <conversation, local file path, or brief note identifier>

## Architectural decisions

Durable decisions that apply across all phases:

- **Routes**: ...
- **Schema**: ...
- **Key models**: ...
- **Actions**: ...
- **Domain functions**: ...
- **UI**: ...
- (add/remove sections as appropriate)

---

## Phase 1: <Title>

**Scenarios**: <list of behavior scenarios>

### Feature touchpoints

- **Actions**: ...
- **Domain functions**: ...
- **UI**: ...

### What to build

A concise description of this feature slice. Describe the end-to-end behavior, not layer-by-layer implementation.

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
