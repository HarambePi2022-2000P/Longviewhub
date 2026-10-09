# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 (late) |
| Project phase | Phase 1 — Discover (Phase 0 Preserve starts the moment server access exists) |
| System state | Domain live on host-run DNS (server.plus / hostiso.com); server 172.241.164.114 (Leaseweb USA, NYC), Apache; mail self-hosted on the same box, SPF present, DMARC absent; TLS healthy (Let's Encrypt wildcard to 2026-12-04, auto-renewing); spoke/stratus/nvr1 are dead projects slated for retirement (D-003); Nextcloud version unknown; old backups on external HDs, currency unknown |
| What changed last | 2026-10-09: DNS set, IP ownership and certificate state captured; a certificate false alarm from crt.sh lag was corrected; D-003 retire spoke/stratus/nvr1. No infrastructure changes yet. |
| Current blockers | B01 session cannot reach public DNS or the server · B03 hosting provider inferred, plan and access path still unknown · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Tony runs the DKIM one-liner, then names the hosting panel / SSH access path (T01), then the registrar readout including the GoDaddy question |

## Waiting on Tony
1. DKIM query (T23), one line of PowerShell.
2. Hosting plan and access path (T01): panel URL, SSH yes/no, provider name on the bill.
3. Registrar readout (T03): expiry, auto-renew, lock, contact email, 2FA; and whether the domain moved to GoDaddy in June 2026.
4. Nextcloud baseline (T06) from the admin UI (05_Nextcloud/baseline_capture.md) or shell (Step 4), secrets redacted.
5. Backup media inventory (T22, 06_Backups_Recovery/backup_media_inventory.md): external HDs, workstation, 2 TB SSD.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
