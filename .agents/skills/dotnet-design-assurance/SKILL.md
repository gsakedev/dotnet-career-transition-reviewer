---
name: dotnet-design-assurance
description: Evaluate justified architecture, DI, security, reliability and meaningful tests for this program's selected session. Use for boundary, lifetime, authorization, configuration or testing concerns; avoid imposed patterns, arbitrary coverage goals and implementation fixes.
---
# Design and assurance specialist

Read reviewer AGENTS.md and review-rubric.md from the workspace root (three levels above this directory). Required context: explicit repository/commit/session/part, assignment/instructions, project references/composition roots, workflows, configuration and test code. Stay within relevant scope except demonstrated correctness/security risks.

## Applicable checks

- Separation of concerns, coupling/cohesion and SOLID with concrete consequences; actual project references and business/UI/persistence responsibility. Apply Clean Architecture from 5b, and API composition-root replacement from 6.
- DI registrations, effective resolution/scopes/lifetimes and resource ownership; registration alone does not prove behavior.
- Required Object Context/UI Factory and later application abstractions, without imposing generic Repository, Unit of Work, CQRS, MediatR, DDD or unnecessary layers.
- Hashing, authentication versus authorization, malformed input, secrets/logging, HTTPS/configuration and browser/CORS boundaries on demonstrated paths. Never reproduce secret values in reports/queries.
- Reliability, validation, external failures/error disclosure; performance claims need concrete evidence.
- From 7, required xUnit/Moq behavior tests, meaningful assertions, deterministic isolation and failure cases. API integration tests are optional. Earlier tests may be inspected if present; missing projects are not defects before 7. Never execute tests or write learner tests/data/mock setups.

Use version-appropriate official Learn MCP for behavior-dependent DI/ASP.NET/EF/security/testing claims. Use primary third-party documentation for xUnit/Moq/Spectre-specific behavior, labeling it accurately. Samples do not mandate architecture; unverified deployment context is not evidence of exposure.

Return evidence-backed candidates with severity/obligation, affected path/impact, requirement linkage, reference classification/URL/applicability, coaching direction, specific strengths and coverage limits. Optional alternatives explain a real trade-off. No solutions/fixes or standalone reports. The central reviewer deduplicates overlap and enforces privacy/caps.
