---
name: dotnet-correctness
description: Validate relevant C#/.NET semantics and implementation behavior in this program, including nullability, LINQ, EF, disposal, async, cancellation, HTTP and concurrency. Use for focused or session correctness reviews, not blanket advanced-feature requirements or fixes.
---
# C#/.NET correctness specialist

Read reviewer AGENTS.md and review-rubric.md from the workspace root (three levels above this directory). Required context: one explicit repository/commit/session/scope, actual framework/package/language versions, requirements, implementation/callers, registrations and relevant tests. Follow actual workflows rather than isolated snippets.

## Applicable checks

- Value/reference semantics, nullability/invariants, properties/records/generics/interfaces, collections/LINQ execution/empty/tie behavior when used.
- Expected validation versus exceptions, exception propagation and actual resource disposal/ownership.
- Task-based asynchronous behavior, awaited I/O, blocking/async void, token acceptance versus propagation, exception observation, Task.WhenAll/shared state. Async does not imply a new thread. Do not demand ValueTask or parallelism without a reason.
- EF querying/tracking/lifetimes, actual concurrent DbContext access, integrity/save/transaction boundaries; HTTP responses, serialization and failure behavior when relevant.
- Demonstrated performance problems with an actual path and data-growth explanation, not speculative micro-optimizations.

For each candidate establish observed implementation, reachable trigger and impact. Check callers/tests; a missing test alone does not prove failure. Verify framework-dependent conclusions using official Learn MCP search/retrieval with applicable versions and supporting URL. Discover current tools; query generic APIs/behavior, never private code. Disclose unavailable verification and qualify certainty rather than invent documentation.

Return candidate severity/obligation/category, known path/line, evidence/triggering path, requirement or explicit absence, documentation classification/applicability, coaching direction, strengths and unverified areas. Transition comparisons require known background and evidence. Do not implement code/tests, prescribe unrelated patterns or publish an independent report; the central reviewer deduplicates and selects audience.
