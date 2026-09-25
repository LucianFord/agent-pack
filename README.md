# Agent Pack — Free Sample

Production-tested AI coding agent configurations for **Claude Code** and **Kimi Code**.
Not another prompt collection — every config here ships with its known failure modes,
dispatch conditions, and acceptance criteria, battle-shaped on real codebases.

## What's in this free pack

| Agent | What it does |
|---|---|
| [`surgeon-reviewer`](agents/surgeon-reviewer.md) | Code review that reports only real problems — bugs, money-losing logic, maintenance traps — ranked by blast radius. Zero style opinions. |
| [`test-archaeologist`](agents/test-archaeologist.md) | Writes characterization tests for legacy code by excavating its *actual* behavior contract first. The safety net you need before any refactor. |
| [`migration-pilot`](agents/migration-pilot.md) | Runs large-version migrations as small, individually revertible stages with before/after behavior snapshots. |

## Install

**Claude Code:**
```bash
mkdir -p .claude/agents
cp agents/*.md .claude/agents/
```

**Kimi Code:**
```bash
mkdir -p .kimi/agents
cp agents/*.md .kimi/agents/
```

Then just ask, e.g. *"use surgeon-reviewer on this PR"* — or let the orchestrator
auto-dispatch based on each file's `description` field.

## Why these are different

Free agent configs are everywhere. Three things make these worth your disk space:

1. **Failure modes included.** Each config documents how it goes wrong (alarm inflation,
   diff blindness, snapshot theater...) and how the config itself guards against it.
2. **Dispatch conditions in the frontmatter.** The `description` tells the orchestrator
   *when* to use the agent — that's what makes multi-agent orchestration actually work.
3. **Acceptance criteria built in.** Each agent knows how to check its own output,
   so you get verdicts (`SHIP` / `DO NOT SHIP`), not essays.

See [EXAMPLE.md](EXAMPLE.md) for a sample surgeon-reviewer report — and [BENCHMARK.md](BENCHMARK.md) for the full OWASP NodeGoat run (official vulnerability list: zero misses).

## Full pack

This is 3 of 12 agents + 3 multi-agent orchestration workflows
(PR pipeline, legacy-system rescue, release-day pipeline).

**Full pack: $19** — launching soon on Gumroad. Watch this repo to get notified.

## License

MIT — use them, fork them, tell us what broke.
