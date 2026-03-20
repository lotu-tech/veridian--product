# Lessons Learned — 2026-03-20 Pipeline Cycle

## Issues Processed
- **#2** Remove the cronjob from the api process (investigation-api)
- **#3** CSP Issue (investigation-front)

## Key Observations

### 1. Tester mock design matters
The tester agent wrote 31 tests but 8 had Jest mock isolation flaws (`beforeEach` clearing mocks before assertions could read them). The developer correctly identified these as test design issues and documented them, but couldn't fix them (not its role). When the tester was re-dispatched, it fixed all 8 in just 2 turns/$0.30. **Lesson:** Tester agents should validate mock lifecycle in their own tests before advancing — add a self-check step.

### 2. Repo onboarder adds latency but is valuable
`investigation-front` had no CLAUDE.md. The onboarder took a full run (112K chars output) before the architect could start. This added ~5 min but the architect then completed in only 7 turns/$0.20 — the CLAUDE.md context likely helped. **Lesson:** One-time cost per repo, pays off in agent efficiency.

### 3. Scoper repos table injection was critical
Issue #3 was stuck in Architecture with no `project:` label because the scoper had no repos table. After fixing the runner to inject repos.yml data, the scoper correctly mapped "CSP" → `investigation-front` in 9 turns/$0.05. **Lesson:** Agents need structured reference data, not just issue text.

### 4. Developer was thorough but expensive relative to task
Developer used 46 turns/$0.59 on #2 — mostly fixing the 8 test mock issues the tester should have caught. **Lesson:** Better test quality from the tester reduces developer cost.

### 5. Architect output for #3 was clean and minimal
CSP fix correctly identified as a 1-line change to `next.config.js` — `connect-src` needs `https://api.veridianosint.com.br` and `wss://api.veridianosint.com.br`. Good scoping by the architect.

## Cost Summary
| Agent | Issue | Turns | Cost |
|-------|-------|-------|------|
| Scoper | #3 | 9 | $0.05 |
| Tester | #2 | 2 | $0.30 |
| Developer | #2 | 46 | $0.59 |
| Architect | #3 | 7 | $0.20 |
| **Total** | | **64** | **$1.14** |
