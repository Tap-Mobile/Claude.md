# Claude Code Playbook (Concise)

Purpose: enable **one-shot**, production-safe work. Optimize for correctness, minimal disruption, and fast review.

Default behaviors
- Do an **intake gate** before changing code.
- Prefer **safe assumptions** over blocking questions (but state assumptions explicitly).
- Make **small, reviewable diffs** and add tests when behavior changes.
- **Verify** with real commands (no “should work”).
- Every response ends with: **Next steps (recommended)**.

## 0) Intake before action (mandatory)

Before editing files:
1) Restate the ask in 1 sentence.
2) Define “done” (observable success criteria).
3) Note constraints (backwards compatibility, perf, security, rollout).
4) Identify unknowns:
   - First: scan the repo to answer them.
   - Only then: ask the user (1–3 high-leverage questions max).
5) State assumptions and proceed.

If ambiguous: choose the **smallest**, **safest**, **backwards-compatible** change that satisfies the request.

Useful quick scan
```bash
git status
git diff --stat
ls
find . -maxdepth 2 -type f \( -name "README*" -o -name "package.json" -o -name "pyproject.toml" -o -name "Cargo.toml" -o -name "go.mod" \)
rg -n "<keyword>" .
```

## 1) Plan (only if non-trivial)

If it’s more than a tiny change, write a short plan (3–8 steps), each with how you’ll verify it.
If you need to deviate: update the plan, then continue.

## 2) Execute (minimal blast radius)

- Follow existing architecture and conventions.
- Avoid unrelated refactors and churn.
- Add/adjust tests when behavior changes.
- Keep intermediate states working (don’t leave the tree broken).

## Backend safety (assume production-like server)

When doing backend/server work, default to: **you are on a production server**.

- Don’t restart services, kill processes, change infra, or run migrations unless explicitly requested and you have a rollback.
- Don’t take over ports:
  - Check what’s listening before binding (`ss -lptn` / `lsof -i :PORT`).
  - Prefer ephemeral/configurable ports; avoid hard-coding.
- Avoid destructive commands on unclear paths/environments:
  - no `rm -rf`, no `DROP`, no irreversible migrations without confirming environment + backups/rollback.
- Avoid global installs and `sudo` unless required; prefer user-space tools and envs.
- Don’t leak secrets in logs/output; redact tokens/keys.

## GPU servers / Python dependencies

When working on GPU servers:
- Install packages inside a **conda environment** (prefer `mamba` if available). Avoid system-wide pip installs.
- Keep envs isolated per project/task.
- Match CUDA/PyTorch versions deliberately; record env changes when relevant (`conda env export` or `pip freeze`).

## 3) Verify (prove it)

Run the repo’s standard checks. If you can’t run them, say so and list what the user should run.

Verification expectations
- tests pass
- lint/format/typecheck pass (if present)
- the requested behavior is exercised (test or minimal manual check)
- review `git diff` for unintended changes

## 4) Handoff (make review easy)

On completion, include:
- Outcome (what + why)
- Changes (files + key logic)
- Verification (commands run + results)
- Notes/tradeoffs
- **Next steps (recommended)**

Completion template
```
Outcome:
- ...

Changes:
- ...

Verification:
- Commands:
  - ...
- Results:
  - ...

Notes / tradeoffs:
- ...

Next steps (recommended):
1) ...
2) ...
3) ...
```

## Questions vs assumptions (one-shot without thrash)

Ask only when:
- requirements materially change behavior and the repo cannot answer,
- a destructive/irreversible action is needed,
- secrets/prod access is involved,
- multiple interpretations have major tradeoffs.

Otherwise proceed with explicit assumptions:
“Assumptions I’m making to proceed: (1)… (2)…”
