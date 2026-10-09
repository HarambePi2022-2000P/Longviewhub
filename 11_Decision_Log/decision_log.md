# Decision Log

## D-001 — LiPoNarc repository is out of scope
- Date: 2026-10-03
- Decision: The GitHub repo this session launched from (HarambePi2022-2000P/LiPoNarc, Android battery manager) is unrelated to LongviewHub. No LongviewHub material will be committed there.
- Reason: Tony confirmed it is unrelated; the session simply launched from it.
- Alternatives: commit docs there anyway (rejected: pollutes an unrelated codebase).
- Tradeoffs: none.
- Reversibility: full.
- Affected systems: documentation location only.

## D-002 — Permanent home for LongviewHub documentation
- Date raised: 2026-10-03 · Decided: 2026-10-04
- Decision: private GitHub repository **Longviewhub** (github.com/HarambePi2022-2000P/Longviewhub; project name "LongviewHub Retool"), owned by Tony. Claude reads and writes it across sessions. Git history is the audit trail for documentation; infrastructure changes still go in 12_Change_Log.
- Options: (a) private GitHub repo `longviewhub-ops` — versioned, readable/writable by Claude across sessions, git history doubles as change log; (b) folder in the longviewhub@gmail.com Drive — simple, but that account was lost and recovered in May 2026 and has no version history; (c) folder inside Nextcloud itself — rejected for now: the system under repair should not be the only copy of its own recovery documentation.
- Recommendation: (a).
- Status: DECIDED by Tony 2026-10-04. Repo created by Tony 2026-10-09 (an earlier attempt under a different name never became visible); first push the same day.

## D-003 — Retire spoke, stratus and nvr1
- Date: 2026-10-09
- Decision: the subdomains spoke.longviewhub.io, stratus.longviewhub.io and nvr1.longviewhub.io (and their www variants) are old projects and will be retired.
- Reason: Tony: "old projects, not really working on those anymore, they can be deleted."
- Alternatives considered: keep dormant (rejected: dead names under a 2-year includeSubDomains HSTS policy cause daily certificate-validation noise and are attack surface for nothing).
- Tradeoffs: none identified, provided nothing worth keeping sits behind them.
- Reversibility: DNS records are reversible if recorded before deletion. Deleting directories or databases is not. Execution is therefore staged: (1) record each name's target and what its document root / database contains; (2) delete the DNS records and panel subdomain entries (YELLOW); (3) delete directories and databases only after Tony sees the inventory and says so per item (RED).
- Affected systems: DNS zone, hosting panel subdomain list, AutoSSL, possibly home devices and Tailscale nodes.
- Status: DECIDED; execution blocked on hosting-panel access (T01).

## Lesson recorded 2026-10-09 — crt.sh lag
- A certificate-renewal failure was declared from crt.sh history and was wrong; the serving certificate had renewed on 2026-09-05 and crt.sh had not indexed it. Rule: read the serving certificate (browser padlock or openssl) before concluding anything about renewal state. crt.sh is a lower bound on what has been issued.
