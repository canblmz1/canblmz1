<div align="center">

# Can Bilmez

### Backend & Developer Tooling Engineer

**Reliability · Execution Integrity · Testing · Open Source**

I build and investigate systems where **“the data looks valid” is not enough** — especially when the next step can write a file, run a command, mutate state, or trigger an external side effect.

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

| | Contribution | Outcome |
|---|---|---|
| **Vercel AI SDK** | Reported a core automatic tool-execution integrity bug affecting **AI SDK v5, v6 and v7** | Vercel reproduced it on all three release lines, merged fixes/backports for each, credited `canblmz1` as a co-author on the resulting fix commits, and shipped the v7 fix in **`ai@7.0.70`** |
| **Tugtainer** | Implemented configurable container update / rollback lifecycle hooks end-to-end | **Merged** upstream in [PR #217](https://github.com/Quenary/tugtainer/pull/217) |
| **Roo Code / Roomote** | Responsibly reported an environment-configuration exposure issue | Publicly acknowledged in the **v0.39.1** release notes |

### Vercel AI SDK — multi-version tool execution fix

The issue was not limited to one release. Vercel independently reproduced the same unsafe automatic tool execution behavior across **v5, v6 and v7**, then landed release-line fixes that gate execution on safe terminal model states.

**Report → reproduce → fix → backport → release**

[`#19063 report`](https://github.com/vercel/ai/issues/19063)
→ [`#19066 v7 fix`](https://github.com/vercel/ai/pull/19066)
→ [`#19120 v6 backport`](https://github.com/vercel/ai/pull/19120)
→ [`#19121 v5 backport`](https://github.com/vercel/ai/pull/19121)
→ [`ai@7.0.70`](https://github.com/vercel/ai/releases/tag/ai%407.0.70)

> Side-effecting tools should not execute merely because their arguments parse successfully; the terminal model state matters too.

[**View the verified contribution ledger →**](CONTRIBUTIONS.md)

---

## Featured projects

### 🛡️ [prefix-safe-json](https://github.com/canblmz1/prefix-safe-json)

**Fail-closed execution integrity for streamed LLM tool calls.**

Distinguishes complete executable arguments from truncated, malformed, filtered, provider-failed, or otherwise unconfirmed state **before side effects happen**.

`streaming JSON` · `LLM tool calls` · `provider adapters` · `schema validation` · `execute / retry / reject`

---

### 🧪 [RuleProbe](https://github.com/canblmz1/ruleProb)

**Executable compliance testing for AI coding instructions.**

Turns `CLAUDE.md`, `AGENTS.md`, Cursor rules, and similar repository instructions into sandboxed scenarios with scored reports, CI integration, SARIF output, provider comparison, and regression-oriented evidence.

`AI agents` · `developer tooling` · `sandbox testing` · `CI` · `SARIF`

---

### ⚡ [Tautest](https://github.com/canblmz1/tautest)

**PR-scoped mutation testing for JavaScript / TypeScript.**

Uses StrykerJS to focus mutation testing on changed lines and turn surviving mutants into actionable review signals instead of broad, expensive mutation runs.

`mutation testing` · `StrykerJS` · `TypeScript` · `GitHub Actions` · `CI quality gates`

---

## Engineering style

- Reproduce against current upstream before proposing a fix.
- Prefer regression tests that fail on the old behavior.
- Test at the real execution boundary when side effects are involved.
- Compare patch failures against a clean baseline before calling them regressions.
- Prefer small native fixes when a dependency would add more complexity than value.
- Keep security/reliability claims scoped to what the evidence actually proves.
- Fail closed where an ambiguous state could trigger an irreversible action.

---

## Current focus

**Backend systems · AI agent/tool execution safety · developer infrastructure · testing systems · OSS reliability**

I am especially interested in failure modes that sit between:

```text
looks valid  ───────────────►  safe to execute
              not the same thing
```

That boundary is where a lot of interesting bugs live.
