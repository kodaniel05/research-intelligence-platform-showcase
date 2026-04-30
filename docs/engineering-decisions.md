# Engineering Decisions

## Keep the Public Repo Non-Operational

This showcase repository is intentionally documentation-only. It does not include private application source code, provider adapters, database schema details, prompts, credentials, or reusable implementation logic.

Reasoning:

- Recruiters can evaluate product scope and engineering judgment.
- The private project remains protected.
- The public repo avoids becoming a cloneable competitor or leaking operational details.

## Server-First Dashboard Architecture

The private application uses server-side preparation for major dashboard views. Data fetching, normalization, ranking, and view-model assembly happen before the UI renders.

Why this matters:

- Less duplicated client-side data orchestration.
- More predictable page states.
- Cleaner separation between interactive UI and backend workflow logic.
- Easier testing of service-level behavior.

## Graceful External Source Failure

External APIs are unreliable by nature. The architecture treats each source category independently so failure in one provider does not collapse the whole dashboard.

Design implications:

- Source adapters return controlled fallback states.
- The feed can render partial results.
- Users see source mode indicators.
- Ranking continues with whatever valid data is available.

## Normalize Before Ranking

Different providers return different fields, quality levels, timestamps, and identifiers. The private app normalizes candidates before ranking or display.

Benefits:

- Ranking code works against consistent shapes.
- UI components do not need provider-specific branches.
- Dedupe and filtering are easier to reason about.
- New providers can be added behind an adapter boundary.

## AI as an Optional Workflow Layer

AI is useful for structured summarization and analysis, but it is not treated as a hard dependency for the whole product.

Safeguards:

- Explicit user-triggered AI workflows.
- Input caps and request budgets.
- Structured output expectations.
- Validation before persistence.
- Fallback behavior when model access is disabled or fails.

## Visible Source State

The UI communicates whether a section is using live, cached, fallback, or demo data.

Reasoning:

- Research tools need trust signals.
- Users should know when a feed is incomplete.
- Fallback data should not be mistaken for fresh live intelligence.

## Focused Feature Boundaries

Major workflows are separated into clear product areas:

- Signals.
- Papers.
- Concepts.
- Decisions.
- Email automation.

This reduces cognitive load and keeps each feature testable as an independent workflow.

## Test Strategy

The private project emphasizes focused tests around behavior with meaningful failure risk:

- Ranking and source handling.
- Concept linking and validation.
- Paper analysis mode behavior.
- Decision scoring.
- Email send safety.

The goal is to test the business behavior without requiring full end-to-end dependence on live external providers.

## Security and IP Boundaries

Public materials are written to communicate the product clearly without exposing:

- Exact ranking formulas.
- Private prompts.
- Source adapter implementation.
- Database table structure.
- Stored research data.
- Secrets or environment values.

