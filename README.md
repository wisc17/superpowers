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

### Parked: receiving-code-review

No skill, hook, command, doc, or test referenced it.

### Parked: finishing-a-development-branch

Execution reports the branch and hands control back to you, instead of opening a merge/PR/discard menu. No merging, pushing, or PR creation on your behalf — when a plan finishes, Claude just reports what it did.

### Parked: checking-gates

Only ever reachable through the opt-in user-gate re-validation hook. With that hook unregistered it served a flow that never fires. `/gate-check` removed with it.

Also drops the background check that asked for permission and nudged me to enable an enforcement hook I don't use.

### Parked: executing-plans

The single-agent fallback flow — the agent implements each task itself, no per-task subagent, no two-stage review. This fork always routes to subagent-driven-development. The Execution Handoff still asks how to execute; "Parallel Session" now means Claude stops there so you can resume with subagent-driven-development in a session of your choosing. `/execute-plan` removed with it.

### Parked: using-git-worktrees

You manage the git layout. Execution only checks it isn't on main/master and asks before starting there.

### Wanted, blocked on platform: the architect pattern

Upstream v6.4.0 added a consult hop to the Parallel Session handoff — the executing session messages the plan-writing session (`ListAgents` + `SendMessage`) to settle design questions instead of guessing, so the architect keeps its planning context while executors work with a focused one. **We want this.** It is dropped here only because Claude Code offers cross-session messaging on macOS, Linux, and WSL 2 but [not on native Windows](https://code.claude.com/docs/en/cross-session-messaging#availability) — verified on 2.1.226, where `ListAgents` reports itself disabled for the session and its subagents alike. Nothing in the changelog or issue tracker commits to a Windows port as of 2026-08-11. If native Windows support lands (or this fork moves to WSL 2), revisit: retarget the Parallel Session branch to the consult flow and re-add the consult instructions to the implementer dispatch. Do not treat a future upstream merge that reintroduces this text as an unwanted change to be re-dropped — check the platform first.
