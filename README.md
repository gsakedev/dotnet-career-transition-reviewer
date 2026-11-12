# .NET Career Transition Reviewer

A review workspace for Codex and Claude Code, for mentoring experienced engineers moving from another technology stack to C# and .NET.

The reviewer checks the engineer's implementation against the selected assignment, assesses the C#/.NET practices demonstrated in the code, and identifies useful next learning priorities. Reviews stay within the current curriculum stage and use evidence from an explicitly selected repository and commit.

## How the review works

1. Select one mentee repository, assignment/session, target snapshot and audience.
2. Load the authoritative assignments, learning objectives and applicable repository instructions.
3. Inspect the implementation and its callers using the three technical specialist skills.
4. Consolidate findings, strengths, assignment coverage and verification limits into the main assessment.
5. Automatically run the learning coach against the same evidence and code snapshot to produce a separate learning report.

The skills are reusable instructions followed by the coding assistant. They do not automatically launch separate agents, schedule background reviews or run learner applications.

Both tools use the same review contract, canonical skills and report templates. Claude Code has a thin entrypoint that reads those files directly; there is no conversion or compilation step after editing the shared instructions. This shares the workflow and criteria, but does not guarantee identical judgments or wording across models.

| Skill | Responsibility |
| --- | --- |
| [dotnet-career-transition-reviewer](.agents/skills/dotnet-career-transition-reviewer/SKILL.md) | Coordinates the review, selects relevant specialists, validates findings and produces the requested outputs. |
| [dotnet-assignment-compliance](.agents/skills/dotnet-assignment-compliance/SKILL.md) | Maps active assignment requirements to implementation evidence and coverage. |
| [dotnet-correctness](.agents/skills/dotnet-correctness/SKILL.md) | Checks applicable C#/.NET behavior, including LINQ, nullability, async, data access and resource handling. |
| [dotnet-design-assurance](.agents/skills/dotnet-design-assurance/SKILL.md) | Assesses design, boundaries, DI, security, reliability and testing at the appropriate curriculum stage. |
| [dotnet-learning-coach](.agents/skills/dotnet-learning-coach/SKILL.md) | Identifies demonstrated strengths, areas to strengthen and personalized next learning priorities. Runs automatically after normal reviews unless explicitly excluded. |

## Reports

Normal reviews produce **two reports for each requested audience**:

| Report | Contents |
| --- | --- |
| Main assessment | Assignment compliance, code quality, correctness, general engineering, C#/.NET understanding, strengths, prioritized findings and verification limits. |
| Learning Progress and Next Focus | Evidence-backed strengths and gaps, up to three learning priorities, small learner-owned activities, success indicators and one immediate next action. |

The main assessment currently combines code review and C#/.NET transition assessment. Coverage can be included in the assessment or provided as a separate supporting artifact.

Learning observations use `Demonstrated`, `Needs improvement` and `Not evidenced`. A concept that was not demonstrated is not automatically a defect or evidence that the engineer does not understand it. Progress claims require an explicit historical baseline from the same mentee.

## Repository layout

~~~text
dotnet-career-transition-reviewer/
├── AGENTS.md
├── CLAUDE.md
├── .mcp.json
├── review-rubric.md
├── targets.json
├── .claude/skills/
│   └── dotnet-career-transition-reviewer/SKILL.md
├── .agents/skills/
│   ├── dotnet-career-transition-reviewer/
│   │   ├── SKILL.md
│   │   └── references/
│   ├── dotnet-assignment-compliance/
│   ├── dotnet-correctness/
│   ├── dotnet-design-assurance/
│   └── dotnet-learning-coach/
│       ├── SKILL.md
│       └── references/learning-focus-template.md
├── reviews/
│   ├── mentor-only/
│   └── mentee/
└── verification/
~~~

[AGENTS.md](AGENTS.md) defines the review contract. [review-rubric.md](review-rubric.md) defines evidence standards, severity, coverage and curriculum boundaries. [targets.json](targets.json) holds local paths, optional mentee registrations and selection/output settings.

[CLAUDE.md](CLAUDE.md) imports the shared contract. The [Claude entrypoint](.claude/skills/dotnet-career-transition-reviewer/SKILL.md) loads the canonical reviewer from `.agents/skills/`. [.mcp.json](.mcp.json) supplies the Claude project Microsoft Learn server configuration.

## Setup

### 1. Prepare separate local checkouts

Use Codex or Claude Code with local file access, Git, the authoritative training-template checkout and the mentee checkout you intend to review. Keep this reviewer repository separate from both:

~~~text
C:\review-workspaces\
├── dotnet-career-transition-reviewer\
├── training-template\
└── mentee-example\
~~~

These are example paths; the repositories do not have to share a parent directory. Clone this repository into your chosen reviewer location:

~~~powershell
git clone https://github.com/gsakedev/dotnet-career-transition-reviewer.git
~~~

The reviewer performs static inspection. Learners run their own builds, tests and application checks and can supply the results as additional evidence.

### 2. Configure your local paths

Edit the corresponding fields in the existing `targets.json`, preserving its other settings. The paths shipped in the file are machine-specific. For example:

