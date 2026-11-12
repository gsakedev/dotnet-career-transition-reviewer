---
name: dotnet-career-transition-reviewer
description: Review an explicitly selected mentee repository for the .NET career-transition curriculum in Claude Code. Use for full assignment, focused, change or historical progress reviews, including the automatic Learning Progress and Next Focus report. Setup checks do not authorize a mentee review.
---

# Claude Code reviewer entrypoint

This is a thin entrypoint to the shared reviewer, not a separate review policy.

1. Locate the reviewer root, three levels above this skill directory. Read its `AGENTS.md`, `targets.json` and `review-rubric.md`.
2. Read and follow the [canonical central skill](../../../.agents/skills/dotnet-career-transition-reviewer/SKILL.md) at its original location. Its `references/` links and sibling specialist paths are relative to the canonical `.agents/skills/dotnet-career-transition-reviewer/` directory, not this Claude wrapper.
3. Apply the complete canonical workflow in this conversation, including specialist instruction loading, evidence validation and the automatic learning report. Skill files are instructions; this wrapper does not spawn agents or authorize execution of learner code.
4. Preserve the shared explicit-target, audience, privacy, finding-limit and opt-in saving rules. If any required canonical file is missing, report that limitation and stop the affected work.

Use the request below and the user's explicit context to establish the review manifest. Clarify missing required target or scope details before dependent analysis; never infer a mentee from setup, recent work or the registry.

## Review request

$ARGUMENTS
