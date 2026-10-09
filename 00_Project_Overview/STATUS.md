# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 (late) |
| Project phase | Phase 0 — Preserve (now concrete) running alongside Phase 1 — Discover (server mapped 2026-10-09) |
| System state | Domain registered at eNom (via qunatum.com), expires 2027-06-18, transfer-locked; DNS on the host's cluster (server.plus / hostiso.com); **cPanel shared hosting** (CloudLinux 7, host "us05", 172.241.164.114, Leaseweb USA NYC), Apache, PHP 8.3, MariaDB 10.6, 213 GB of 700 GB used; **live Nextcloud 33.0.9 at cloud.longviewhub.io, one week old, 1.4 GB, cron healthy**; ≈200 GB of old installs, data dirs and backups on the same disk (HUB2 v24 data 17 GB; March snapshot 67 GB; RJ 44 GB; Downloads 27 GB); an abandoned v35 install web-reachable and being scanned; admin user named `admin` with failed logins today; mail self-hosted on the same box, SPF present, DMARC absent; TLS healthy (Let's Encrypt wildcard to 2026-12-04, auto-renewing); spoke/stratus/nvr1 are dead projects slated for retirement (D-003); Nextcloud version unknown; old backups on external HDs, currency unknown |
| What changed last | 2026-10-09 (late): Tony decided HUB2 is to be migrated into the live instance (T32) and authorized removing the v35 install (D-005). Databases inventoried (five). Phase 0 snapshot plan written. Apex confirmed to serve a 403, not a redirect. No infrastructure changes yet; the v35 move is the first and is pending Tony's keystroke. |
| Current blockers | B01 session cannot reach public DNS or the server (Tony relays via cPanel Terminal) · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Tony runs the v35 move (T31), then starts the Phase 0 snapshot (T04), then picks the migration approach (D-004) |

## Waiting on Tony
1. T31: run the one-line move of cloud_v35_old and clouddata_v35 to ~/retired/ and confirm.
2. T04 Phase 0: download the five database backups (cPanel → Backup), run the tar block, unlock the 2 TB SSD, copy ≈41 GB down, verify.
3. D-004: choose migration approach A (import into the fresh 33) or B (upgrade HUB2 through nine majors). PM recommends A.
4. T37: one line each on paladin (9 GB), RJ/NC_old (33 GB), hubdata.tar.gz (12 GB), Downloads/public_html (27 GB), ziDVB66i (5 GB file).
5. T23: cPanel → Email → Email Deliverability, screenshot the longviewhub.io row (DKIM/SPF state).
6. T03: log into the registrar (qunatum.com): auto-renew ON, contact email current, 2FA ON.
7. T22: unlock the 2 TB SSD and report free space (it is the Phase 0 destination); inventory the external HDs when convenient.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
