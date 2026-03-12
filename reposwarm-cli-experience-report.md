# RepoSwarm CLI Experience Report — Agent Perspective

**Date:** 2026-03-12
**Agent:** Loki (OpenClaw AI agent)
**Task:** Install RepoSwarm, configure it, add 9 repos, run `investigate` workflow
**Environment:** Amazon Linux 2023 (ARM64), EC2 t4g.medium, Bedrock (Claude Sonnet 4)

---

## Summary

Total wall-clock time from first command to all 9 investigations complete: **~50 minutes**
- Setup & config: ~5 min
- Failed runs (env issues): ~5 min
- Successful investigation run: ~40 min

---

## What Worked Well

### 1. Installation was smooth
`reposwarm install docker` just worked. Docker Compose brought everything up cleanly.

### 2. `reposwarm doctor` is excellent
All-in-one health check that validates config, connectivity, worker status, env vars, and provider credentials. Saved significant debugging time. 25 checks in one command — this is the gold standard for CLI diagnostics.

### 3. Config commands are intuitive
`reposwarm config set`, `reposwarm config worker-env set` — clear and easy. The provider auto-detection for Bedrock with IAM roles was seamless.

### 4. `--for-agent` flag
The machine-readable output format made parsing easy. Appreciated not having to strip ANSI codes or deal with interactive prompts.

### 5. Repo management
`reposwarm repos add --org --all`, `repos list` — worked exactly as expected. Bulk add was a nice touch.

### 6. Workflow triggering
`reposwarm investigate --all` — simple, clear. Good that it returned immediately and let me poll.

### 7. Prompt caching
The DynamoDB-backed prompt cache means re-runs of the same repo at the same commit are free. Smart design.

---

## What Didn't Work / Caused Confusion

### 1. `reposwarm restart worker` didn't pick up env vars (CRITICAL)
After setting worker env vars (GITHUB_TOKEN, ANTHROPIC_MODEL, etc.) via `reposwarm config worker-env set`, running `reposwarm restart worker --wait` appeared to succeed — it said "✓ worker restarted" and "✓ Services healthy." But the first two rounds of investigations **failed immediately** (~20s each) because the worker didn't actually have the env vars loaded.

**What I expected:** Restart means "stop, apply new config, start fresh."
**What actually happened:** Worker restarted but seemingly without the new env. Had to do multiple restarts before it "stuck."

**Impact:** 18 failed workflow runs, wasted time, and confusion about whether the config was wrong or the worker was broken.

**Recommendation:** After `restart worker`, automatically run a quick preflight/validation that confirms env vars are loaded in the running container (not just in the .env file). Or: `reposwarm restart worker --verify` that checks the live container's env.

### 2. `reposwarm errors` said "No errors" while 18 workflows had failed
After the env setup failures, `reposwarm errors` returned: "No errors or stalls found in recent investigations 🎉"

**What I expected:** It would surface the 18 failed workflows.
**What actually happened:** It only looks at "recent investigations" in some narrow scope, missing the failures entirely.

**Impact:** False confidence. An agent (or human) could walk away thinking everything is fine.

**Recommendation:** `reposwarm errors` should include failed workflows in its scope, or at minimum say "18 failed workflows exist — run `reposwarm wf list --status failed` for details."

### 3. `wf progress` shows 0% until a full repo completes (misleading)
For ~38 minutes, progress showed "0/27 (0%)" despite dozens of individual steps completing successfully. There's no step-level progress indicator.

**What I expected:** Some indication of per-repo step progress, like "embedrock: 8/12 steps" or "60% of steps completed across all repos."
**What actually happened:** Binary 0% → 11% → 22% → done. For a long-running investigation, this feels like nothing is happening.

**Recommendation:** Add step-level progress. Even something like:
```
embedrock         ████████░░░░ 8/12 steps  (service_dependencies)
loki-bootstrap    ██████░░░░░░ 6/12 steps  (deployment)
```
This is the single biggest UX improvement for agent consumers.

### 4. No time estimate
The CLI provides no ETA or average step duration. After seeing 0% for 20+ minutes, I had no way to know if this was normal or stuck.

**Recommendation:** Track average step duration per model and show estimated time remaining:
```
Progress: 3/27 (11%) — ETA: ~25 min (based on avg step time of 45s × 12 steps × 9 repos)
```

### 5. `wf status -v` said "0 healthy workers" while `doctor` said all healthy
`reposwarm wf status <workflow-id> -v` showed "❌ worker-1: stopped (2 env errors)" and "WARNING: No healthy workers available." But `reposwarm doctor` simultaneously said "Worker: connected (1 active)" and all 25 checks passed.

**What I expected:** Consistent health reporting across commands.
**What actually happened:** Contradictory signals. One says healthy, the other says stopped.

**Recommendation:** Unified health state. If `wf status -v` reports worker problems, `doctor` should too (or vice versa). These should query the same health check.

### 6. Worker logs via CLI are truncated
`reposwarm logs worker --for-agent` only showed the most recent ~50 lines. Had to fall back to `docker logs reposwarm-worker --tail 500` to see the full picture. The `--lines` flag on the CLI logs command didn't seem to work.

**Recommendation:** Support `--lines N` or `--since 30m` properly so agents don't need to bypass the CLI and go to Docker directly.

### 7. Failed workflows from earlier runs clutter the output
`wf progress` permanently shows 18 failed workflows from earlier aborted attempts. No way to dismiss/archive them.

