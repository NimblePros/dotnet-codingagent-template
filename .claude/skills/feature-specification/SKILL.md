---
name: feature-specification
description: Generates a structured feature specification markdown file from a GitHub issue. Use when creating a spec for a new feature, epic, or backlog item that needs implementation planning.
---

# Feature Specification

Create a feature specification file at `.claude/specs/<issue-number>-<short-title>.md` using the structure below.

## Spec Format

### Header

```markdown
# <Feature Title>

**GitHub Issue**: #<number>
**Status**: Draft

## Overview

<What is being built and why. 2-4 sentences.>

## Acceptance Criteria

- [ ] <Testable condition 1>
- [ ] <Testable condition 2>

## Out of Scope

- <Explicitly list what is NOT included>

## Open Questions

- [ ] <Decision needed before implementation can begin>
```

### Implementation Phases

After the header sections, define sequential implementation phases. Each phase must be completable and testable before the next begins. Use this structure for each phase:

```markdown
## Phase 1: <Name> — <dotnet-agent | react-agent | test-agent>

**Goal**: <One sentence describing what this phase delivers>

### Tasks

- [ ] <Specific task 1 — e.g., "Create `Registration` entity with Id, EventId, AttendeeId, CheckedInAt">
- [ ] <Specific task 2 — e.g., "Add EF Core migration for Registrations table">
- [ ] <Specific task 3>

### Details

<Any data models, API endpoint signatures, component names, or other specifics needed to complete this phase without ambiguity. Include example request/response JSON for API endpoints.>

### Done When

- [ ] <Verifiable exit condition — e.g., "`dotnet build` passes with no warnings">
- [ ] <Verifiable exit condition — e.g., "GET /api/registrations returns 200 with correct shape">
```

## Phase Ordering Guidelines

Structure phases as vertical slices, each one adding a fully functional piece of the feature, where possible.

Within each phase, follow this order. For library or backend-only features, you may omit the UI Layer and End-to-End Tests phases.

1. **Domain Model**: Define the core entities and relationships needed for the feature.
2. **Unit Tests**: Note test cases that should be implemented alongside the domain model to validate business logic. Only conditional logic needs unit tests.
3. **Use Cases**: Define uses cases as message handlers that work with the domain model (and persistence and other abstractions) to implement the feature's behavior.
4. **Unit Tests**: Note test cases that should be implemented alongside the use cases to validate the feature's behavior. Only conditional logic needs unit tests.
5. **Infrastructure and Persistence**: Implement repositories, EF Core migrations, and any other data access needed to support the use cases.
6. **Integration Tests**: Consider whether integration tests are worthwhile for any use cases; prefer API-level ASP.NET Core tests that cover the full stack over isolated tests of repositories or other infrastructure.
7. **API Layer**: Implement API endpoints, validation, and any other necessary logic to expose the feature to the frontend.
8. **Functional Tests**: Implement ASP.NET Core integration tests (a.k.a. functional tests) that cover the API endpoints and use cases implemented in this phase. Ensure you cover happy path and all expected HTTP responses including 401, 404, etc.
9. **UI Layer**: Implement front-end React/Blazor/etc. components, forms, and API integration needed to provide a user interface for the feature.
10. **End-to-End Tests**: Implement .NET Playwright tests that cover the happy path and any critical edge cases for the feature.

Each phase must name which agent executes it.

## Guidelines

- Always begin by asking clarifying questions if any requirements or details are ambiguous. Do not make assumptions.
- Tasks must be specific enough to implement without clarifying questions
- Each task should be a single, independently completable unit of work
- "Done When" conditions must be objectively verifiable — no subjective criteria
- Flag security or performance considerations inline within the relevant phase
- Include example JSON for every API endpoint in the phase Details section
