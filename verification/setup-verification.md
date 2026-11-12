# Infrastructure setup verification

Date: 2026-10-22 (Europe/Athens). Reviewer/rubric version: 1.0.0.

Status: infrastructure created; runtime discovery, template loading and actual Microsoft Learn search/retrieval succeeded. Configuration preservation has the limitation documented below. No mentee implementation was reviewed. No application build/test, dependency installation or mutating Git operation was executed.

## Created infrastructure

Workspace: `C:\Users\gsake\source\review-workspaces\dotnet-career-transition-reviewer`, outside the training-template repository. No Git repository was initialized.

- `AGENTS.md`: central reviewer safety, explicit selection, audience/privacy, coverage, cap/overflow and documentation policy.
- `review-rubric.md`: evidence/status/severity/obligation rules, session gates and known interpretation issues.
- `targets.json`: template reference, version, empty mentee registry, no selected mentee/session, mentor default and output policy.
- `.agents/skills/dotnet-career-transition-reviewer/SKILL.md`: orchestration and report routing.
- `.agents/skills/dotnet-assignment-compliance/SKILL.md`: criteria/evidence/coverage assessment.
- `.agents/skills/dotnet-correctness/SKILL.md`: relevant C#/runtime/async/data behavior.
- `.agents/skills/dotnet-design-assurance/SKILL.md`: relevant boundaries/DI/security/reliability/tests.
- `.agents/skills/dotnet-career-transition-reviewer/references/mentor-report-template.md`.
- `.agents/skills/dotnet-career-transition-reviewer/references/mentee-report-template.md`.
- `.agents/skills/dotnet-career-transition-reviewer/references/coverage-template.md`.
- This setup verification document and supporting evidence files listed below.

Created empty report roots: `reviews/mentor-only/` and `reviews/mentee/`. Per-mentee/session/review directories will be created only for an explicitly requested review. No fabricated mentee identifiers or reports were created.

The central agent is a role defined by AGENTS.md and the directly invocable orchestration skill; no native custom subagent or agent hierarchy was installed.

## Static validation

All four manifests passed the bundled skill-creator `quick_validate.py` validator. Their names match their directories and are unique. All local references from the central skill resolve. targets.json parses, has zero mentees and no selected target. The workspace is outside the configured template. See `static-validation.json`.

## Runtime instruction and skill discovery

A fresh local Codex CLI session was launched from the reviewer workspace with `--skip-git-repo-check --ephemeral --sandbox read-only`. It performed infrastructure checks only, with no subagents or implementation review.

- The reviewer AGENTS.md content was present in startup context before tool use. The verifier distinguishes startup presence from subsequent explicit reads; model self-report alone cannot independently prove the host's file-loading mechanism. The launch working directory and local file are controlled in this check.
- All four reviewer skills were advertised at startup by name and local path, before explicitly reading their manifests. All four were then successfully read.
- Reviewer AGENTS.md, targets.json and rubric were explicitly read as well.
- The underlying event stream ends with `turn.completed` and contains no `turn.failed` event. The outer PowerShell command reported exit code 1 despite the completed turn and saved final response; this is recorded rather than treating the wrapper exit as a clean process success. Runtime tool results independently demonstrate the checks below.
- stderr contains credential-store fallback warnings. They did not prevent the successful Learn calls.

See `discovery-and-mcp-result.md`, `discovery-and-mcp-events.jsonl` and `discovery-and-mcp-stderr.log`. The JSONL/stderr were redirected by Windows PowerShell and use UTF-16; the final response is UTF-8. These are mentor-owned setup evidence, not learner reports.

## Microsoft Learn registration and connectivity

Before registration, effective `codex mcp list --json` had no server for https://learn.microsoft.com/api/mcp. The authorized CLI operation was:

```powershell
codex mcp add microsoft-learn --url https://learn.microsoft.com/api/mcp
```

Effective listing afterwards contains exactly one enabled registration named `microsoft-learn` at that endpoint. Configuration file: `C:\Users\gsake\.codex\config.toml`.

```toml
[mcp_servers.microsoft-learn]
url = "https://learn.microsoft.com/api/mcp"
```

Preservation limitation: a semantic comparison immediately after CLI registration detected a change to the existing node_repl table as well. The exact pre-change TOML was not retained, so full source-setting preservation cannot be certified. Its effective command, enabled state, environment variable names and startup timeout match the pre-registration observations; no deliberate unrelated configuration edits were made. No speculative rollback was attempted. See `mcp-registration.json`. Do not claim byte-for-byte or fully verified preservation of all prior settings.