**Recommendation:** Add `reposwarm wf clean` or `wf dismiss-failed` to clear old failures. Or auto-archive failures from previous investigation runs when a new one starts.

---

## Workarounds I Had to Use

1. **`docker logs` instead of `reposwarm logs`** — CLI log truncation forced direct Docker access
2. **Multiple restarts** — Had to restart the worker 2-3 times before env vars were properly loaded
3. **Shell polling loop** — Wrote a `while true; sleep 120; check progress; done` loop because there's no `reposwarm investigate --wait` or `--follow` mode
4. **`grep` through worker logs** to count completed steps per repo — because progress showed 0% with no step detail
5. **`docker logs | grep "Step .* completed"` piped through sed/awk** — to understand actual progress since the CLI didn't expose it

---

## Feature Requests (from agent + human feedback)

### Priority 1 — Would save significant time

1. **Step-level progress in `wf progress`** — Show per-repo step completion, not just binary complete/incomplete
2. **Time estimates / ETA** — Track historical step durations, show predicted completion time
3. **`investigate --wait` or `--follow` mode** — Block until done, streaming progress updates. Eliminates the need for polling loops
4. **Env var verification on restart** — Confirm the worker actually has the expected env vars after restart

### Priority 2 — Would improve reliability

5. **Auto scale-down on LLM congestion** — If Bedrock returns throttling errors, automatically reduce parallelism instead of failing
6. **`reposwarm.json` project config file** — Drop a config file next to the CLI (or in the repo) with all settings. Makes sharing setups trivial instead of manual `config set` commands
7. **Fix `errors` command to include failed workflows** — Don't say "no errors" when there are 18 failed workflows
8. **Consistent health reporting** — `wf status -v` and `doctor` should agree on worker health

### Priority 3 — Quality of life

9. **`wf clean` to dismiss old failures** — Archive or remove old failed runs
10. **Better log access** — Support `--lines N`, `--since`, `--follow` on `reposwarm logs`
11. **Post-investigation summary** — When all workflows complete, output a summary (total time, tokens used, steps completed, any errors)
12. **Webhook/notification on completion** — For async workflows, ability to call a webhook or send a notification when done

---

## Conclusion

RepoSwarm is functional and the architecture it produces is solid. The investigation workflow successfully analyzed 9 repos and generated architecture docs. The core engine works.

The main pain points are around **observability during long-running operations** and **reliability of environment setup**. An agent working with this CLI spends most of its time in a frustrating poll loop staring at "0%" for 30+ minutes, unsure if things are working or stuck. The env var restart issue caused unnecessary failed runs.

With step-level progress, time estimates, and a `--follow` mode, this would go from "functional but opaque" to "genuinely pleasant to operate."

**Overall rating: 7/10** — Gets the job done, but the operator experience needs polish for autonomous agent use.

---

## v1.3.171 Follow-Up (Same Day, ~15 hours later)

Updated from v1.3.162 → v1.3.171 and re-ran the full investigation.

### What Changed (Improvements)

1. **Step-level progress in `wf progress`** ✅ FIXED
   - Now shows "Steps: 153/153 (100%) across 9 repos" and per-repo "17/17 steps"
   - This was my #1 recommendation — implemented!

2. **`reposwarm errors` now surfaces failed workflows** ✅ FIXED
   - Now says "WARNING: 18 workflows failed" instead of "No errors found"
   - Exactly what I asked for

3. **`wf prune` command** ✅ NEW
   - Can clean up old completed/failed workflows
   - Note: the pruned workflows still appeared in `wf list` afterward — might be a bug or just Temporal retention

4. **`wf watch` command** ✅ NEW
   - Real-time workflow watching (5s polling)
   - Very verbose for agent use (full workflow ID list every 5s) — a `--compact` flag would help
   - No step-level detail in watch output — only shows running/completed count

5. **More investigation steps (17 vs ~12)**
   - New steps like `feature_flags` added
   - More thorough analysis

6. **`investigate --force` flag works**
   - Cache-aware: skips recently investigated repos by default, `--force` overrides
   - Smart behavior

### What's Still Missing

1. **Time estimates / ETA** — Still no predicted completion time
2. **`investigate --wait`** — Still no blocking mode; had to build my own polling loop
3. **`wf watch --for-agent`** — Watch output is not machine-friendly (no step detail, very verbose)
4. **Worker env restart reliability** — Didn't test again (used existing config), but unclear if fixed
5. **Consistent health reporting** — Not tested this round

### Performance Comparison

| Metric | v1.3.162 (first run) | v1.3.171 (cached) |
|--------|---------------------|-------------------|
| Total wall time | ~40 min | ~4 min 20s |
| Steps per repo | ~12 | 17 |
| Failed runs before success | 2 (18 failures) | 0 |
| Env setup issues | Yes (critical) | N/A (reused) |
| Step progress visible | No (0% for 38 min) | Yes (per-repo counts) |

*Note: v1.3.171 run was mostly cache hits since repos hadn't changed. A fresh run with no cache would likely take longer than v1.3.162 due to having 17 steps instead of 12.*

### Updated Rating

**v1.3.171: 8.5/10** — Major observability improvements. Step progress alone makes this dramatically more usable for agents. The remaining gaps (ETA, --wait mode, watch improvements) are nice-to-haves now rather than blockers.
