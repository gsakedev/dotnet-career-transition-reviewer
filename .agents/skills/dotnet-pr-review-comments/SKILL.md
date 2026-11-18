---
name: dotnet-pr-review-comments
description: Turn a completed .NET mentoring review into learner-safe GitHub pull-request comments. Use after the mentor accepts the reviewer's offer or explicitly requests PR comments; preview the exact feedback and require approval before creating a pending review. Publication requires a separate explicit request.
---

# Optional PR review comments

Read `AGENTS.md` and `review-rubric.md` from the reviewer root, three levels above this skill directory. Reuse the explicitly selected review and its manifest. This skill prepares and, after approval, uploads learner-safe feedback; it does not run another full review or modify learner/template files.

## Enter only after opt-in

An accepted post-review offer or direct request to prepare comments authorizes drafting and read-only PR inspection. It does not authorize uploading unseen text. If the user already approved the exact payload and destination below, use that approval without asking again.

Require the source review or current validated findings, the explicit mentee/session/scope, and the intended GitHub PR URL or host/owner/repository/number. Ask only for missing information. A local repository or branch alone does not select a PR. Do not discover a different mentee or widen a focused review.

## Prepare a concrete preview

1. Verify the PR identity, open state, base/head repositories and full SHAs, actual changed-file diff, and the authenticated account that would own the review. The reviewed committed target must match the PR head. Historical, dirty-tree or mismatched evidence requires revalidation against an explicitly agreed current target before proceeding; never silently retarget.
2. Inspect existing reviews and inline comments, including all pages and the chosen account's pending drafts, to avoid duplicates. Use the same authenticated identity for pending-review reads and writes. If an identical pending review already exists, verify it against the approved payload and return its link without creating another. Do not overwrite, delete, append to or publish an unrelated draft; report the conflict and stop this operation. Managing that unrelated draft is a separate request.
3. Select only validated findings within the requested scope. Preserve finding IDs and Must-fix/Optional distinctions, the normal five-finding cap and required overflow disclosure. Do not convert learning priorities or speculative hypotheses into extra defects. If nothing warrants a PR comment, say so and create no empty review.
4. Write comments independently for the learner from validated evidence. Explain the observed behavior, consequence and learner-owned next step without supplying assignment solutions. Never upload a private report, personnel comparisons, internal hypotheses, mentor-only paths, secrets or private discussion. A mentee-only source does not authorize reading a private assessment.
5. Anchor inline comments to verified PR diff paths, sides and lines. A file line number is not a diff position. Put a valid finding outside the available diff into the review summary with an appropriate committed-code reference, or omit it with an explanation to the mentor; never invent an inline location.
6. Show the exact proposed summary and every comment, including its finding ID, obligation and location. Clearly separate this upload payload from any mentor-only explanation. Identify the destination PR, account, head/base SHAs, comment count and intended `PENDING` state. Ask approval to create this concrete pending review, explaining that this writes to GitHub but does not submit it.

Keep the preview in chat unless saving is requested. If saving, use a new validated reviewer review directory under the existing audience/path rules; keep approval and operation status mentor-only. Do not save in learner/template repositories. A saved draft is not authorization to upload.

## Create only the approved pending review

Read [pending-review-api.md](references/pending-review-api.md) when choosing or using the GitHub transport. Prefer an available connector/MCP tool only if it supports an unsubmitted review. Use an already configured authenticated GitHub CLI or REST client as a fallback. Do not install tools, add servers or change authentication automatically.

Immediately before a write, recheck the PR, account, head/base SHAs, diff locations and existing pending reviews. Bind approval to that destination, account, snapshot and exact payload. Changes to any of them invalidate the affected approval: refresh the preview and obtain approval for the changed result. Do not treat a generic yes to the initial offer as payload approval.

Create one review containing the approved summary and comments, with the explicit head commit and no submission event. Never substitute an immediate inline comment, issue comment, `COMMENT`/`APPROVE`/`REQUEST_CHANGES` submission or `gh pr review` for pending creation.

Read the created review and all its comments back. Verify its account, commit, `PENDING` state, exact summary, comment count, bodies and locations against the approved payload; then recheck PR drift. Compare raw Markdown and resolved diff locations; only CRLF/LF normalization is acceptable, not trimming or rewriting text. Report the returned review ID/link and verified state, and stop. Pending creation never triggers publication.

If creation or verification times out or gives an ambiguous result, query remote reviews/comments before retrying. Reuse a verified identical pending review; if the result is partial, mismatched or unobservable, report the uncertainty and stop writes. Never blindly retry or remove a draft to recover. If the PR moved during creation, leave the draft unsubmitted and flag it as stale.

## Publication is a separate request

The mentor can submit the pending review manually on GitHub. If explicitly asked to publish it here, resolve the specific review ID and re-read the pending contents, author and current PR snapshot. Publish only the payload the mentor approved. Changed text/comments or snapshot require a fresh preview and approval; already-submitted reviews must not be submitted again.

Use a `COMMENT` submission unless the mentor explicitly chooses another review verdict. Approval to create a draft does not authorize any submission event. After submission, verify the resulting state, contents and submission timestamp. Do not change the PR, branch, files, other reviews or conversation threads.

## Delegation and unavailable tools

The current reviewer can perform this skill directly. If delegation is useful and authorized, a child may prepare learner-safe wording from the scoped evidence; the coordinating reviewer retains approval checks and all GitHub writes. Delegation does not transfer or expand approval.

If authenticated pending-review creation or read-back verification is unavailable, keep the preview in chat and explain the missing capability. Do not claim a GitHub draft exists or switch to publishing. Setup alone never authorizes a real PR operation.
