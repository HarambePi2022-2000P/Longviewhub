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

## D-004 — How to migrate HUB2 into the live 33.0.9 instance (PENDING)
- Date raised: 2026-10-09. Tony's requirement: everything from HUB2 (files, calendars, contacts, shares, "everything else") into the new instance.
- Option A, **import into the fresh instance**: copy user file trees into clouddata and run `occ files:scan`; extract calendars and contacts from the HUB2 database (iCalendar and vCard blobs in oc_calendarobjects / oc_cards) and import them through the Calendar and Contacts apps; recreate shares by hand (two users). Keeps the new instance and its app setup. Loses: share links and tokens, file version history, trash, activity log, per-app data not re-imported.
- Option B, **upgrade HUB2 in place to 33 and make it the production instance**: stage the 24.0.12 code + nextclouddata + a copy of its DB under a temporary path, step 24→25→26→27→28→29→30→31→32→33 with PHP switched 8.1 → 8.2 → 8.3 via MultiPHP, then swap it into cloud.longviewhub.io and copy the 1.4 GB of new files in. Preserves everything Nextcloud knows about. Costs: nine upgrade hops on shared hosting (time limits, memory), three PHP switches, the week-old instance's app configuration is discarded, higher chance of a mid-sequence failure that needs a restore.
- PM recommendation: **Option A**, because the new instance is the one Tony wants to keep, there are two users so shares are few, and nine sequential upgrades through PHP changes on shared hosting is the riskier operation. Choose B only if version history, trash contents or existing share links matter.
- Reversibility: both start from the Phase 0 snapshot and leave HUB2's data untouched; A can be redone, B can be abandoned at any hop.
- Status: awaiting Tony's choice.

## D-005 — Remove the abandoned Nextcloud 35 install
- Date: 2026-10-09. Decision: retire public_html/cloud_v35_old, clouddata_v35 and database hfppyjna_cloud35. Authorized by Tony ("delete whatever files you need to get the 35 stuff out of there"); he reinstalled 33 on 2026-10-07 for app compatibility.
- Execution: staged. Step 1 now: move cloud_v35_old and clouddata_v35 out of the web root into ~/retired/ (YELLOW, reversible by moving back) — this ends the public exposure immediately. Step 2 after the Phase 0 snapshot is verified: delete ~/retired/* and drop hfppyjna_cloud35 (RED).
- Why not delete immediately: cloud_v35_old's config points at the live data directory; the code dir itself holds no data, but the rule is no deletion before an off-server snapshot exists. The move achieves the security goal with zero data risk.
- Affected: web root, quota (≈1.1 GB freed at step 2), MySQL.
