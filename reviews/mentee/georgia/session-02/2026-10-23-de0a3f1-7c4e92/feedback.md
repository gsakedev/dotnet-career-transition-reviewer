# Assignment 2b — feedback for Georgia

## Review context

- Date: 23 October 2026.
- Repository: mentee-georgia.
- Branch: feature/assignment2b.
- Reviewed commit: de0a3f1ec1932ef269a83a465c52b4e6b17f14e0.
- Scope: Full assignment 2b snapshot, including relevant inherited requirements. Uncommitted changes excluded; working tree clean.
- Requirements: authoritative training template at fbe08912a9502cb03088882cf553d93cc439e582, assignments 2b, 2a and 1b, plus README/license obligations from 1a and session 1/2 mentor notes.
- Environment declared by the project: .NET 10, nullable reference types enabled, Spectre.Console 0.57.2.
- Evidence: Source inspection only. No application build, test or interactive execution was performed.

## Strengths

You have connected all eight reports to the menu. ReportingService holds the aggregation logic separately from ReportingFeature's console rendering, which makes the calculations easier to reason about.

The appointment variants use an abstract base class, sealed concrete types and enums for fixed options. Your use of OfType<T>(), grouping and the shared popularity extension demonstrates relevant C# and LINQ skills.

Customer deletion also removes associated appointments. Appointment conflicts are checked before changes are applied, and their exceptions are handled by the UI. These are useful safeguards.

All implementation references below are relative to src/Appointments.Console at the reviewed commit.

## Things to reconsider

### F1 — High · Must-fix · Reliability: customer names enter confirmation markup

In Features/CustomerFeatures.cs:58 and Features/AppointmentFeatures.cs:135–138, the customer's name is inserted into a confirmation title without escaping. UI/ConfirmationDialogue.cs:22 passes the title to Spectre.

A name such as Alex [debug] is accepted as customer input but can cause the confirmation to throw while rendering. There is no handler on these paths, so the application can close and lose the current in-memory records.

