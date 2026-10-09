# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 (late) |
| Project phase | Phase 1 — Discover (Phase 0 Preserve starts the moment server access exists) |
| System state | Domain live on host-run DNS (server.plus / hostiso.com); server 172.241.164.114 (Leaseweb USA, NYC), Apache; mail self-hosted on the same box, SPF present, DMARC absent; **certificate auto-renewal stopped in July 2026** (apex on a GoDaddy cert to 2027-01-03; spoke expires 2026-10-11); three undocumented hosts: spoke, stratus, nvr1; Nextcloud version unknown; old backups on external HDs, currency unknown |
| What changed last | 2026-10-09: DNS set, IP ownership and certificate history captured; certificate-renewal failure found (R8); spoke/stratus/nvr1 discovered (R9). No infrastructure changes yet. |
| Current blockers | B01 session cannot reach public DNS or the server · B03 hosting provider inferred, plan and access path still unknown · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Tony reads the serving certificate on longviewhub.io and spoke.longviewhub.io (browser padlock), resolves spoke/stratus/nvr1, runs the DKIM one-liner |

## Waiting on Tony
1. Certificates (T26): padlock readout for longviewhub.io and spoke.longviewhub.io; then the hosting panel's SSL/TLS Status page.
2. spoke / stratus / nvr1 (T25): what they are, plus their A records.
3. DKIM query (T23), one line.
4. Registrar readout (T03): expiry, auto-renew, lock, contact email, 2FA. ICANN lookup cannot do .io; log in, or use the .io registry lookup.
5. Hosting plan and access path (panel / SSH) for the server behind longviewhub.io.
6. Nextcloud baseline from the admin UI (05_Nextcloud/baseline_capture.md) or shell (Step 4), secrets redacted.
7. Backup media inventory (06_Backups_Recovery/backup_media_inventory.md): external HDs, workstation, 2 TB SSD.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
