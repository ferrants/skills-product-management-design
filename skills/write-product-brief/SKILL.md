---
name: brief-to-tickets
description: Generate implementation plans as Linear issues/tickets from a PRD stored in Linear. Use this skill when the user wants to break a PRD into tasks, create implementation tickets, generate a plan from a product brief, or turn a Linear document into actionable Linear issues. Also trigger when the user mentions "tickets from PRD", "break down the PRD", "create tasks from the spec", or wants to go from planning to execution in Linear.
---

# Brief to Tickets

Break a PRD (stored as a Linear document) into independently-grabbable Linear issues using vertical slices (tracer bullets).

This skill is the natural follow-up to the **write-product-brief** skill, which creates PRDs as Linear documents. Once a PRD exists in Linear, this skill turns it into actionable tickets.

## Process

### 1. Locate the PRD in Linear

Ask the user which PRD to break down. Use Linear MCP tools to find it:

- Use `Linear:list_documents` to search for the PRD by name or browse recent documents. You can filter by project if the user specifies one.
- Use `Linear:get_document` to retrieve the full PRD content once identified.

If the user provides a document ID or slug directly, fetch it with `Linear:get_document`. If they give a name, search with `Linear:list_documents` using the `query` parameter.

Read and internalize the full PRD content.

### 2. Identify the target team and project

Ask the user which Linear team the issues should be created under (required for creating issues). Also ask which project to associate the issues with, if applicable.

Use `Linear:list_projects` or `Linear:get_project` to confirm the project exists. Use `Linear:list_issue_labels` if you need to find relevant labels for the tickets.

### 3. Explore the codebase (if applicable)

If the PRD references a codebase, read the key modules and integration layers referenced. Identify:

- The distinct integration layers the feature touches (e.g., DB/schema, API/backend, UI, tests, config)
- Existing patterns for similar features
- Natural seams where work can be parallelized

If no codebase is available or relevant, skip this step.

### 4. Draft vertical slices

Break the PRD into **tracer bullet** plans. Each plan is a thin vertical slice that cuts through ALL integration layers end-to-end, NOT a horizontal slice of one layer.

<vertical-slice-rules>
- Each slice delivers a narrow but COMPLETE path through every layer (schema, API, UI, tests)
- A completed slice is demoable or verifiable on its own
- Prefer many thin slices over few thick ones
- The first slice should be the simplest possible end-to-end path (the "hello world" tracer bullet)
- Later slices add breadth: edge cases, additional user stories, polish
</vertical-slice-rules>

### 5. Quiz the user

Present the proposed breakdown as a numbered list. For each slice, show:

- **Title**: short descriptive name
- **Layers touched**: which integration layers this slice cuts through
- **Blocked by**: which other slices (if any) must complete first
- **User stories covered**: which user stories from the PRD this addresses

Ask the user:

- Does the granularity feel right? (too coarse / too fine)
- Are the dependency relationships correct?
- Should any slices be merged or split further?
- Is the ordering right for the first tracer bullet?
- Are there any slices missing?

Iterate until the user approves the breakdown.

### 6. Create Linear Issues

For each approved slice, create a Linear issue using `Linear:save_issue`. Create issues in dependency order (blockers first) so you can set up blocking relationships using real issue IDs.

For each issue, set:

- **title**: A clear, descriptive title for the slice (required)
- **team**: The team the user specified (required)
- **description**: Use the issue body template below (as Markdown)
- **project**: The project the user specified (if any)
- **labels**: Any relevant labels
- **priority**: Based on the slice's position in the dependency chain (earlier = higher priority)
- **blockedBy**: Array of issue IDs for any slices that must complete first

After creating each issue, note the returned issue ID so you can reference it in the `blockedBy` field of subsequent issues.

<issue-body-template>
## Parent PRD

This issue is part of the PRD: **[PRD Title]** (link to the Linear document if available)

## What to build

A concise description of this vertical slice. Describe the end-to-end behavior, not layer-by-layer implementation. Reference specific sections of the parent PRD rather than duplicating content.

## Acceptance criteria

- [ ] Criterion 1
- [ ] Criterion 2
- [ ] Criterion 3

## User stories addressed

Reference by number from the parent PRD:

- User story 3
- User story 7
</issue-body-template>

### 7. Print a summary

After creating all issues, print a summary table with the Linear issue identifiers:

```
| Identifier | Title                  | Blocked by | Priority |
|------------|------------------------|------------|----------|
| TEAM-42    | Basic widget creation  | None       | High     |
| TEAM-43    | Widget listing         | TEAM-42    | Normal   |
```

Let the user know all issues have been created in Linear and they can view them in their workspace.

Do NOT modify the parent PRD document.