This affects inherited deletion/cancellation functionality and the error-handling requirement. Spectre.Console 0.57.2 parses selection titles as markup; invalid tags can throw. See the [version-specific source](https://raw.githubusercontent.com/spectreconsole/spectre.console/0.57.2/src/Spectre.Console/Prompts/SelectionPrompt.cs) and [official markup reference](https://spectreconsole.net/console/reference/markup-reference/).

Focus: Trace how customer text reaches confirmation rendering. Compare it with the places where your tables already preserve user text safely.

### F2 — Medium · Must-fix · Correctness: weekend bookings disappear from the day report

Services/ReportingService.cs:93 excludes Saturday and Sunday, although appointment creation permits those dates.

If all appointments are on Saturday, the report evaluates only five zero-count days and can show Monday as both busiest and quietest. Assignment 2b report 8 does not restrict reporting to working days.

Focus: Use the booking requirements to determine the complete population of days being measured. Keep the useful zero-count handling you already considered.

### F3 — Medium · Must-fix · Validation: the phone rule differs from the assignment

Services/Validators/Validators.cs:9–12 and Features/CustomerFeatures.cs:69–70 enforce ten digits beginning with 69.

Assignment 1b requires a plus symbol followed by at least six digits. A value such as +123456 should satisfy that requirement but is rejected by the current rule. Creation and update both use it.

Focus: Reconcile the validator, prompt and README with the specified international format.

### F4 — Medium · Must-fix · Correctness: an invalid report date is accepted

Features/ReportingFeature.cs:98–104 uses an always-successful prompt validator and ignores TryParseExact's boolean result.

For example, 31/02/2026 proceeds with DateOnly.MinValue and produces a report for that value. A populated out variable does not mean parsing succeeded.

This affects assignment 2b's selected-date report and input guidance. Microsoft Learn MCP verified the .NET 10 API behavior: failure returns false and writes DateOnly.MinValue. See [DateOnly.TryParseExact](https://learn.microsoft.com/en-us/dotnet/api/system.dateonly.tryparseexact?view=net-10.0).

Focus: Treat successful conversion as part of accepting the selected date.

### F5 — Medium · Must-fix · Requirements: training-focus comments are not requested

Features/AppointmentFeatures.cs:22 collects optional general notes. The training-specific prompts request duration and injuries, but do not explicitly request the body areas the customer wants to focus on.

Assignment 2a asks for those comments to help the trainer prepare. General notes could store them; a separate property is not required. The interaction currently permits the required information to be missed.

Focus: Make the training interaction explicitly collect the focus information without turning optional general notes into an unrelated requirement.

Significant findings: 5. Included: 5. Omitted: 0. Critical omitted: 0.

## Learning focus

- General engineering: Keep the separation between calculation and rendering. Concentrate on requirement fidelity, the population a statistic measures, and failure paths at UI boundaries.
- C# language/runtime: Typed dates, enums, inheritance, nullable values and LINQ are useful choices here. Strengthen how you use API contracts, especially the relationship between a TryParse return value and its out parameter.
- .NET ecosystem: Spectre has markup-aware surfaces, so rendering user data requires consistent treatment. Your existing escaped list/table displays provide a useful point of comparison.
- SOLID/design: The current separation has value. There is no requirement to add interfaces, a DI container or more layers for this assignment. Explain responsibility boundaries and invariants through the actual code.
- Clean Architecture: It becomes an assignment requirement at 5b. It is not a missing feature in this review.

Discussion questions:
1. What should a caller conclude about the out value when parsing returns false?
2. Which days should participate in the minimum calculation when no bookings exist on those days?

## Assignment compliance and review coverage

Met means supported by inspected source, not executed successfully. New criteria come from 2b; inherited criteria remain active from earlier assignments. Sources are relative to the template's docs/sessionN/assignments directories.

| ID | Criterion / source | Lineage / obligation | Evidence | Status |
| --- | --- | --- | --- | --- |
| B01 | Registered-customer count / 2b R1 | New, required | ReportingService:8 | Met |
| B02 | Selected-date count / 2b R2 | New, required | ReportingService:13; F4 | Partially Met |
| B03 | Counts by service / 2b R3 | New, required | ReportingService:25 | Met |
| B04 | Common masseuse preference / 2b R4 | New, required | ReportingService:38 | Met |
| B05 | Common training duration / 2b R5 | New, required | ReportingService:48 | Met |
| B06 | Busiest date per service / 2b R6 | New, required | ReportingService:67 | Met |
| B07 | Common massage type / 2b R7 | New, required | ReportingService:58 | Met |
| B08 | Busiest/quietest day / 2b R8, session notes | New, required | ReportingService:82; F2 | Partially Met |
| B09 | Guided reports UI / 2b UI | New, required | Program menu/report routes; F4 | Partially Met |
| B10 | LINQ / 2b considerations | New, required | ReportingService, EnumerableExtensions | Met |
| B11 | Assignment 2b branch / 2b considerations | New, required | Verified branch | Met |
| B12 | Assignment 2b PR / 2b considerations | New, required | PR not inspected | Not Verified |
| B13 | Application folder name / 1b, 2b structure | Required, ambiguous | Console/ConsoleApp source conflict | Not Verified |
| I01 | Application README / 1a | Inherited, required | README | Met |
| I02 | License / 1a | Inherited, required | LICENSE | Met |
| I03 | Modern .NET console target / 1b | Inherited, required | csproj:5 | Met |
| I04 | Create customer / 1b | Inherited, required | CustomerFeatures:11 | Met |
| I05 | List customers / 1b | Inherited, required | CustomerFeatures:21 | Met |
| I06 | Update customer / 1b | Inherited, required | CustomerFeatures:34 | Met |
| I07 | Delete customer / 1b | Inherited, required | CustomerFeatures:50; F1 | Partially Met |
| I08 | Email validation / 1b | Inherited, required | Validators:14–15; shared UI use | Met |
| I09 | International phone validation / 1b | Inherited, required | Validators:9; F3 | Not Met |
| I10 | Linked appointment, booking fields, unique ID / 1b | Inherited, required | Models, picker, creation | Met |
| I11 | Cancel appointment / 1b | Inherited, required | AppointmentFeatures:131; F1 | Partially Met |
| I12 | Update appointment / 1b | Inherited, required | Feature and service updates | Met |
| I13 | List appointments / 1b | Inherited, required | AppointmentFeatures:70 | Met |
| I14 | In-memory lists / 1b | Inherited, required | CustomerService, AppointmentService | Met |
| I15 | Massage options / 2a | Inherited, required | MassageType; booking/update | Met |
| I16 | Optional masseuse preference / 2a | Inherited, required | No-preference option | Met |
| I17 | Training durations / 2a | Inherited, required | TrainingDuration; booking/update | Met |
| I18 | Training-focus comments / 2a | Inherited, required | F5 | Partially Met |
| I19 | Injury/pain notes / 2a | Inherited, required | AppointmentFeatures:30,112 | Met |
| I20 | Appropriate OOP / 2a | Inherited, required | Base class and variants | Met |
| I21 | Error handling / 2a | Inherited, required | Conflict handling; F1/F4 | Partially Met |

Total: 34 = 24 Met + 7 Partially Met + 1 Not Met + 2 Not Verified. These counts describe inspection coverage, not a score.

## Verification and limitations

- Inspected all application source files under src/Appointments.Console.
- Traced all eight menu/report paths, including empty, tied and zero-count cases.
- Inspected customer and appointment CRUD, input validation, selection/back/empty paths, conflict checks, deletion cleanup, startup, navigation and seed-data loading.
- No build, test or interactive runtime execution; no supplied execution output. Runtime behavior remains unverified.
- Microsoft Learn MCP search and retrieval checked .NET 10 DateOnly.TryParseExact and MaxBy behavior. Empty popularity groups can produce null, which the helper handles. [MaxBy API](https://learn.microsoft.com/en-us/dotnet/api/system.linq.enumerable.maxby?view=net-10.0).
- Spectre behavior was checked using official web documentation and source tagged 0.57.2.
- No PR inspected. The authoritative folder wording conflicts between Console and ConsoleApp, so no folder-name defect is assigned.
- Tie-breaking and zero-row presentation for absent services are unspecified; no extra policy is imposed.
- Historical Git procedures and unrelated later-session material/sample implementations were excluded. No future-session database, DI, async, architecture or mandatory test-project requirements were applied.
- Relevant assignment/instruction copies matched the authoritative template's textual content. The reviewed branch and commit remained unchanged.
- Static inspection does not establish overall readiness.

## Suggested next step

Reproduce the confirmation problem using a customer name containing brackets, then investigate that one rendering boundary before moving to the other findings.

