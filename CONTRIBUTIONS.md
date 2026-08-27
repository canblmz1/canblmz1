# Open-source contributions

A concise ledger of public upstream work, separated by **merged**, **acknowledged**, **open**, and **closed-unmerged** status so nothing is overstated.

_Last verified: 2026-08-27._

## Merged upstream contributions

### Atomic Agent — native tool-call execution integrity

- Merged PR: [AtomicBot-ai/atomic-agent#144](https://github.com/AtomicBot-ai/atomic-agent/pull/144)
- Merge commit: [`dcf77f180b0f66b8f2d5858afa212638237ac619`](https://github.com/AtomicBot-ai/atomic-agent/commit/dcf77f180b0f66b8f2d5858afa212638237ac619)

The patch closes an execution-integrity failure mode in the native OpenAI-compatible tool path where malformed or ambiguously terminated `function.arguments` could still reach dispatch. Non-empty malformed arguments now fail instead of silently becoming `{}`, pending tool calls require a real terminal signal, Qwen-tagged calls share the same termination-safety decision as native calls, and an undelimited final SSE event is flushed and parsed at EOF.

The upstream maintainer independently traced the implementation, found two blocking cross-path issues plus two smaller issues, and re-probed the corrected branch before merge. The final maintainer validation covered Qwen tagged calls, native bare EOF, terminal events and `[DONE]` without trailing blank lines, malformed final events, additional tool-argument deltas without a terminal signal, SSE comments, split multi-byte UTF-8, empty/[DONE]-only responses, and abort during a pending call. The patch was then merged onto current `main`.

### Vercel AI SDK — unsafe finish-reason tool execution

- Reported: [vercel/ai#19063](https://github.com/vercel/ai/issues/19063)
- AI SDK 7 fix: [vercel/ai#19066](https://github.com/vercel/ai/pull/19066)
- AI SDK 6 backport: [vercel/ai#19120](https://github.com/vercel/ai/pull/19120)
- AI SDK 5 backport: [vercel/ai#19121](https://github.com/vercel/ai/pull/19121)
- v7 release: [`ai@7.0.70`](https://github.com/vercel/ai/releases/tag/ai%407.0.70)
- Credit: `Co-authored-by: canblmz1` on the merged v7, v6, and v5 fix commits

The report showed that valid side-effecting tool calls could still auto-execute when the enclosing model call ended with `length`, `error`, `content-filter`, or `other`. Vercel classified the report as a high-confidence bug and independently reproduced the defect on AI SDK 7, then on the v6 and v5 release lines.

The resulting fixes use an explicit safe-finish-reason allowlist: automatic tool execution is permitted only after `stop` or `tool-calls`. On streaming paths, execution is deferred until the terminal finish reason is known rather than beginning before that state arrives.

The v7 fix is publicly shipped in `ai@7.0.70`. The v5 and v6 backport PRs are merged; this ledger intentionally does not name specific published v5/v6 patch versions without independent verification.

### Tugtainer — container update lifecycle hooks

- Merged PR: [Quenary/tugtainer#217](https://github.com/Quenary/tugtainer/pull/217)

Implemented configurable container lifecycle hooks across agent, backend, persistence, executor, frontend, documentation, and tests. The design uses two opt-in safety gates (`ALLOW_HOOKS` and agent-side `ALLOW_EXEC`) and defines distinct blocking/report-only semantics for update and rollback hooks.

## Publicly acknowledged security / reliability reports

### Roo Code / Roomote — environment configuration exposure

- Release acknowledgment: [Roomote v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1)

Roomote's v0.39.1 changelog publicly credits `@canblmz1` for reporting an unused environment endpoint that could expose raw environment configuration. This is a public responsible-reporting acknowledgment, not a CVE or bounty claim.

### Cline — truncated tool-call repair / execution integrity

- Report: [cline/cline#13001](https://github.com/cline/cline/issues/13001)
- Follow-up integration attempt: [cline/cline#13304](https://github.com/cline/cline/pull/13304) — **closed, not merged**

The report showed that truncated tool-call arguments could be silently repaired into valid-looking JSON before execution. Maintainers independently reproduced the residual container-level truncation case and confirmed that stream-level termination information is needed to distinguish syntactically repairable input from genuinely complete input.

PR #13304 validated one concrete integration approach using `prefix-safe-json` as a raw-stream execution-integrity side channel and included execution-level dispatch tests, but it was closed without merge. It is therefore evidence of a tested integration attempt, **not** an upstream adoption claim.

## Current upstream work

These entries are intentionally listed as **open**, not as merged contributions.

### Node.js

- [nodejs/node#64954 — `fs: fix recursive readdir with buffer encoding`](https://github.com/nodejs/node/pull/64954)
- Status: **open**
- Scope: recursive `readdir` Buffer encoding support across callback, sync, promises, and `withFileTypes` paths, with regression coverage.

### Trendyol Baklava

- [Trendyol/baklava#1220 — `fix(pagination): clean up resize listener on disconnect`](https://github.com/Trendyol/baklava/pull/1220)
- Status: **open**
- Scope: fixes a resize-listener reference leak in `bl-pagination` and adds a regression test that verifies the exact registered handler is removed.

### Vercel AI SDK documentation

- [vercel/ai#18770 — `docs: gate jsonrepair on finishReason in the truncation cookbook`](https://github.com/vercel/ai/pull/18770)
- Status: **open**
- Scope: documents truncation as a different failure mode from malformed JSON and checks `finishReason` before attempting `jsonrepair` in the cookbook example.

### Sandbase Harness — `prefix-safe-json` integration pilot

- [sandbaseai/sandbase-harness#73 — `fix: gate confirmed tools on raw stream completion`](https://github.com/sandbaseai/sandbase-harness/pull/73)
- Status: **open**
- Dependency: exact `prefix-safe-json@0.4.2`
- Scope: confirmation-required tool calls are persisted for later execution only when their real streamed argument lifecycle is complete, schema-valid, identity-consistent, and safely terminated.
- Claim boundary: this is an **open integration pilot**, not an upstream adoption claim unless merged.

## How I approach upstream work

- Reproduce against current upstream before proposing a fix.
- Prefer execution-level or behavior-level tests over event-only assertions.
- Compare failures against a clean baseline before calling them regressions.
- Keep native fixes small when a new dependency would not materially reduce complexity.
- Separate confirmed behavior from inference and avoid inflating security impact.
- Treat side-effect boundaries as integrity problems: valid-looking data is not automatically safe-to-execute data.
