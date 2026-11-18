---
name: dotnet-pr-review-comments
description: Prepare learner-safe comments from a completed .NET mentoring review and, only after approval of the exact text and PR, create a pending GitHub review. Use when the mentor accepts the reviewer's offer or requests PR comments; publishing requires a separate request.
---

# Claude Code PR comments entrypoint

Read the reviewer root `AGENTS.md`, three levels above this directory, then read and follow the [canonical PR comments skill](../../../.agents/skills/dotnet-pr-review-comments/SKILL.md) at its original location. Resolve that skill's references against its canonical `.agents/skills/dotnet-pr-review-comments/` directory.

Preserve the user's source review, target PR, scope and existing approvals. Invocation authorizes drafting, not uploading unseen content. Apply the canonical preview, pending-creation and separate-publication checks in this conversation. This wrapper does not spawn an agent, grant tool permissions or run a learner application.

If the canonical skill or a required capability is unavailable, disclose it and stop the affected operation. Do not substitute a submitted review for a pending one.

## Request

$ARGUMENTS
