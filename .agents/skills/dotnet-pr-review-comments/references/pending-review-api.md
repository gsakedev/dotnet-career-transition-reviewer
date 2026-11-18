# Pending GitHub review transport

Read this after the parent skill's opt-in and target checks. These API notes do not grant permission to upload or submit anything.

## Choose a transport

Inspect the available connector/MCP schema. A tool requiring `COMMENT`, `APPROVE` or `REQUEST_CHANGES` submits a review and cannot satisfy pending-only approval. In particular, do not use a submit-only `add_review_to_pr` operation for draft creation.

Prefer a pending-capable connected tool. Otherwise use an already configured GitHub CLI or an authenticated REST client with an existing authorized credential provider. Use the confirmed GitHub host and account; do not mix accounts across pending-review inspection and creation. Never print credentials, place them in command arguments/payload files, or persist them in review artifacts. Do not install tools or reconfigure credentials during this workflow.

## REST operations

Let `P` be `repos/{owner}/{repo}/pulls/{pull_number}` for the explicitly confirmed PR. Supply concrete validated values; do not let a CLI infer the reviewer repository from the current directory.

| Purpose | Operation |
| --- | --- |
| Confirm PR and SHAs | `GET P` |
| Inspect changed files | `GET P/files`; obtain the full diff if a patch is missing/truncated |
| Inspect existing reviews | `GET P/reviews` |
| Create a pending review | `POST P/reviews` with `commit_id`, `body` and `comments`; omit `event` entirely |
| Verify a review | `GET P/reviews/{review_id}` |
| Verify inline comments | `GET P/reviews/{review_id}/comments` |
| Submit only after separate approval | `POST P/reviews/{review_id}/events` with an approved `event` |

Paginate collection endpoints. The comments response is an array; compare every element, not a presumed single-object wrapper. Check actual returned state and contents instead of treating an HTTP success as proof of the requested outcome. GitHub documents [pending creation and submission separately](https://docs.github.com/en/rest/pulls/reviews).

Construct the request from the approved preview using a JSON serializer:

~~~json
{
  "commit_id": "<verified-full-head-sha>",
  "body": "<approved-learner-safe-summary>",
  "comments": [
    {
      "path": "<verified-repository-relative-path>",
      "line": 42,
      "side": "RIGHT",
      "body": "<approved-learner-safe-comment>"
    }
  ]
}
~~~

The example line is illustrative, not an approved location. Prefer verified `line`/`side` and optional `start_line`/`start_side`. If a transport requires `position`, derive it from the actual diff; do not pass a file line number as a position. Keep finding IDs/obligation in the approved text or local preview mapping, not unsupported API fields.

## GitHub CLI fallback

Use only when `gh` is already available and authenticated for the selected host/account. Supply the serialized JSON over standard input with `--input -`, or use `--input <file>` if saving that payload was explicitly requested and its reviewer output path validated. Preserve exact newlines; never interpolate comment text into executable shell syntax. See [gh api input and pagination](https://cli.github.com/manual/gh_api).

~~~text
gh api --hostname <confirmed-host> --method POST repos/<owner>/<repo>/pulls/<number>/reviews --input -
~~~

That command shape expects the approved JSON on stdin; it is not a runnable placeholder example or a dry run. For reads, set `--method GET` explicitly and paginate collections. The agent may need host permission for network access; approval to post does not bypass the host's permissions.

If no suitable authenticated transport is available, retain the local preview and report the prerequisite. Never fall back to `gh pr review --comment`, direct published comments, or an automatically installed server.
