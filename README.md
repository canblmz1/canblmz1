# Can Bilmez

Backend & developer-tooling engineer focused on **reliability, execution integrity, testing, and production-grade automation**.

I like problems where the happy path is easy but the failure boundary is not: streamed LLM tool calls, side-effecting agent runtimes, mutation testing, CI/CD reliability, auth/session hardening, and data consistency.

**TypeScript · Node.js · Python · FastAPI · PostgreSQL · GitHub Actions · AI/LLM tooling**

## Selected open-source work

- **Vercel AI SDK** — reported an execution-integrity bug where valid tool calls could still auto-execute after unsafe model termination states. The issue was reproduced upstream and fixed; I was credited as a co-author on the merged fix.  
  [Issue #19063](https://github.com/vercel/ai/issues/19063) · [Merged fix #19066](https://github.com/vercel/ai/pull/19066) · [Fix commit](https://github.com/vercel/ai/commit/a8285273ef8b4b2c36cf3cb692706da7f40077d4)

- **Tugtainer** — contributed the merged container update lifecycle hooks feature, covering guarded container exec, backend policy gates, rollback/update hooks, frontend controls, migration, and tests.  
  [PR #217](https://github.com/Quenary/tugtainer/pull/217)

- **Roo Code / Roomote** — responsibly reported an environment-configuration exposure issue that was publicly acknowledged in the v0.39.1 release notes.  
  [v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1)

[See the verified open-source contribution ledger →](CONTRIBUTIONS.md)

## Featured projects

### [RuleProbe](https://github.com/canblmz1/ruleProb)
**Executable compliance tests for AI coding instructions.** Turns `CLAUDE.md`, `AGENTS.md`, Cursor rules, and similar instruction files into sandboxed test scenarios with scored reports, CI integration, SARIF output, and provider comparisons.

### [Tautest](https://github.com/canblmz1/tautest)
**PR-scoped mutation testing for JavaScript/TypeScript.** Uses StrykerJS to focus mutation testing on changed lines and turn surviving mutants into review-ready signals instead of broad, expensive mutation runs.

### [prefix-safe-json](https://github.com/canblmz1/prefix-safe-json)
**Fail-closed execution integrity for streamed LLM tool calls.** Distinguishes complete executable arguments from truncated or unconfirmed state before side effects occur, with provider adapters, schema validation, concurrent call coordination, and explicit execute/retry/reject decisions.

## What I optimize for

- Reproducible bug reports instead of speculative claims
- Regression tests that fail on the old behavior
- Small native fixes when a dependency would be unnecessary
- Explicit failure semantics around side effects
- Baseline-vs-patch validation for large test suites
- Security and reliability controls that fail closed where practical

## Current direction

I am especially interested in **backend systems, AI agent/tool execution safety, developer infrastructure, testing systems, and open-source reliability work**.

If a bug sits between “the data looks valid” and “the system is actually safe to execute it,” that is usually the kind of problem I want to investigate.
