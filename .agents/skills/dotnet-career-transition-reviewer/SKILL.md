---
name: dotnet-career-transition-reviewer
description: Orchestrate session-specific reviews for this .NET career-transition program across explicitly selected independent mentee repositories, with an automatic learning-focus report. Use for full, focused or historical progress reviews with mentor or mentee output; infrastructure checks never authorize an implementation review.
---
# Central reviewer

Read reviewer AGENTS.md, targets.json and review-rubric.md from the workspace root (three levels above this skill directory). Prefer launching from that root: sibling instruction/skill discovery is not automatic.

## Establish context

Require exactly one explicit mentee repository and session/assignment part. Verify canonical Git root and configured ID/path agreement. No mentees are configured initially. An explicit path can select a target without editing targets.json; persistent registration requires mentor authorization. Clarify collisions/ambiguity before analysis. Never infer a target from recent work or use the template as a mentee.

Record the manifest required by AGENTS.md. Read authoritative template root/session instructions, assignment, learning objectives and relevant inherited requirements, plus applicable mentee root/nested instructions. Record actual target framework/package/language settings; sample versions do not prove mentee versions. Disclose copied-assignment divergence and interpretation conflicts. Never execute repository code/builds/tests.

For snapshots, inspect relevant complete workflows. For changes, resolve explicit base/target SHAs, read diff and surrounding paths, distinguish introduced/pre-existing issues. Never switch branches. For progress, use historical content/reports from the same mentee and separate changed expectations, demonstrated progress, regressions and missing evidence. A branch suggests a session but does not override explicit selection or settle a missing part.

## Inspect and validate

1. Extract uniquely identified active criteria with source, obligation and lineage. Read [coverage-template.md](references/coverage-template.md); track all criteria and execution paths, including uninspected areas.
2. Read the selected sibling specialist SKILL.md files: dotnet-assignment-compliance for requirements; dotnet-correctness for relevant C#/runtime/async/data behavior; dotnet-design-assurance for relevant structure/security/testing. Skills are not spawned agents.
3. Validate candidates against implementation, callers and tests. Use official Learn MCP search/retrieval for framework-dependent claims with actual versions. Query generic APIs/behavior only, never private source. Cite supporting URL and applicability. Disclose unavailable tools or labeled official-web fallback.
4. Consolidate by root cause; discard speculative, low-value and unrelated claims. Record strengths and separate general engineering, language/runtime and ecosystem evidence. Previous-stack comparisons need supplied background; causes inferred from code remain hypotheses.
5. Reconcile criteria totals/statuses, inspect coverage limits and check target drift. A limited inspection does not establish application quality/readiness.

## Automatic learning report

After preparing the selected main review assessment, automatically read and apply [dotnet-learning-coach](../dotnet-learning-coach/SKILL.md) to produce a separate Learning Progress and Next Focus report. This is part of every normal snapshot, change, focused or historical progress review; no separate user command is required. Respect an explicit request to exclude coaching. Infrastructure/setup checks alone do not trigger it.

Supply the same explicit manifest, relevant code snapshot/callers, assignment and learning objectives, consolidated review evidence, coverage/limitations, versions/documentation ledger and any explicit comparison baseline. The existing combined review is sufficient input; separate code/.NET reports are optional. Keep focused coaching within the inspected scope and respect Session 1a's lack of application evidence.

Use the selected audience for both outputs: mentor by default, mentee only when requested. When both audiences are requested, construct learner-safe coaching independently. A mentee-only review must not automatically read a private mentor assessment. Treat new suspected defects as candidates for central validation and consolidation before finalizing either report.

Coordinate finding IDs, cap/overflow disclosures, at most two discussion questions and the same immediate next action across the complete feedback per audience. Recheck privacy and target drift after coaching. Return the learning report alongside the main review in chat; if saving was explicitly requested, include next-learning-focus.md in the same validated per-audience review directory without overwriting reports. Do not silently omit coaching because the main report already has a learning-focus section; disclose missing evidence or unavailable skill files as limitations.

## Audience and output

Default mentor mode: use [mentor-report-template.md](references/mentor-report-template.md). Include detailed reasoning/trade-offs and all significant validated findings, without implementation-ready solutions. Private artifacts stay under reviews/mentor-only/.

Only when requested, use [mentee-report-template.md](references/mentee-report-template.md). Construct feedback independently from selected learner-safe evidence, not a wholesale private-assessment summary. Never expose private paths/links or personnel hypotheses. Mentee-only mode does not automatically read/create a mentor assessment except to retain significant overflow privately. Sanitize learner coverage while preserving all status/gap transparency.

Apply AGENTS.md's normal five-finding cap and private overflow exception. Disclose total, omitted and critical-omitted counts; critical overflow means incomplete feedback requiring mentor triage. Do not hide additional technical findings in coverage comments. End learner guidance with one concrete next step and at most two meaningful questions.

Return in chat unless saving is requested and permitted. Validate output containment/path segments; use a unique date/short-SHA/suffix review ID; never overwrite, publish or copy reports to a mentee repository. The central reviewer owns consolidation and verification limitations.
