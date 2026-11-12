# Assignment 2a review handoff — Georgia

Audience: mentor only. This file is a continuation aid, not learner-facing feedback or a new Assignment 2b review.

Date: 2026-11-03 (Europe/Athens).

## Current status

**The mentor confirmed that the fixes have not been made.** Treat the two required corrections below as outstanding. This status is mentor-supplied; no fresh implementation review of Assignment 2b was performed when preparing this handoff.

Four comments were published on Assignment 2a PR #3: the date-conversion comment was published by the mentor; the naming, phone-message and model-design comments were subsequently submitted together as a Comment review. No approval or merge was performed by the assistant. PR state after that publication has not been refreshed for this handoff.

PR: https://github.com/agileactors/dotnet-onboarding-coaching-georgiabali/pull/3

## Review snapshot and boundaries

- Reviewer workspace: `C:\Users\gsake\source\review-workspaces\dotnet-career-transition-reviewer`
- Explicit mentee repository: `C:\Users\gsake\source\review-workspaces\mentee-georgia`
- Mentee ID: `georgia`
- Reviewed session/part: Session 2, Assignment 2a, with relevant inherited Assignment 1b behavior.
- Review mode: full committed snapshot; mentor audience; no historical comparison baseline.
- Reviewed branch: `feature/assignment2a`
- Reviewed SHA: `d722008683ca360394ecbd03075494cbdaa20644`
- Working tree was clean during the original static review; uncommitted content was excluded.
- Authoritative template: `C:\Users\gsake\source\repos\dotnet-external-coaching-template`
- Template SHA recorded during review: `fbe08912a9502cb03088882cf553d93cc439e582`
- Requirements: `docs/session2/assignments/assignment2a.md`, `docs/session2/session2a.md`, inherited `docs/session1/assignments/assignment1b.md` and relevant session guidance.
- Reviewer/rubric version: 1.0.0.
- No application builds, tests or interactive runs were performed. Source files were not edited by the assistant.
- Previous-stack background: not supplied.

The mentor explicitly narrowed the emphasis to C#/.NET correctness, OOP, structure, naming and design principles. Spectre rendering issues were excluded from the final requested comments. Do not reintroduce those as unresolved PR requirements from this handoff.

The earlier broad review is not reproduced here. This file records the final published comments and agreed obligations; it is not a replacement for a full acceptance-criteria matrix.

## Published comments and expected actions

Paths and line numbers below refer to the reviewed Assignment 2a SHA. Recheck locations and callers in any later snapshot.

### 1. Required: date conversion consistency

Location: `src/Appointments.Console/Features/AppointmentFeatures.cs:159`

Published text:

> Must-fix - date conversion
>
> IsValidDate validates with InvariantCulture, but this second TryParseExact uses the current culture and its success flag is ignored. With different calendar settings, accepted input can produce a different date or a failed conversion can leave DateOnly.MinValue.
>
> Please make validation and conversion consistent and account for conversion failure. Hint: compare this overload with the one used in Validators.IsValidDate.

Expected outcome:

- Validation and conversion interpret the input using consistent culture/calendar rules.
- Conversion failure cannot silently become a stored default date.
- Check both appointment creation and updates, since they share this helper.

Status: **Outstanding, confirmed by mentor.** Do not claim runtime reproduction or successful correction.

Supporting API behavior was retrieved through Microsoft Learn MCP:
https://learn.microsoft.com/en-us/dotnet/api/system.dateonly.tryparseexact?view=net-10.0

### 2. Required, minor: phone validation message

Location: `src/Appointments.Console/Features/CustomerFeatures.cs:70`

Published text:

> Must-fix (minor) - phone validation message
>
> In AskPhone (line 70), the message says "10 digits starting with 69", but IsValidPhone expects + followed by at least six digits, matching the assignment. Please align the message with the actual rule so users know what input is accepted. Hint: the validator itself already follows the required format.

Expected outcome:

- Error text describes the international-format rule: `+` followed by at least six digits.
- Do not ask her to replace the existing rule with a local-number rule: the reviewed validator already matches the assignment.

Status: **Outstanding, confirmed by mentor.**

### 3. Optional: consistent input-method naming

Location: file-level comment on `src/Appointments.Console/Features/AppointmentFeatures.cs`; declaration discussed at line 157.

Published text:

> Optional - naming
>
> Consider PromptForDate (line 157), PromptForTime and PromptForNotes to make the interactive behavior clear. GetDateFromUser is also reasonable. Choose one naming convention and apply it consistently; this is a readability suggestion, not an Assignment 2a requirement.

Expected response: she may adopt consistent names or explain her naming choice. `PromptFor...` is the suggested convention; `Get...FromUser` is also acceptable. This is not a C# language requirement or merge blocker.

### 4. Optional: model design direction

Location: file-level comment on `src/Appointments.Console/Models/Appointment.cs`.

