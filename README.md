<div align="center">

# Can Bilmez

<img src="https://readme-typing-svg.demolab.com?font=Fira+Code&weight=600&size=22&duration=2800&pause=900&color=58A6FF&center=true&vCenter=true&repeat=true&width=820&height=45&lines=Backend+systems+%C2%B7+Developer+tooling;Fail-closed+LLM+tool+execution;Reproduce+%E2%86%92+red-team+%E2%86%92+ship+upstream" alt="Backend systems, developer tooling, and fail-closed execution safety" />

**Reliability · Testing · Open Source · Execution Integrity**

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
| **Atomic Agent** | Closed malformed / ambiguously terminated native tool-call execution paths across native OpenAI-compatible and Qwen-tagged flows | **Merged upstream** in [PR #144](https://github.com/AtomicBot-ai/atomic-agent/pull/144) as [`dcf77f1`](https://github.com/AtomicBot-ai/atomic-agent/commit/dcf77f180b0f66b8f2d5858afa212638237ac619) after maintainer re-probes |
| **Vercel AI SDK** | Reported unsafe automatic tool execution after `length`, `error`, `content-filter`, and `other` terminal states | Reproduced across **v5, v6, v7**; fixes/backports merged on all three lines; co-author credit on the resulting fix commits; v7 shipped in **`ai@7.0.70`** |
| **Tugtainer** | Implemented configurable container update / rollback lifecycle hooks across backend, executor, persistence, UI, docs, and tests | **Merged** upstream in [PR #217](https://github.com/Quenary/tugtainer/pull/217) |
| **Roo Code / Roomote** | Responsibly reported an environment-configuration exposure issue | Publicly acknowledged in the **v0.39.1** release notes |

### Atomic Agent — fail closed at the real dispatch boundary

[`AtomicBot-ai/atomic-agent#144`](https://github.com/AtomicBot-ai/atomic-agent/pull/144) prevents truncated or malformed native tool-call arguments from silently becoming executable input.

The final patch covers:

- malformed non-empty `function.arguments` → parse failure, never silent `{}` fallback;
- bare EOF with pending tool calls → fail closed unless a real terminal signal was observed;
- Qwen tagged calls → same termination-safety decision as native calls;
- final SSE events without a trailing blank line → flushed and parsed correctly at EOF;
- parallel tool calls and stream-read failures → zero dispatch;
- zero-argument calls and clean provider termination → preserved.

The maintainer re-ran the original probes plus additional EOF/UTF-8/abort cases before merging the patch to `main`.

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

`0.4.2` is published on npm with provider terminal-state handling, incremental argument evidence, schema validation, identity correlation, one-shot execution decisions, and AI SDK execution guards.

```bash
npm install prefix-safe-json@0.4.2
```

The release path is independently auditable: the published npm tarball is reproducible byte-for-byte from the tagged source, SLSA provenance is verified against the exact package/version bundle npm authenticated, and the verifier fails closed when release identity cannot be established.

`AI SDK v5/v6/v7` · `streaming JSON` · `tool calling` · `schema validation` · `reproducible release` · `fail closed`

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
- **Trendyol Baklava** — [`Trendyol/baklava#1220`](https://github.com/Trendyol/baklava/pull/1220): fix a resize-listener reference leak in `bl-pagination` and prove cleanup with a regression test.
- **Vercel AI SDK docs** — [`vercel/ai#18770`](https://github.com/vercel/ai/pull/18770): distinguish truncation from malformed JSON before `jsonrepair` in the cookbook flow.
- **Sandbase Harness** — [`sandbaseai/sandbase-harness#73`](https://github.com/sandbaseai/sandbase-harness/pull/73): open `prefix-safe-json@0.4.2` integration pilot for confirmation-required tool execution; **not an adoption claim unless merged**.

---

## Engineering style

- Reproduce against current upstream before proposing a fix.
- Prefer regression tests that fail on the old behavior.
- Test the real execution boundary when side effects are involved.
- Compare patch failures against a clean baseline before calling them regressions.
- Keep security and reliability claims scoped to what the evidence proves.
- Fail closed where ambiguous state can trigger an irreversible action.

---

## Current focus

**Backend systems · AI agent/tool execution safety · developer infrastructure · testing systems · OSS reliability**

```text
looks valid  ───────────────►  safe to execute
              not the same thing
```
