# Assignment 2b — mentor-only technical assessment

Audience: Mentor only. Do not distribute this assessment as learner feedback.

## Review metadata

- Review ID: 2026-10-23-de0a3f1-7c4e92.
- Date: 23 October 2026, Europe/Athens.
- Reviewer/rubric version: 1.0.0.
- Mentee: Georgia; repository-derived ID: georgia. Registry validation unavailable because targets.json contains invalid JSON (`"mentees": [georgia]`). Explicit repository selection governs this review.
- Canonical repository: C:\Users\gsake\source\review-workspaces\mentee-georgia.
- Branch: feature/assignment2b.
- Target: de0a3f1ec1932ef269a83a465c52b4e6b17f14e0.
- Session/part: 2b. Full assignment snapshot, including relevant inherited functionality; uncommitted changes excluded. Working tree clean.
- Template: C:\Users\gsake\source\repos\dotnet-external-coaching-template at fbe08912a9502cb03088882cf553d93cc439e582. No drift from configured discovery commit.
- Assignment sources: docs/session2/assignments/assignment2b.md; inherited docs/session2/assignments/assignment2a.md and docs/session1/assignments/assignment1b.md; README/license obligations from docs/session1/assignments/assignment1a.md.
- Comparison baseline: Not applicable; no historical comparison requested.
- Previous-stack background: Unknown; no stack comparisons or progress claims.
- Project evidence: src/Appointments.Console/Appointments.Console.csproj:5–11 targets net10.0, enables nullable reference types and implicit usings, and references Spectre.Console 0.57.2. No explicit LangVersion found. Expected framework-default language: C# 14; actual compiler/SDK not verified. [Microsoft language versioning](https://learn.microsoft.com/dotnet/csharp/language-reference/language-versioning).
- Delivery: Markdown in the reviewer workspace. No repository implementation/configuration changes, builds or tests.

## Outcome and strengths

All eight report actions are connected. Five validated issues need attention; runtime acceptance is not established.

- General engineering: ReportingService separates aggregation from Spectre rendering in ReportingFeature. Customer deletion removes associated appointments (CustomerFeatures.cs:60), preventing orphaned bookings.
- C# language/runtime: Appointment is abstract, variants are sealed, and fixed options use enums. OfType<T>(), GroupBy, projections and a reusable popularity extension demonstrate relevant LINQ knowledge. Appointment.CompareTo compares date and then time.
- .NET ecosystem: Spectre menus and tables provide consistent interaction. List displays escape dynamic content. Appointment-conflict exceptions are handled at the UI boundary.
- Empty popularity queries produce a nullable result and display Unknown. The weekday report explicitly handles an entirely empty list. Microsoft Learn MCP verified the relevant .NET 10 MaxBy behavior.

Implementation references below are relative to src/Appointments.Console at the exact target commit.

## Prioritized mentoring findings

### F1 — High / Must-fix / Reliability: unescaped confirmation titles can terminate the application

Evidence: Features/CustomerFeatures.cs:58; Features/AppointmentFeatures.cs:135–138; UI/ConfirmationDialogue.cs:20–22. Names are interpolated into confirmation titles without escaping. Customer names are permitted to contain brackets.

Trigger: A customer named Alex [debug] is selected for deletion, or an associated appointment is selected for cancellation. Spectre parses the title as markup; an invalid style tag raises an exception. These paths have no exception handler, so termination can discard the in-memory session.

Obligation: Reliable inherited customer deletion and appointment cancellation, plus appropriate error handling (assignment 1b CRUD; assignment 2a additional considerations).

Verification: Static call tracing; official Spectre.Console 0.57.2 source confirms title rendering via new Markup(Title). Official Spectre documentation confirms invalid style tags throw. These are third-party API behaviors, not Microsoft MCP claims.

