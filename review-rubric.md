# Review rubric

Reviewer/rubric version: 1.0.0. Record this version and the inspected instruction paths in reports.

## Evidence and acceptance criteria

A finding needs an observed implementation, reachable scenario, concrete impact, supporting code/call sites and an applicable obligation or optional rationale. Missing context is a verification question. Use known paths/lines from the exact target. Distinguish static inspection, learner-supplied output and actual execution; tests inspected are not tests passed.

Extract uniquely identified criteria from authoritative assignments/notes with source references and template SHA. Record required/optional/ambiguous obligation and new/inherited/superseded lineage. Count active criteria once. Optional features not elected are excluded with reasons, not marked Not Met. List superseded/excluded items separately. Requirement conflicts remain visible and Not Verified until clarified.

- Met: sufficient evidence for the criterion, with evidence method stated; static Met does not claim runtime success.
- Partially Met: some required parts demonstrated, with specific gaps.
- Not Met: a demonstrated violation/absence after relevant inspection.
- Not Verified: missing evidence, ambiguity or uninspected behavior.

Total active criteria = Met + Partially Met + Not Met + Not Verified. Track execution paths separately: entry, validation, business rule, data/external calls, authorization, errors and output where relevant. Preserve coverage transparency without using matrix comments to bypass finding limits.

## Session gates

Verify these navigation aids against the actual source per review; they do not replace assignments.

| Stage | Assessment focus | Do not impose |
| --- | --- | --- |
| 1a | README/LICENSE and Git workflow evidence | Application implementation |
| 1b | Customer/appointment CRUD, validation, in-memory lists, Spectre UI | DI, database, architecture split, async, tests |
| 2a | Appointment variants, justified OOP choices, error handling | A prescribed hierarchy, database, DI, tests |
| 2b | Eight LINQ reports, empty/tie/zero-day behavior | Mandatory query syntax or invented tie-breaking |
| 3 | Users/roles/login/auditing, hashing, Microsoft DI/logging, Object Context/UI Factory | EF, async, Clean Architecture; Seq is optional |
| 4 | EF CRUD/relationships/configuration/secrets | Async, Clean Architecture, generic Repository/Unit of Work |
| 5a | Async DB/HTTP, cancellation, independent parallel retrieval, name-day discount | 5b split, CQRS/MediatR, test projects |
| 5b | Domain/Application/Infrastructure/Console boundaries, DTOs, composition root | Generic repositories, CQRS/MediatR or DDD |
| 6 | Eleven REST routes, DTOs/errors/statuses, Basic auth/HTTPS, async, API composition root, console HTTP client | JWT/OAuth, caching/versioning, required integration tests |
| 7 | xUnit/Moq Domain/Application behavior tests and determinism | Coverage percentages; API integration tests are optional |
| 8 | WASM API-only presentation, functionality, CORS, credential handling | Blazor Server, replacing Basic auth, state libraries |

Inspect relevant inherited functionality, not unrelated later topics. Session 6 replaces 5b's console composition root with API hosting. Repositories/application abstractions appear in 5b; generic repositories and separate Unit of Work are not mandated.

## Known interpretation issues

- Assignment 1b names Console in a path but ConsoleApp in its tree/notes: clarify before penalizing.
- Assignment 3 requires hashing, with PasswordHasher<T> as an example; notes name that type. Do not reject an alternative solely by name.
- Assignment 4 permits database-first; committed migrations are conditional on code-first.
- Session 5 notes combine a/b: do not apply 5b structure to a 5a-only review.
- Session 6 optional administrator restrictions and unspecified reporting/user-management routes leave inherited details ambiguous. Identify authorization risk independently; do not invent routes.
- Earlier resources expose testing/patterns before required assignments. Exposure is not obligation. The specified admin/admin training default is not itself a learner deviation; do not invent deployment exposure.

## Severity versus obligation

| Severity | Demonstrated impact |
| --- | --- |
| Critical | Reachable severe compromise, major corruption/data loss or fundamental correctness failure needing immediate mentor attention |
| High | Substantial security/correctness/reliability failure in a demonstrated workflow |
| Medium | Localized defect or important requirement gap with bounded impact |
| Low | Minor impact or justified improvement |

Must-fix: applicable requirement or demonstrated defect needs correction. Optional: justified improvement beyond minimum compliance. Identify subjective preferences separately and usually omit them. Official samples do not convert preferences into requirements. No arbitrary scores/weights.

## Mentoring and reporting

Evaluate general engineering, C# language/runtime and .NET ecosystem separately. Streams/LINQ, Task/Promise/CompletableFuture, properties, generics, disposal and lifetime comparisons require supplied background and relevant evidence. Do not infer causes from style. Progress requires historical code or reports from the same mentee; account for changed expectations and baseline limitations.

Normal feedback: maximum five consolidated findings. Private mentor assessment: every significant validated finding, including overflow. Disclose total/omitted/critical-omitted counts; never silently suppress critical issues. Use the central AGENTS.md audience policy and the selected report template. Do not automatically export mentor-only reasoning. End learner guidance with one concrete action.