This wording replaced the earlier pending encapsulation discussion before publication:

> Optional - model design direction
>
> Your appointment hierarchy is a good foundation. The next design goal is to make invalid state harder to create and prevent callers from changing properties without following the relevant rules. Start by considering constructor-based initialization and read-only appointment properties. We can then discuss how updates would replace an appointment while preserving its identity.
>
> This is a learning direction for a later refactor, not a merge-blocking requirement for Assignment 2a.

Expected response: discuss the direction and trade-offs. No blanket immutability, constructor, factory or folder refactor is required for the Assignment 2a merge.

## Agreed structural assessment

- The abstract `Appointment` with sealed `MassageAppointment` and `PersonalTrainingAppointment` variants is appropriate for Assignment 2a.
- Common booking information belongs in the base class; subtype-specific information belongs in each concrete class.
- `abstract Service` requires concrete classes to supply their service. Their fixed overrides prevent a massage object from identifying itself as personal training. This is a useful existing invariant.
- `IComparable<Appointment>` compares date first and time second for ordering.
- Features, services and models have recognizable responsibilities; extra layers/interfaces/patterns are not required at this stage.
- Existing `Models/` and `Models/Enums/` folders are acceptable. An `Entities/` folder is not required.
- Getter-only properties can prevent reassignment. Private setters alone provide controlled mutability, not full immutability.
- A referenced mutable `Customer` prevents an appointment from being deeply immutable, even when the appointment's customer reference is getter-only.
- Constructors establish initial state; public operations or immutable replacements govern later changes. Factories are optional, not automatically better than public constructors.
- Scheduling conflicts involve other appointments and reasonably remain a service responsibility.
- Immutable replacements require explicit identity preservation and a decision about whether appointments retain customer snapshots or reflect later customer changes.

The mentor prefers constructor validation and read-only models as a future architectural direction. Preserve that preference while distinguishing it from Assignment 2a acceptance requirements. Do not describe conceptual reference code discussed in chat as an applied or tested implementation.

## Solution file: already present in Assignment 2a

The reviewed commit adds the repository-root solution:

`dotnet-onboarding-coaching-georgiabali.slnx`

It includes:

`src/Appointments.Console/Appointments.Console.csproj`

Therefore, do not report that the solution was first added in 2b or 3. `.slnx` is a supported solution format; an older `.sln` file is not needed unless an actual tooling compatibility constraint requires it.

Documentation retrieved through Microsoft Learn MCP:
https://learn.microsoft.com/dotnet/core/tools/dotnet-sln

## Merge expectations for Assignment 2a

Request the date-conversion correction and phone-message correction. Ask the learner to supply a successful build and manual creation/update verification. Neither has been independently executed by the assistant.

Naming and model design remain optional. Do not turn the future design direction into an unannounced merge requirement.

## Starting point for the next Assignment 2b review

Observed while writing this handoff:

- Repository: `C:\Users\gsake\source\review-workspaces\mentee-georgia`
- Current branch: `feature/assignment2b`
- Current HEAD: `aecc662614b83a841f9ebe555980c42504ca6a69`
- Working tree: clean.

This is a new snapshot. Do not apply 2a line numbers, counts or conclusions automatically to it. The mentor states that the required corrections remain unfixed; verify their actual presence in 2b and identify them as inherited where supported.

Reload reviewer, authoritative template/session and mentee instructions. Verify Git root, branch, full SHA and dirty state again. Read Assignment 2b plus inherited 2a/1b requirements. Review all eight LINQ reports, callers and reachable empty/tie/zero-day cases within the actual assignment wording. Assess C#/.NET and scope-appropriate design separately. Do not impose DI, databases, async, Clean Architecture or test projects.

Known reviewer-infrastructure issue: `targets.json` contains invalid JSON (`"mentees": [georgia]`). The explicitly supplied repository allowed the review to proceed. Do not repair configuration without a separate mentor request.

### Suggested prompt for the new chat

```text
Use $dotnet-career-transition-reviewer for a full mentor-facing Assignment 2b review of:
C:\Users\gsake\source\review-workspaces\mentee-georgia

Read this mentor-only handoff first:
C:\Users\gsake\source\review-workspaces\dotnet-career-transition-reviewer\reviews\mentor-only\georgia\session-02\2026-11-03-d722008-handoff-1716-a7c9\assignment2a-handoff.md

Review committed HEAD on feature/assignment2b after verifying branch, SHA and dirty status; exclude uncommitted changes. Return results in chat. This is a snapshot review, not a historical progress comparison.

The Assignment 2a date-conversion and phone-message corrections remain outstanding according to me; verify their presence in this snapshot. Naming and immutable-model guidance were optional. Focus on C#/.NET correctness, LINQ, OOP and justified structure within Session 2. Keep the repositories read-only and do not run builds or tests. Do not use sub-agents.
```