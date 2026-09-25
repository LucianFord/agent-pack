# Benchmark — security-auditor & surgeon-reviewer × OWASP NodeGoat

Date: 2026-09-25 · Target: [OWASP NodeGoat](https://github.com/OWASP/NodeGoat)
(the industry-standard deliberately-vulnerable training app, with a documented vulnerability list — i.e., ground truth you can check our output against)

Method: sub-agents executed the pack's config files verbatim against a fresh clone.
No human steering, no cherry-picking of the target.

## Results

**security-auditor** (full-pack tier, whole-app review):

- 8 findings: **3 CRITICAL** (hardcoded session secret → forge any user's session;
  default admin credentials; `eval()` on request body → RCE) + **5 HIGH**
  (Mongo `$where` injection, IDOR, missing admin middleware, stored XSS, SSRF)
- **Every vulnerability class in NodeGoat's official tutorial list was found. Zero misses.**
- Discipline check: a real CVE in `underscore` was correctly downgraded to *unreachable*
  (no `_.template` call anywhere) — no CVE-padding
- Every finding: `path:line`, reachable attack path, minimal fix, verification step
- Sweep: 30 files read in full + 7 repo-wide grep passes

**surgeon-reviewer** (free sample, 4 files only):

- 5 WILL BREAK findings (plaintext credentials, ReDoS via catastrophic backtracking,
  session fixation, username enumeration, 1-char passwords accepted) → verdict: DO NOT SHIP
- 3 suspicions actively disproved and *not* reported ("trigger sequence unrealistic")
  — the most expensive property of a reviewer is not wasting your time

## Why this matters

Free prompt collections claim quality. This pack publishes a benchmark against a target
with public ground truth. Reproduce it yourself: clone NodeGoat, load the free
`surgeon-reviewer`, point it at `app/routes/session.js` + `app/data/user-dao.js`.
