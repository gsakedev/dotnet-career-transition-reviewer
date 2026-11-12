# Assignment 2b — Georgia: draft comments and mentor expectations

Audience: mentor only. Do not distribute this reference wholesale. Only the three quoted comment bodies below are prepared for learner-facing use.

Date: 2026-11-03 (Europe/Athens).

## Status and boundaries

- These are drafts for mentor review. No GitHub comments or pending review were created or submitted.
- The GitHub connector could not access the repository. Browser automation then stopped because it could not reliably determine the current browser URL. No publication was attempted through a submitting review tool.
- Repository: C:\Users\gsake\source\review-workspaces\mentee-georgia
- Branch: feature/assignment2b
- Reviewed commit: aecc662614b83a841f9ebe555980c42504ca6a69; clean working tree; uncommitted changes excluded.
- Locations below refer to that commit. Verify the PR head and current diff locations before attaching comments.
- Per the mentor's latest instruction, assume Assignment 2a corrections are fixed. This is an assumption, not verified implementation evidence. Do not repeat the 2a date, phone, naming or model-design comments as new 2b requirements.
- Assignment 2b's reporting-date helper is separate from the 2a booking-date helper and remains in scope.
- Source, configuration and Git metadata were not changed. No builds, tests or interactive application runs were performed.
- This document records the selected feedback, not a replacement for a full acceptance-criteria review.

## Draft comment 1 — Must-fix: invalid report dates

Location: src/Appointments.Console/Features/ReportingFeature.cs, line 100; relevant lines 98–104.

> **Must-fix — invalid report dates**
>
> The validation callback always returns `true`, and the result of `DateOnly.TryParseExact` is ignored. An impossible date such as `31/02/2026` therefore becomes `DateOnly.MinValue`, and the report runs for an unintended date.
>
> Please distinguish invalid input from a valid date with zero appointments, and handle conversion failure explicitly.

### What I expect from Georgia

- Invalid date input does not reach the reporting service as a default or unintended date.
- A valid date is interpreted consistently with the stated format.
- A valid date with no appointments remains a legitimate zero-count result.
- Historical report dates remain usable; do not impose the booking rule that dates must be today or later.
- She explains how conversion success determines whether the report runs.

This is a correctness requirement, not a request for a particular helper, exception type or architecture.

Supporting API behavior, previously retrieved through Microsoft Learn MCP for .NET 10:
https://learn.microsoft.com/en-us/dotnet/api/system.dateonly.tryparseexact?view=net-10.0

## Draft comment 2 — Must-fix: all permitted booking days

Location: src/Appointments.Console/Services/ReportingService.cs, line 93; relevant method starts at line 82.

> **Must-fix — include all permitted booking days**
>
> This filter excludes Saturday and Sunday, although the application permits appointments on those days. With appointments only on Saturday, the report can identify Monday as both busiest and quietest.
>
> Assignment 2b does not specify a Monday–Friday restriction. Please include all permitted booking days while preserving consideration of days with zero appointments.

### What I expect from Georgia

- Weekend appointments participate in the busiest/quietest-day calculation.
- Days with zero appointments remain candidates for the minimum.
- With no appointments, the application communicates that there is no report result.
- A selected day is a valid maximum or minimum when counts tie. The assignment does not mandate returning every tied day or a particular tie-breaking order.
- She explains the set of days the calculation considers and why.

Source: authoritative docs/session2/assignments/assignment2b.md, requirement 8, with docs/session2/AGENTS.md guidance on zero-appointment days. Template commit: fbe08912a9502cb03088882cf553d93cc439e582.

## Draft comment 3 — Optional: typed reporting results

Location: src/Appointments.Console/Services/ReportingService.cs, line 38. Related contracts appear at lines 25, 48, 58 and 67.

> **Optional — separate computation from presentation**
>
> Keeping report computation outside the console feature is a useful separation. Returning display labels and `"Unknown"` still gives the reporting service responsibility for presentation.
>
> Consider returning typed results that distinguish a value from no result, leaving formatting to the console feature. This is a design improvement, not an Assignment 2b blocker.

### What I expect from Georgia

- She can explain the trade-off between returning display strings and preserving typed values.
- She may improve the contracts or explain why she is keeping them for this stage.
- No mandatory refactor, extra project, DTO framework, interface hierarchy or named pattern follows from this comment.

This is a design recommendation based on the observed responsibilities, not a Microsoft API rule or assignment requirement.

## Project structure and model expectations

A new Domain project is not required for Assignment 2b.

- Keep the current single-project structure, including Models/ and Models/Enums/.
- An Entities/ folder is not required; changing a folder name does not establish encapsulation.
- The abstract Appointment and sealed variants remain an appropriate foundation.
- Constructor validation and read-only properties remain optional future design guidance from the Assignment 2a handoff.
- Scheduling conflicts reasonably remain a service responsibility because they involve multiple appointments.
- Domain/Application/Infrastructure/Console separation is introduced at Assignment 5b. Do not bring that acceptance requirement forward to 2b.
- Do not require DI containers, databases, async, additional layers or unit test projects here.

## Verification to request from Georgia

Ask her to supply a successful build and a short manual verification summary covering the two required corrections. Evidence should distinguish what she actually ran from what she inspected. No automated test project is required for this stage.

For the date report, the summary should include valid input, impossible input and a valid date with no appointments. For the weekday report, it should include weekend bookings, zero-appointment days and empty data. These are expected behavior categories, not supplied test implementations or data setups.

## Feedback scope

The selected feedback contains two must-fix comments and one optional comment. It is not a claim that every earlier broad-review observation has become an acceptance condition. Spectre rendering concerns are excluded from this focused comment set. Earlier optional naming and model-design guidance is not repeated as a requirement. The earlier training-focus-comments observation is inherited, outside these selected 2b comments, and is not a new merge condition here under the instruction to assume 2a items are fixed; its implementation is not independently reverified.

## Concrete next step

Ask Georgia to address the reporting-date failure handling first and share what happens for a valid date, an impossible date and a valid date with no appointments.