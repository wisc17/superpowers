# Superpowers Extended for Claude Code

> ## ⚠️ This Is a Personal Fork (wisc17 changes)
>
> This copy adapts the default skills to fit my own workflow and preferences.
> The goal is to keep the agent helpful but unobtrusive — fewer unnecessary prompts, fewer questions on how to proceed, more control left in my hands.
>
> Everything else works exactly like upstream Superpowers Extended.

---

This fork is based on [pcvelz/superpowers](https://github.com/pcvelz/superpowers) — the "Superpowers Extended for Claude Code" plugin — itself a fork of [obra/superpowers](https://github.com/obra/superpowers).

Upstream documentation (installation, the workflow guide, the skills library, the configuration reference) lives in those repositories and is not reproduced here. This README records only what this fork changes, one entry per change, in the order they were made.

## Changes

### Fork packaging

Updated the marketplace and plugin manifest for this fork.

### Design docs aren't committed to git

Brainstorming writes the spec as an untracked working file instead of committing it.

### My selected model is always respected

Subagents never get silently downgraded to a cheaper/faster model to save cost.

### Parked: using-superpowers

Skills serving flows this fork doesn't run are **parked**: `SKILL.md` is renamed to `SKILL.txt`, which drops the skill out of discovery so it no longer costs a line in every context window. Nothing is deleted; renaming the file back re-enables it. This fork runs one execution path — `writing-plans` → `subagent-driven-development` — and leaves git to you.

using-superpowers itself stays fully active: the SessionStart hook injects its complete text every session. Parking only stops it being *double*-registered as an invocable skill.

Deliberately **not** parked: **requesting-code-review**. Subagent-driven-development dispatches the final whole-branch review straight from its `code-reviewer.md` template by relative path, so the skill and that file both stay live.

### Parked: dispatching-parallel-agents

Nothing routed to it — subagent-driven-development has its own Bounded Parallel Dispatch section.
