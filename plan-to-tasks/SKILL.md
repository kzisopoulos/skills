---
name: plan-to-tasks
description: Break a local feature plan into independently-grabbable local task files using feature slices. Use when user wants to convert a plan to implementation tasks, create local task files, or break down a feature plan into work items.
---

# Plan to tasks

Break a local feature plan into independently-grabbable task files using feature slices.

## Process

### 1. Locate the feature plan

Ask the user for the local plan path if it is not already clear from context.

If the plan is not already in your context window, read the local Markdown file before drafting tasks.

### 2. Explore the codebase (optional)

If you have not already explored the codebase, do so to understand the current state of the code.

### 3. Draft feature slices

Break the plan into **feature slice** tasks. Each task is a thin vertical path through the relevant integration layers end-to-end, not a horizontal slice of one layer.

Slices may be `HITL` or `AFK`. `HITL` slices require human interaction, such as an architectural decision or a design review. `AFK` slices can be implemented locally without human interaction. Prefer `AFK` over `HITL` where possible.

<feature-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- Each task must contain enough touchpoint detail to prevent implementation drift
- Every task must explicitly cover actions, domain functions, and UI. If one area is not touched, state `None` and explain why.
</feature-slice-rules>

### 4. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Type**: HITL / AFK
- **Blocked by**: which other slices (if any) must complete first
- **Scenarios covered**: which behavior scenarios this addresses
- **Actions**: local actions or commands the task needs
- **Domain functions**: domain behavior the task needs
- **UI**: screens, components, or interactions the task needs

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be combined or split further?
- Are the correct slices marked as HITL and AFK?
- Are the actions, domain functions, and UI touchpoints specific enough to prevent drift?

Iterate until the user approves the breakdown.

### 5. Create the local task files

Create a task folder for the feature under `./tasks/<feature-slug>/`.

For each approved slice, create a Markdown file named with a three-digit sequence and a short slug: `001-task-name.md`, `002-next-task.md`, etc. Create files in dependency order (blockers first) so the "Blocked by" field can reference earlier local task files.

<task-template>
# Task <number>: <Title>

> Source plan: <local plan path>

## Type

HITL or AFK

## Blocked by

- `<relative-task-path>` (if any)

Or "None - can start immediately" if no blockers.

## Scenarios covered

- Scenario 1
- Scenario 2

## Feature touchpoints

- **Actions**: specific actions, handlers, commands, or workflows this task must implement or update
- **Domain functions**: specific domain behavior, data rules, validations, and state transitions this task must implement or update
- **UI**: specific screens, components, states, interactions, and copy this task must implement or update

## What to build

A concise description of this feature slice. Describe the end-to-end behavior and reference specific sections of the source plan rather than duplicating large blocks.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## Implementation notes

- Touchpoint detail that must not drift from the plan
- Relevant constraints, edge cases, and verification steps
- Expected tests or manual checks

</task-template>