The fresh Codex session discovered current Learn search, fetch and code-sample tools. It executed:

1. Server `microsoft-learn`, tool `microsoft_docs_search`, query: `EF Core 10 DbContext concurrent asynchronous operations multiple parallel operations same DbContext await`.
2. Server `microsoft-learn`, tool `microsoft_docs_fetch`, URL: https://learn.microsoft.com/dotnet/api/microsoft.entityframeworkcore.dbcontext.savechangesasync?view=efcore-10.0

Both completed successfully with returned content and no tool error. The retrieved remarks establish that one DbContext does not support parallel operations, including concurrent async queries; operations should be awaited immediately or parallel work should use separate contexts. This was a generic connectivity probe, not a finding about mentee code. The URL targets EF Core 10 and retrieved content includes the v10.0.0 package/efcore-10.0 moniker, along with other version monikers. No web/CLI documentation fallback was used.

The host subsequently exposed the Microsoft Learn tools to this setup chat as well. A direct search (`EF Core 10 DbContext concurrent asynchronous operations SaveChangesAsync`) and fetch of the same EF Core 10 API page both succeeded here, with `isError=false` and 11,510 characters of retrieved Markdown. Thus actual retrieval was verified in both the fresh local Codex session and the setup chat. Start a fresh VS Code Codex chat to ensure current configuration and local skills are loaded.

## Training-template access and integrity

Template: `C:\Users\gsake\source\repos\dotnet-external-coaching-template`.

Verified commit: `fbe08912a9502cb03088882cf553d93cc439e582`.

All eight session AGENTS.md and eleven assignment files are readable from the reviewer workspace. The fresh Codex session explicitly loaded template root AGENTS.md, Session 5 and 6 instructions, assignments 5a/5b/6 and inventoried all sessions/assignments. It correctly distinguished the 5b four-project architecture from 5a and identified the Session 6 prohibition on console Application/Infrastructure references and direct database access. These were instruction-loading checks, not source implementation reviews.

Tracked template file SHA-256 values and Git status match the captured baseline. The existing untracked `docs/session6.7z` remains present and untouched. No mentee repository was selected or accessed for review.

## VS Code usage

1. File > Open Folder: `C:\Users\gsake\source\review-workspaces\dotnet-career-transition-reviewer`.
2. Start a fresh Codex chat with this folder as its working root. Use read-only permissions for analysis. Do not add mentee repositories as writable roots.
3. Use `/skills` or type `$` and confirm the four local skill names. Do not assume a chat already opened in another folder inherits them.
4. Invoke the central skill with an explicit target; the example below is a pattern, not a registered mentee or a review already performed:

```text
$dotnet-career-transition-reviewer
Review mentee <identifier> at <absolute mentee repository path>.
Session 6, HEAD only; exclude uncommitted changes.
Mode: mentor. Return the assessment in chat; do not save files.
```

For learner feedback, explicitly request `Mode: mentee` or both audiences. For saved reports, explicitly request saving within the appropriate reviewer report directory and use a session permitted to write there; no template/mentee writes are authorized. Session 5 requests must identify 5a, 5b or both. Progress comparisons must identify historical SHAs or report baselines from the same mentee.

CLI equivalent for a read-only session outside Git:

```powershell
codex --cd "C:\Users\gsake\source\review-workspaces\dotnet-career-transition-reviewer" --sandbox read-only
```

If the interactive CLI requires a Git workspace, use `codex exec --skip-git-repo-check --sandbox read-only --cd <reviewer-path> <prompt>`; do not initialize Git merely to bypass that check.

## Remaining limits

- No pilot review or behavioral assessment against a real mentee implementation has occurred. Skill metadata/discovery and setup checks do not prove future finding quality.
- Read-only CLI sandbox was requested; no write probe was performed. Instructions and audience folders are not OS access controls or a guarantee against model errors. Review sharing permissions before any distribution.
- Exact preservation of original node_repl source configuration is not certified, as described above.
- Fresh CLI discovery was exercised; VS Code UI discovery itself was not automated. Confirm `/skills` in the fresh IDE chat.
- Review scope/coverage are instruction-driven, not enforced by a custom execution engine. Builds/tests remain unexecuted.
- Future tool/schema/documentation or template changes require per-review discovery/version checks. No Microsoft documentation plugin/package was installed.
