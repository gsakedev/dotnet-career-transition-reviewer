@AGENTS.md

# Claude Code adapter

The imported `AGENTS.md` is the reviewer contract. Keep review policy in that file, `review-rubric.md`, `targets.json` and the canonical `.agents/skills/` packages. This adapter only supplies Claude Code entrypoints.

- Start Claude Code in this reviewer repository. For a review request, read and follow `.agents/skills/dotnet-career-transition-reviewer/SKILL.md`, whether the user invokes `/dotnet-career-transition-reviewer` or requests a review in ordinary language.
- Map the canonical Codex startup instructions to this host: `CLAUDE.md` imports the contract, and `.claude/skills/dotnet-career-transition-reviewer/SKILL.md` exposes the slash command. Use `/dotnet-career-transition-reviewer` in Claude Code in place of Codex's `$dotnet-career-transition-reviewer`.
- Read canonical specialist skills and templates from their original `.agents/skills/` locations. Resolve each file's relative references against that file's directory. Do not copy the policies into Claude-specific files or assume every specialist is a Claude slash command.
- Follow the central skill's automatic learning-coach step and its audience, evidence, scope and saving rules. Loading this adapter or checking setup does not authorize a mentee review.
- Discover the available Microsoft Learn MCP tools in this host. The project `.mcp.json` declares the documentation endpoint; configuration alone does not verify connectivity. Follow the shared contract's generic-query and fallback rules.
- If the import or a required canonical file is unavailable, disclose the missing path and stop the affected work instead of inventing a replacement policy.

These instructions do not grant filesystem permissions. Keep the learner-ownership and read-only review boundaries in the imported contract.
