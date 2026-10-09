# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 (late) |
| Project phase | Phase 0 — Preserve (now concrete) running alongside Phase 1 — Discover (server mapped 2026-10-09) |
| System state | Domain registered at eNom (via qunatum.com), expires 2027-06-18, transfer-locked; DNS on the host's cluster (server.plus / hostiso.com); **cPanel shared hosting** (CloudLinux 7, host "us05", 172.241.164.114, Leaseweb USA NYC), Apache, PHP 8.3, MariaDB 10.6, 213 GB of 700 GB used; **live Nextcloud 33.0.9 at cloud.longviewhub.io, one week old, 1.4 GB, cron healthy**; ≈200 GB of old installs, data dirs and backups on the same disk (HUB2 v24 data 17 GB; March snapshot 67 GB; RJ 44 GB; Downloads 27 GB); an abandoned v35 install web-reachable and being scanned; admin user named `admin` with failed logins today; mail self-hosted on the same box, SPF present, DMARC absent; TLS healthy (Let's Encrypt wildcard to 2026-12-04, auto-renewing); spoke/stratus/nvr1 are dead projects slated for retirement (D-003); Nextcloud version unknown; old backups on external HDs, currency unknown |
| What changed last | 2026-10-09: account fully mapped (05_Nextcloud/install_inventory.md): live 33.0.9 instance baselined, every old copy located and sized, backups found to be on-server only. No infrastructure changes yet. |
| Current blockers | B01 session cannot reach public DNS or the server (Tony relays via cPanel Terminal) · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Tony answers the data question (T32) and the IP question (K13), and pastes the third block (database inventory, apex .htaccess, v35 config) |

## Waiting on Tony
1. T32: is the live 1.4 GB everything, or does the 17 GB HUB2 data (and its calendars/contacts/shares in the old DB) need to come into the new instance?
2. K13: is 153.66.15.100 your IP (failed `admin` password confirmations today)?
3. Third cPanel Terminal block: database list with sizes, apex .htaccess, cloud_v35_old config (safe keys), remaining folder sizes, APCu availability.
4. DKIM query (T23), one line of PowerShell.
5. Registrar login (T03): auto-renew, contact email, 2FA; and whether Tony changed anything at registrar/host on 2026-10-09.
6. Backup media inventory (T22): external HDs, workstation, 2 TB SSD. Plan name and provider on the hosting bill.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