~~~json
{
  "reviewer_workspace": "C:\\review-workspaces\\dotnet-career-transition-reviewer",
  "training_template": {
    "repository": "C:\\review-workspaces\\training-template"
  },
  "mentees": [
    {
      "id": "mentee-example",
      "repository": "C:\\review-workspaces\\mentee-example"
    }
  ]
}
~~~

This is an excerpt, not a replacement for the entire configuration file. A mentee entry uses a stable `id` and an absolute `repository` path. Use doubled backslashes in JSON strings on Windows, or forward slashes.

Registration is optional: `"mentees": []` is valid, and you can supply a repository path directly in a review request. Registration does not select a default mentee. Always explicitly identify the repository and session/assignment part for each review.

The template's `discovery_commit` records the original discovery snapshot. Each review resolves and records the actual template SHA and reports relevant drift.

From the reviewer folder, check JSON syntax with PowerShell:

~~~powershell
Get-Content -LiteralPath .\targets.json -Raw | ConvertFrom-Json | Out-Null
~~~

### 3. Open the reviewer folder in your coding tool

#### Codex

Use this repository as the working folder and start a fresh chat. In the CLI or IDE extension, use `/skills` or type `$` to confirm the five skills are available. Codex discovers repository skills under `.agents/skills`. See the official [skills documentation](https://learn.chatgpt.com/docs/build-skills) and [AGENTS.md instructions](https://learn.chatgpt.com/docs/agent-configuration/agents-md).

For a CLI session that returns the review in chat:

~~~powershell
codex --cd "C:\review-workspaces\dotnet-career-transition-reviewer" --sandbox read-only
~~~

#### Claude Code

Start an interactive Claude Code session from the reviewer root:

~~~powershell
Set-Location -LiteralPath "C:\review-workspaces\dotnet-career-transition-reviewer"
claude --permission-mode plan
~~~

Plan mode is intended for analysis without editing project files. Ask for the review reports in chat, not an implementation plan. See Claude Code's [permission modes](https://code.claude.com/docs/en/permissions).

The root `CLAUDE.md` uses `@AGENTS.md` to load the shared contract. Claude discovers one native entrypoint under `.claude/skills/`; invoke `/dotnet-career-transition-reviewer` and supply the review details below. That entrypoint reads the canonical central skill, which loads the applicable specialists and automatically includes the learning coach. There is no need to copy the five canonical skills or invoke the coach separately. See the official [instruction imports](https://code.claude.com/docs/en/memory) and [Claude Code skills](https://code.claude.com/docs/en/skills) documentation.

Keep learner and template repositories read-only in either tool. Instruction files describe behavior; they do not enforce operating-system permissions. Grant only the access needed for inspection. If you explicitly request saved reports, use a session that permits writing to the appropriate reviewer report directory. Opening a sibling mentee folder does not load this reviewer's instructions automatically.

### 4. Connect Microsoft Learn

The reviewer uses Microsoft Learn MCP to verify framework-dependent claims. Complete the setup for your coding tool before a review.

#### Codex

First check existing registrations:

~~~powershell
codex mcp list
~~~

If the Microsoft Learn endpoint is not already configured, add it:

~~~powershell
codex mcp add microsoft-learn --url https://learn.microsoft.com/api/mcp
codex mcp list
~~~

The endpoint is documented in Microsoft's [MCP developer reference](https://learn.microsoft.com/en-us/training/support/mcp-developer-reference). Codex configuration is described in the official [MCP documentation](https://learn.chatgpt.com/docs/extend/mcp?surface=cli).

#### Claude Code

The repository already includes a project-scoped `.mcp.json` with the Microsoft Learn HTTP endpoint. It does not modify user-wide Claude or Codex configuration. In your interactive Claude session, review the workspace trust and project-server approval prompts. Inspect the server with `/mcp`; from a terminal in the reviewer folder, you can also use:

~~~powershell
claude mcp list
~~~

Do not add another registration if Microsoft Learn is already available. Check the effective server configuration if an existing local registration conflicts with the project entry. See Claude Code's [project-scoped MCP setup](https://code.claude.com/docs/en/mcp).

#### Verify documentation access

Confirm an actual generic documentation search and page retrieval in the review environment; a configured-server listing alone does not prove connectivity. For example, ask your assistant to search and retrieve official documentation about `DateOnly.TryParseExact`. Keep private code and mentee identifiers out of documentation queries.

If MCP is unavailable, the reviewer discloses the limitation and labels any official-web fallback. Reviews do not install tools or change MCP configuration automatically.

## Request a review

Invoke the central skill; it coordinates the specialists and automatically includes the learning report:

| Tool | Invocation |
| --- | --- |
| Codex | `$dotnet-career-transition-reviewer` |
| Claude Code | `/dotnet-career-transition-reviewer` |

The request fields, review criteria and reports are the same in either tool.

### Full assignment review

Replace the example ID, path, branch and session with the intended target. In Claude Code, replace the first line with `/dotnet-career-transition-reviewer`:

~~~text
$dotnet-career-transition-reviewer

Review mentee mentee-example.
Repository: C:\review-workspaces\mentee-example
Session/part: 2b.
Expected branch: feature/assignment2b.
Target: current HEAD; resolve and record the full SHA.
Scope: full committed snapshot, including relevant inherited requirements.
Uncommitted changes: exclude.
Audience: mentor.
Previous stack: Node.js/TypeScript.
Return the reports in chat; do not save files.
~~~

Previous-stack background is optional; omit it when unknown. The reviewer must not infer it from code style. A dirty working tree is not permission to include uncommitted changes in a HEAD-only review.

### Learner feedback and saved reports

In an otherwise complete review request, choose `Audience: mentee` for learner feedback or `Audience: both` for separate mentor and learner outputs. To save the results, explicitly add:

~~~text
Save the requested reports under the appropriate reviewer reviews directories
using a new, unique review ID. Do not overwrite existing reports.
~~~

The default is mentor output in chat. Saved artifacts use:

~~~text
reviews/mentor-only/<mentee-id>/session-<NN>/<review-id>/
reviews/mentee/<mentee-id>/session-<NN>/<review-id>/
~~~

Review IDs contain the date, target short SHA and a unique suffix. The saved learning report is named `next-learning-focus.md`. Learner reports are independently written from selected validated evidence and do not expose private mentor commentary.

### Focused, change and progress reviews

Keep the explicit repository, session, target and audience fields, then adjust the scope:

| Review type | Additional input |
| --- | --- |
| Focused | Name the workflows or concerns to inspect, such as LINQ reporting and empty-data behavior. |
| Change | Supply explicit base and target commits so introduced issues can be distinguished from pre-existing ones. |
| Progress | Supply the same mentee's historical SHA or identified report baseline and the current target. |

The learning coach keeps the same inspected scope. Without a comparison baseline, it reports current evidence and marks progress comparison as not assessed.

## Curriculum boundaries

The authoritative assignment and session instructions govern each review. The rubric provides navigation and interpretation guidance:

| Stage | Main focus |
| --- | --- |
| 1a | Git workflow, README and license |
| 1b | C# fundamentals, in-memory customer/appointment CRUD and console UI |
| 2a / 2b | OOP and error handling / LINQ reporting |
| 3 | Users, roles, hashing, DI, logging, Object Context and UI Factory |
| 4 | EF Core persistence, relationships and configuration |
| 5a / 5b | Async, cancellation and HTTP / Clean Architecture boundaries |
| 6 | ASP.NET Core API, authentication and console HTTP client |
| 7 | xUnit/Moq behavior tests; optional API integration tests |
| 8 | Blazor WebAssembly consuming the API |

Clean Architecture is required from 5b; required test projects start in 7. Early exposure to a concept does not automatically make it a requirement in an earlier assignment.

## Evidence, privacy and sharing

- Findings must identify a reachable scenario, concrete impact and supporting code. Severity and `Must-fix`/`Optional` obligation are separate.
- Acceptance criteria use `Met`, `Partially Met`, `Not Met` or `Not Verified`, with reconciled totals and visible inspection gaps.
- Normal feedback has at most five consolidated findings across the main and learning reports for each audience. Significant overflow is retained privately and disclosed; critical overflow requires mentor triage.
- Reviews preserve learner ownership: no learner/template edits, complete assignment solutions, application execution, builds/tests or automatic Git publication. Static inspection is distinguished from learner-supplied execution evidence.
- Mentor-only folders are an organizational convention, not access control. The current `.gitignore` does not exclude `reviews/`, `verification/` or `targets.json`. Review repository access and tracked contents before sharing; ignore rules do not remove files already committed.
- Share the reviewer instructions and skills with sanitized configuration. Keep private assessments, personal identifiers, credentials and verification logs out of distributable kits.

The `verification/` directory contains historical setup evidence. It does not establish current MCP connectivity, skill discovery or the correctness of a future review.

## Troubleshooting

- **Codex skills are missing:** confirm the reviewer working folder and the `.agents/skills` files, then restart the session if needed.
- **Claude's reviewer command is missing:** confirm `.claude/skills/dotnet-career-transition-reviewer/SKILL.md` exists in the reviewer checkout and start a fresh session there. Only the central reviewer has a Claude slash command; the other canonical skills load through its workflow.
- **Claude cannot find a template or specialist:** keep `.agents/skills/` alongside the adapter; copying only `.claude/` is insufficient. Resolve canonical relative references from the original `.agents/skills/` directory.
- **Microsoft Learn is unavailable:** inspect the active host's MCP configuration and project approval status, then verify a generic search and page retrieval. A review must disclose unavailable MCP and any official-web fallback.
- **The target is ambiguous:** provide the repository path, session/part, branch and target SHA or HEAD explicitly. A registry entry does not authorize implicit selection.
- **A review refers to old code:** provide matching review/code snapshots or an explicit comparison baseline. The reviewer stops dependent conclusions when the target drifts.
- **Reports were not saved:** saving must be requested, and the chosen report directory must be writable in that session.

For the detailed operating rules, see [AGENTS.md](AGENTS.md), [review-rubric.md](review-rubric.md) and the [central reviewer skill](.agents/skills/dotnet-career-transition-reviewer/SKILL.md).