- [Version-specific SelectionPrompt source](https://raw.githubusercontent.com/spectreconsole/spectre.console/0.57.2/src/Spectre.Console/Prompts/SelectionPrompt.cs)
- [Official markup reference](https://spectreconsole.net/console/reference/markup-reference/)

Coaching direction: Trace user-provided names into markup-enabled confirmation titles and compare the handling with existing escaped table/picker displays. Preserve user text as data at this boundary.

### F2 — Medium / Must-fix / Correctness: weekend bookings are discarded by the weekday report

Evidence: Services/ReportingService.cs:90–104, particularly the Saturday/Sunday exclusion at line 93. Booking validation permits weekend dates.

Trigger: With appointments only on Saturday, the report evaluates five zero-count weekdays and can report Monday as both busiest and quietest. Unbooked weekend days are also omitted from the minimum.

Obligation: Assignment 2b requirement 8 and session 2 notes on zero-appointment days. Neither specifies a working-days restriction.

Verification: Direct source/control-flow reasoning; no framework-specific inference required.

Coaching direction: Align the reporting population with the days the booking workflow permits. The existing inclusion of zero-count days is useful, but its population is incomplete.

### F3 — Medium / Must-fix / Validation: phone rule contradicts the assignment

Evidence: Services/Validators/Validators.cs:9–12; Features/CustomerFeatures.cs:69–70. Both customer creation and update use a rule for ten digits beginning with 69.

Trigger: +123456 satisfies the assignment but is rejected. A local-format number without a leading plus can be accepted.

Obligation: Assignment 1b customer operations, line 17: plus symbol followed by at least six digits. The README also states that rule.

Verification: Direct comparison of the observed validator and authoritative requirement. This is a requirement mismatch, not a preference for a validation library.

Coaching direction: Reconcile the validation contract, prompt and README with the authoritative requirement.

### F4 — Medium / Must-fix / Correctness: invalid report dates silently select DateOnly.MinValue

Evidence: Features/ReportingFeature.cs:98–104. The prompt validator always returns true; the boolean returned by TryParseExact is ignored.

Trigger: Entering 31/02/2026 proceeds with DateOnly.MinValue and presents an appointment count for that value rather than rejecting invalid input.

Obligation: Assignment 2b requirement 2 and its guided reporting UI requirement.

Verification: Static tracing plus Microsoft Learn MCP search and full retrieval for .NET 10. Failed DateOnly.TryParseExact conversion returns false and writes DateOnly.MinValue to the out result. This is API behavior.

- [DateOnly.TryParseExact](https://learn.microsoft.com/en-us/dotnet/api/system.dateonly.tryparseexact?view=net-10.0)

Coaching direction: Make successful conversion part of accepting the report date. The important concept is that an out value does not independently prove parsing succeeded.

### F5 — Medium / Must-fix / Compliance: personal-training focus comments are not explicitly collected

Evidence: Features/AppointmentFeatures.cs:22, 28–40, 100 and 109–113. Optional general notes are collected, along with duration and injuries, but no prompt requests training-focus areas.

Trigger: A personal-training appointment can be completed without providing the focus information requested by the assignment.

Obligation: Assignment 2a Customer Comments, line 27.

Interpretation: General Notes could hold the comments; a separate property or class is not mandated. The gap is the interaction's failure to request the required information, rather than a prescribed data model.

Verification: Direct prompt/model inspection and requirement comparison; no framework-dependent claim.

Coaching direction: Ensure the interaction explicitly elicits training-focus information while retaining optional general booking notes.

Total significant findings: 5. Normal report: 5. Overflow: 0. Critical omitted: 0. Complete significant findings are F1–F5 above.

## C#, SOLID, design and engineering assessment

| Area | Evidence and assessment | Limit / obligation |
| --- | --- | --- |
| C# language knowledge | Appropriate abstract/sealed classes, enums, nullable values, required members, generic extension methods and comparison logic. LINQ usage is relevant to the task. | Feature use alone does not establish mastery. Parsing-result handling is a concrete weakness (F4). |
| Encapsulation and invariants | Lists are privately held and exposed as IReadOnlyList; conflict checks occur before appointment mutation. Customer updates directly mutate selected objects. | IReadOnlyList limits collection operations through that API; mutable entity properties remain mutable. At this scale, discuss ownership of invariants without prescribing layers. |
| Single responsibility / cohesion | Reporting queries and rendering are distinct. Feature classes orchestrate prompts; services own collection operations. | ReportingService returns some presentation strings/readable names. This is a small-app trade-off, not a significant defect or reason to require an abstraction. |
| Open/closed and polymorphism | Variant-specific data is represented by derived types; Service is overridden. Booking/update dispatch still names the two known variants. | A two-case dispatch is not automatically an OCP violation. No demonstrated extension pressure justifies a finding. |
| Liskov substitution | Both variants share the base booking fields and comparison contract. | No substitution defect was validated; this does not prove every hypothetical subtype would be safe. |
| Interface segregation | The small concrete services expose focused operations. | Interfaces are not required simply to demonstrate ISP. No consumer contract problem was validated. |
| Dependency inversion | Program explicitly composes concrete services and features. | DI containers and broader abstraction boundaries are outside this session. No DIP mandate is imposed. |
| Clean Architecture / patterns | Modest model/service/UI separation is present. | Clean Architecture begins at 5b. No architectural layering compliance judgment is made for 2b. Generic repositories, factories or extra projects would need a concrete reason. |
| Idiomatic C# / maintainability | Typed dates, times and options reduce string ambiguity. Shared popularity logic avoids copying the same aggregation. | Prefer reliable contracts over stylistic changes. No naming/style issue was elevated above demonstrated defects. |
| Reliability / senior engineering judgment | Conflict handling and deletion cleanup are useful safeguards. F1–F4 expose weaknesses in boundary handling, requirement fidelity and assumptions. | Assess these decisions directly; do not infer intent or a previous-language misconception. |
| Tests / verification discipline | No learner test project or execution evidence was identified. | Tests are not mandatory until session 7. No missing-test-project finding. Learner-run build/manual evidence is still needed to verify behavior. |
| Security / performance | Dynamic markup failure is assessed as local reliability (F1); no remote compromise claim. LINQ operates on in-memory lists. | No deployment exposure or demonstrated scaling problem established. No speculative optimization advice. |

This snapshot demonstrates familiarity with several C# features. It does not establish senior-level C# proficiency or overall production readiness. Broader evidence would be needed for debugging, testing strategy, operational trade-offs and sustained maintenance decisions.

## Assignment compliance and coverage

All evidence is static inspection. Met means sufficient source evidence for the stated criterion, not a runtime pass. B criteria are new for 2b; I criteria are inherited.

| ID | Criterion / source | Obligation | Evidence | Status |
| --- | --- | --- | --- | --- |
| B01 | Registered-customer count / 2b R1 | Required, new | ReportingService:8 | Met |
| B02 | Selected-date appointment count / 2b R2 | Required, new | ReportingService:13; F4 | Partially Met |
| B03 | Counts by service / 2b R3 | Required, new | ReportingService:25 | Met |
| B04 | Common masseuse preference / 2b R4 | Required, new | ReportingService:38 | Met |
| B05 | Common training duration / 2b R5 | Required, new | ReportingService:48 | Met |
| B06 | Busiest date per service / 2b R6 | Required, new | ReportingService:67 | Met |
| B07 | Common massage type / 2b R7 | Required, new | ReportingService:58 | Met |
| B08 | Busiest/quietest day / 2b R8, session 2 notes | Required, new | ReportingService:82; F2 | Partially Met |
| B09 | Guided reporting UI / 2b UI | Required, new | Program:56–61,82–90; F4 | Partially Met |
| B10 | LINQ / 2b considerations | Required, new | ReportingService, EnumerableExtensions | Met |
| B11 | Assignment 2b branch / 2b considerations | Required, new | Verified Git branch | Met |
| B12 | Assignment 2b PR / 2b considerations | Required, new | No PR evidence inspected | Not Verified |
| B13 | Application folder name / 1b and 2b structure | Required, ambiguous | Console versus ConsoleApp source conflict | Not Verified |
| I01 | Application README / 1a | Required, inherited | README describes application; minor stale details | Met |
| I02 | License / 1a | Required, inherited | LICENSE | Met |
| I03 | Modern .NET console target / 1b | Required, inherited | csproj:5 | Met |
| I04 | Create customer / 1b customer operations | Required, inherited | CustomerFeatures:11 | Met |
| I05 | List customers / 1b customer operations | Required, inherited | CustomerFeatures:21 | Met |
| I06 | Update customer / 1b customer operations | Required, inherited | CustomerFeatures:34 | Met |
| I07 | Delete customer / 1b customer operations | Required, inherited | CustomerFeatures:50; F1 | Partially Met |
| I08 | Email validation / 1b customer operations | Required, inherited | Validators:14–15; shared UI use | Met |
| I09 | International phone validation / 1b customer operations | Required, inherited | Validators:9; F3 | Not Met |
| I10 | Linked appointment, fields and unique ID / 1b appointments | Required, inherited | Appointment model; picker; creation | Met |
| I11 | Cancel appointment / 1b appointments | Required, inherited | AppointmentFeatures:131; F1 | Partially Met |
| I12 | Update appointment / 1b appointments | Required, inherited | AppointmentFeatures:94; service updates | Met |
| I13 | List appointments / 1b appointments | Required, inherited | AppointmentFeatures:70 | Met |
| I14 | In-memory lists / 1b persistence | Required, inherited | CustomerService, AppointmentService | Met |
| I15 | Massage options / 2a massage | Required, inherited | MassageType; creation/update | Met |
| I16 | Optional masseuse preference / 2a massage | Required, inherited | MasseuseGender; no-preference selection | Met |
| I17 | Training durations / 2a training | Required, inherited | TrainingDuration; creation/update | Met |
| I18 | Training-focus comments / 2a training | Required, inherited | Optional notes only; F5 | Partially Met |
| I19 | Injury/pain notes / 2a training | Required, inherited | AppointmentFeatures:30,112 | Met |
| I20 | Appropriate OOP / 2a considerations | Required, inherited | Abstract base and sealed variants | Met |
| I21 | Error handling / 2a considerations | Required, inherited | Conflict handling; F1/F4 | Partially Met |

Total active criteria: 34 = 24 Met + 7 Partially Met + 1 Not Met + 2 Not Verified. This is an inspection tally, not a quality score.

Ambiguities: No specific tie-breaking policy or zero-row presentation for absent service categories is prescribed. No defect is assigned merely for selecting one tied winner or omitting absent categories. No folder-name penalty is applied until the Console/ConsoleApp conflict is clarified.

Excluded: historical branch/PR/sync procedures from earlier assignments were not reconstructed; later-session database, DI, async, Clean Architecture and mandatory test-project expectations do not apply. No unelected optional feature is marked unmet.

## Execution-path coverage

| Workflow | Inspection state / method | Paths considered | Remaining limit |
| --- | --- | --- | --- |
| Eight reports | Inspected / static | Menu → feature → service → lists → rendering; empty/tied/zero-count cases | No runtime output |
| Customer CRUD | Inspected / static | Prompts, validation, picker/back/empty paths, updates, confirmation and cleanup | Runtime interaction unverified |
| Appointment CRUD | Inspected / static | Association, variants, date/time, conflicts, updates, cancellation | Runtime interaction unverified |
| Startup/navigation | Inspected / static | Program composition, menu dispatch, confirmation, seed option | Terminal behavior unverified |
| Application source | Inspected / static | All files under src/Appointments.Console | No execution |
| PR / external collaboration | Not inspected | No connected PR inspection performed | B12 remains Not Verified |

Scope completeness: Static application inspection complete for stated scope. Runtime acceptance, PR evidence and the folder-name interpretation remain incomplete/unverified. Later-session material and sample projects were not reviewed as learner implementation.

## Verification ledger and loaded instructions

- Microsoft Learn MCP search and full retrieval: DateOnly.TryParseExact and Enumerable.MaxBy, .NET 10. [MaxBy documentation](https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable.maxby?view=net-10.0). The popularity helper groups into reference-type groups; empty input can yield null and is handled. No invented empty-sequence exception finding.
- Microsoft Learn MCP search: C# framework-default language version; actual compiler remains unverified.
- Spectre official web documentation and version-tagged source: title markup behavior, version 0.57.2. This third-party verification is distinct from Learn MCP.
- Reviewer instructions loaded: AGENTS.md, targets.json, review-rubric.md; central reviewer skill; dotnet-assignment-compliance, dotnet-correctness and dotnet-design-assurance skills; coverage, mentor and mentee report templates.
- Template instructions/sources loaded: root AGENTS.md; docs/session1/AGENTS.md; docs/session2/AGENTS.md; assignment preface; assignments 1a/1b/2a/2b; learning objectives for 1b/2a/2b.
- Mentee instructions loaded: root AGENTS.md and docs/session1/AGENTS.md, docs/session2/AGENTS.md. No applicable nested instruction file under src identified.
- Compared selected instruction/assignment copies have matching textual content; Git blobs differ in line endings. No requirement substitution was needed.
- Restrictions preserved: learner owns implementation; no solutions, patches, test setups, builds/tests, dependency changes or mutating Git commands.
- No learner-supplied build/test output. Builds/tests were not executed; tests present would not imply tests passed.
- Final export verification: target branch/SHA unchanged and working tree clean; template SHA unchanged. Registry JSON remains unchanged.
- Organizational audience separation is not access control. Share only the separately prepared learner feedback after checking the destination's audience.

## Mentoring summary

Focus first on failure boundaries and requirement fidelity. C# syntax knowledge is visible; reliable interpretation of input/API contracts needs reinforcement. Discuss SOLID through concrete responsibility and ownership decisions rather than requesting extra architecture.

Suggested discussion questions:
1. What does the out value mean when a TryParse method returns false?
2. Which days belong in the reporting population, based on the booking contract?

## Suggested next step

Ask the learner to reproduce the bracket-containing customer-name confirmation failure and investigate that one rendering boundary first.

