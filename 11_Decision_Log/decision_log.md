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
