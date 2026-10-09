# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 (late) |
| Project phase | Phase 0 — Preserve (now concrete) running alongside Phase 1 — Discover (server mapped 2026-10-09) |
| System state | Domain registered at eNom (via qunatum.com), expires 2027-06-18, transfer-locked; DNS on the host's cluster (server.plus / hostiso.com); **cPanel shared hosting** (CloudLinux 7, host "us05", 172.241.164.114, Leaseweb USA NYC), Apache, PHP 8.3, MariaDB 10.6, 213 GB of 700 GB used; **live Nextcloud 33.0.9 at cloud.longviewhub.io, one week old, 1.4 GB, cron healthy**; ≈200 GB of old installs, data dirs and backups on the same disk (HUB2 v24 data 17 GB; March snapshot 67 GB; RJ 44 GB; Downloads 27 GB); an abandoned v35 install web-reachable and being scanned; admin user named `admin` with failed logins today; mail self-hosted on the same box, SPF present, DMARC absent; TLS healthy (Let's Encrypt wildcard to 2026-12-04, auto-renewing); spoke/stratus/nvr1 are dead projects slated for retirement (D-003); Nextcloud version unknown; old backups on external HDs, currency unknown |
| What changed last | 2026-10-09 (late): Tony decided HUB2 is to be migrated into the live instance (T32) and authorized removing the v35 install (D-005). Databases inventoried (five). Phase 0 snapshot plan written. Apex confirmed to serve a 403, not a redirect. **First infrastructure change made (C-001): v35 install moved out of the web root.** Option A chosen for the migration (D-004). Phase 0 tar job running. |
| Current blockers | B01 session cannot reach public DNS or the server (Tony relays via cPanel Terminal) · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Finish Phase 0 (T04): mysqldump the five databases, wait for the tar job's DONE, verify, copy ≈41 GB to the 2 TB SSD |

## Waiting on Tony
1. T04 Phase 0: run the mysqldump block (one password prompt), wait for `DONE` in ~/phase0/tar.log, run the verify loop, unlock the 2 TB SSD, SFTP ~/phase0/* and ~/backupMARCH26-compressed/* down, confirm sizes.
2. T37: one line each on paladin (9 GB), RJ/NC_old (33 GB), hubdata.tar.gz (12 GB), Downloads/public_html (27 GB), ziDVB66i (5 GB file).
3. T23: cPanel → Email → Email Deliverability, screenshot the longviewhub.io row (DKIM/SPF state).
4. T03: log into the registrar (qunatum.com): auto-renew ON, contact email current, 2FA ON.
5. T22: unlock the 2 TB SSD and report free space (it is the Phase 0 destination); inventory the external HDs when convenient.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
