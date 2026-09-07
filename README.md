# Can Bilmez

Backend and platform engineer focused on **reliable developer tooling, CI/CD, testing systems, and execution correctness**.

I build and operate backend/full-stack systems in **Go, TypeScript, Python, FastAPI, PostgreSQL, and GitHub Actions**, and I contribute fixes upstream when I can reproduce a problem cleanly and prove the behavior with tests.

## Featured open-source projects

### [gh-runner-eol](https://github.com/canblmz1/gh-runner-eol)

A Go CLI and GitHub Action for finding self-hosted GitHub Actions runner versions that are pinned in source or approaching end-of-life.

- source scanning for Dockerfiles, ARC/Helm values, Terraform/Packer, Ansible, and Chef
- live runner + official GitHub deprecation data
- table, JSON, and SARIF output
- tagged releases, CI, tests, security/contribution docs, and merged external contributions

### [RuleProbe](https://github.com/canblmz1/ruleProb)

Executable compliance testing for AI coding instructions such as `CLAUDE.md`, `AGENTS.md`, and repository rules. Produces sandboxed scenarios, scored reports, CI output, and SARIF.

### [Tautest](https://github.com/canblmz1/tautest)

PR-scoped mutation testing for JavaScript and TypeScript. Uses StrykerJS to focus mutation testing on changed lines and turn surviving mutants into review-ready signals.

### [prefix-safe-json](https://github.com/canblmz1/prefix-safe-json)

A small TypeScript library for streamed LLM tool-call correctness. It keeps incomplete or unconfirmed streamed arguments from being treated as executable input and exposes provider/AI-SDK adapters plus validation hooks.

Current package: **`prefix-safe-json@0.5.0`**.

### [runkeep](https://github.com/canblmz1/runkeep)

A focused utility for preserving GitHub Actions run history before retention removes useful CI evidence.

---

## Open-source impact

| Project | Work | Outcome |
|---|---|---|
| **Atomic Agent** | Closed malformed and ambiguously terminated native tool-call execution paths | **Merged upstream** in [AtomicBot-ai/atomic-agent#144](https://github.com/AtomicBot-ai/atomic-agent/pull/144) |
| **Tugtainer** | Implemented configurable container update / rollback lifecycle hooks across backend, executor, persistence, UI, docs, and tests | **Merged upstream** in [Quenary/tugtainer#217](https://github.com/Quenary/tugtainer/pull/217) |
| **Vercel AI SDK** | Reported unsafe automatic tool execution after unsafe terminal states | Fixes/backports merged; co-author credit on the resulting fix, including [vercel/ai#19066](https://github.com/vercel/ai/pull/19066) |
| **Roomote** | Responsibly reported an unused environment endpoint that could expose raw environment configuration | Publicly acknowledged in [Roomote v0.39.1](https://github.com/RooCodeInc/Roomote/releases/tag/v0.39.1) |

[**Verified contribution ledger →**](CONTRIBUTIONS.md)

---

## Production work

I have shipped private/client systems across dealership operations, reporting, and web backends. Those repositories remain private; I do not use client source code as portfolio material.

The public portfolio is intentionally weighted toward work that can be inspected end-to-end: source, tests, CI, release history, and upstream review evidence.

---

## Current upstream work

- **Node.js** — [nodejs/node#64954](https://github.com/nodejs/node/pull/64954): recursive `readdir` Buffer encoding behavior with regression coverage.
- **Apache Maka** — [apache/maka#3434](https://github.com/apache/maka/pull/3434): tool execution gated on raw stream completion evidence.
- **Trendyol Baklava** — [Trendyol/baklava#1220](https://github.com/Trendyol/baklava/pull/1220): resize-listener cleanup with regression coverage.
- **Continue** — [continuedev/continue#13224](https://github.com/continuedev/continue/pull/13224): streamed tool-call handling work.
- **Vercel AI SDK docs** — [vercel/ai#18770](https://github.com/vercel/ai/pull/18770): distinguish truncation from malformed JSON before repair in the cookbook flow.

Open PRs are listed as work in progress, not as merged contribution claims.

---

## Engineering style

- Reproduce against current upstream before proposing a fix.
- Prefer regression tests that fail on the old behavior.
- Test the real side-effect or execution boundary when it matters.
- Compare patch failures against a clean baseline before calling them regressions.
- Keep reliability and security claims scoped to what the evidence proves.
- Prefer small, maintainable fixes over broad abstractions without user pull.

## Current focus

**Backend systems · platform tooling · CI/CD reliability · testing systems · OSS maintenance · LLM tool-call correctness**
