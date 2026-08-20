<div align="center">

# Can Bilmez

### Backend Systems · Developer Tooling · Execution Safety

**Reliability · Testing · Open Source · Fail-Closed Design**

I work on systems where **“valid-looking” is not the same as “safe to execute.”**

<p>
  <img src="https://img.shields.io/badge/TypeScript-111827?style=flat-square&logo=typescript&logoColor=white" alt="TypeScript" />
  <img src="https://img.shields.io/badge/Node.js-111827?style=flat-square&logo=nodedotjs&logoColor=white" alt="Node.js" />
  <img src="https://img.shields.io/badge/Python-111827?style=flat-square&logo=python&logoColor=white" alt="Python" />
  <img src="https://img.shields.io/badge/FastAPI-111827?style=flat-square&logo=fastapi&logoColor=white" alt="FastAPI" />
  <img src="https://img.shields.io/badge/PostgreSQL-111827?style=flat-square&logo=postgresql&logoColor=white" alt="PostgreSQL" />
  <img src="https://img.shields.io/badge/GitHub_Actions-111827?style=flat-square&logo=githubactions&logoColor=white" alt="GitHub Actions" />
</p>

</div>

---

## Open-source impact

| Project | Work | Outcome |
|---|---|---|
| **Vercel AI SDK** | Reported unsafe automatic tool execution after `length`, `error`, `content-filter`, and `other` terminal states | Reproduced across **v5, v6, v7**; fixes/backports merged on all three lines; co-author credit on the resulting fix commits; v7 shipped in **`ai@7.0.70`** |
| **Tugtainer** | Implemented configurable container update / rollback lifecycle hooks across backend, executor, persistence, UI, docs, and tests | **Merged** upstream in [PR #217](https://github.com/Quenary/tugtainer/pull/217) |
| **Roo Code / Roomote** | Responsibly reported an environment-configuration exposure issue | Publicly acknowledged in the **v0.39.1** release notes |

### Vercel AI SDK — report → reproduce → fix → backport → release

[`#19063 report`](https://github.com/vercel/ai/issues/19063)
→ [`#19066 v7`](https://github.com/vercel/ai/pull/19066)
→ [`#19120 v6`](https://github.com/vercel/ai/pull/19120)
→ [`#19121 v5`](https://github.com/vercel/ai/pull/19121)
→ [`ai@7.0.70`](https://github.com/vercel/ai/releases/tag/ai%407.0.70)

> A tool call should not execute just because its arguments parse. The terminal model state is part of the execution contract.

[**Verified contribution ledger →**](CONTRIBUTIONS.md)

---

## Shipped

### 🛡️ [prefix-safe-json](https://github.com/canblmz1/prefix-safe-json)

**Fail-closed execution integrity for streamed LLM tool calls.**

`0.0.1-alpha.4` is published on npm with a high-level AI SDK execution guard, provider terminal-state handling, schema validation, execution evidence, and explicit `execute / retry / reject` decisions.

```bash
npm install prefix-safe-json@next
```

**Release validation:** 802 tests · 95.62% branch coverage · 88.18% mutation score · 11.1M+ fuzz property checks with zero invariant violations.

`AI SDK v5/v6/v7` · `streaming JSON` · `tool calling` · `schema validation` · `fail closed`

---

## Featured projects

### 🧪 [RuleProbe](https://github.com/canblmz1/ruleProb)

**Executable compliance testing for AI coding instructions.** Turns `CLAUDE.md`, `AGENTS.md`, Cursor rules, and similar repository instructions into sandboxed scenarios with scored reports, CI integration, SARIF output, provider comparison, and regression-oriented evidence.

`AI agents` · `developer tooling` · `sandbox testing` · `CI` · `SARIF`

### ⚡ [Tautest](https://github.com/canblmz1/tautest)

**PR-scoped mutation testing for JavaScript / TypeScript.** Uses StrykerJS to focus mutation testing on changed lines and turn surviving mutants into actionable review signals instead of broad, expensive mutation runs.

`mutation testing` · `StrykerJS` · `TypeScript` · `GitHub Actions` · `CI quality gates`

---

## Current upstream work

- **Node.js** — [`nodejs/node#64954`](https://github.com/nodejs/node/pull/64954): recursive `readdir` with Buffer encoding across callback, sync, promises, and `withFileTypes` paths.
- **Atomic Agent** — [`AtomicBot-ai/atomic-agent#144`](https://github.com/AtomicBot-ai/atomic-agent/pull/144): fail closed on malformed or ambiguously terminated native tool calls, with execution-level regression coverage.
- **Trendyol Baklava** — [`Trendyol/baklava#1220`](https://github.com/Trendyol/baklava/pull/1220): fix a resize-listener reference leak in `bl-pagination` and prove cleanup with a regression test.
- **Vercel AI SDK docs** — [`vercel/ai#18770`](https://github.com/vercel/ai/pull/18770): distinguish truncation from malformed JSON before `jsonrepair` in the cookbook flow.

---

## Engineering style

- Reproduce against current upstream before proposing a fix.
- Prefer regression tests that fail on the old behavior.
- Test at the real execution boundary when side effects are involved.
- Compare patch failures against a clean baseline before calling them regressions.
- Keep native fixes small when a dependency would add more complexity than value.
- Keep security/reliability claims scoped to what the evidence proves.
- Fail closed where ambiguous state can trigger an irreversible action.

---

## Current focus

**Backend systems · AI agent/tool execution safety · developer infrastructure · testing systems · OSS reliability**

```text
looks valid  ───────────────►  safe to execute
              not the same thing
```
