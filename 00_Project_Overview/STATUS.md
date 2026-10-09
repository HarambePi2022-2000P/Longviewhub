# LongviewHub Retool — Current Status
**Read this first at the start of every session.** Keep it short; details live in the other files.

| Field | Value |
|-------|-------|
| Status as of | 2026-10-09 |
| Project phase | Phase 1 — Discover (Phase 0 Preserve starts the moment server access exists) |
| System state | Domain live; mailbox working with a deliverability problem; Nextcloud at https://longviewhub.io (version/host unknown); old backups on external HDs, currency unknown |
| What changed last | 2026-10-09: documentation repo Longviewhub created by Tony and populated (first push). 2026-10-06: Nextcloud location and backup media confirmed. No infrastructure changes yet. |
| Current blockers | B01 session cannot reach public DNS or the server · B03 hosting provider unknown · B04 credential locations unknown |
| Active queue | T01 hosting/access · T03 domain renewal state · T07 public lookups · T02/T06 server baseline · T04 backup existence |
| Best next action | Tony runs Step 1 of `10_Runbooks/discovery_checklist.md` from his own machine and pastes the output |

## Waiting on Tony
1. Step 1 public-lookup output (DNS, WHOIS, cert, crt.sh list) from his own machine.
2. Registrar readout: expiry, auto-renew, lock, contact email, nameservers, 2FA.
3. Hosting provider, plan, and access path (panel / SSH) for the server behind longviewhub.io.
4. Nextcloud baseline from the admin UI (05_Nextcloud/baseline_capture.md) or shell (Step 4), secrets redacted.
5. Backup media inventory (06_Backups_Recovery/backup_media_inventory.md): external HDs, workstation, 2 TB SSD.

## Session routine for Claude
1. Read this file, then `backlog.md` (active queue and blocker log).
2. Fold any new evidence from Tony into the registries and risk register; mark each fact CONFIRMED / INFERRED / HYPOTHESIS / UNKNOWN.
3. Every infrastructure change goes in `12_Change_Log` with rollback; every consequential choice in `11_Decision_Log`.
4. Update this file last, commit, push.
