# Nextcloud Install Inventory — account hfppyjna on us05
Captured 2026-10-09 from cPanel Terminal. Versions read from each copy's `version.php` (no PHP executed). CONFIRMED unless marked.

## Live instance
| Field | Value |
|-------|-------|
| URL | https://cloud.longviewhub.io (trusted_domains: localhost, cloud.longviewhub.io). The apex longviewhub.io has no index file, only a 305-byte .htaccess dated 2026-10-07; INFERRED it redirects to the cloud subdomain (confirm by reading it). |
| Code directory | /home/hfppyjna/public_html/cloud.longviewhub.io |
| Version | **33.0.9.1** (occ status: installed, maintenance false, needsDbUpgrade false) |
| Config | config.php 1,196 bytes, modified 2026-10-07 17:18 |
| Data directory | /home/hfppyjna/clouddata — **1.4 GB** |
| Database | MySQL/MariaDB, localhost, name hfppyjna_cloud33, prefix oc_ |
| Background jobs | mode cron; lastcron 2026-10-09 15:25:02 EDT, 57 s before the check → **cron is working** |
| Log | clouddata/nextcloud.log, 3.8 MB |
| Cache | no memcache.* keys in config → no memory cache configured (K15) |
| Apps | 76 enabled (incl. activity, calendar, contacts, mail, maps, memories, music, notes, phonetrack, photos, spreed/Talk, tasks, text, oidc, mcp, secrets, end_to_end_encryption, twofactor_totp/email/backupcodes, bruteforcesettings, suspicious_login); 31 disabled (incl. encryption, files_antivirus, recognize, user_ldap). Full list in the 2026-10-09 capture. |
| Admin account | username is literally `admin` (from log entries) |

## Every other Nextcloud / ownCloud copy on the account
| Path | Version | What it looks like | Status |
|------|---------|--------------------|--------|
| public_html/cloud_v35_old | 35.0.1 | a separate install attempt, dirs dated 2026-10-06; **still web-reachable at longviewhub.io/cloud_v35_old/ and being hit by scanners**; its log lines land in clouddata/nextcloud.log, so its config points at the live data directory or log file (K12) | ABANDONED, EXPOSED |
| clouddata_v35 (home) | — | data dir for the v35 attempt, 2026-10-06 | ABANDONED |
| backupMarch26/public_html/HUB2 | 24.0.12 | the previous production install ("HUB2"), upgraded 24.0.4 → 24.0.12 by the built-in updater on 2026-03-13 | BACKUP COPY (67 GB folder = full home snapshot of 2026-03-13) |
| backupMarch26/public_html/hub | 24.0.4 | an even earlier "hub" install | BACKUP COPY |
| nextclouddata (home) | — | **17 GB**; contains updater-*/backups/nextcloud-24.0.4.1 → this is HUB2's data directory | OLD PRODUCTION DATA, on server only |
| HUB2bu26 (home) | — | 17 GB, dated 2026-10-01, same updater backup inside → a copy of nextclouddata made 2026-10-01 | BACKUP COPY |
| .trash/HUB2, .trash/HUB2bu26 | 24.0.12 | HUB2 code moved to cPanel trash | TRASH |
| hubdata (home) | — | 2026-04-01; size small (not in top 8) | UNKNOWN |
| RJ/NC_old/nextcloud, backupMarch26/RJ/NC_old/nextcloud | 21.0.9 | the original 2021-era install | ARCHIVE |
| Downloads/public_html/nextcloud | 21.0.9 | another 2021 copy | ARCHIVE |
| Downloads/OC, Downloads/public_html/OC | ownCloud 10.10.0 | ownCloud trial | ARCHIVE |
| Downloads/Zphoto, Downloads/public_html/Zphoto, backupMarch26/Zphoto | Zenphoto | photo gallery trial | ARCHIVE |

## Timeline (INFERRED from dates and versions)
- 2021-06-17: account created (dotfiles). Nextcloud 21.0.9 era.
- 2022–2025: "hub" then "HUB2" on Nextcloud 24.0.4, data in nextclouddata (17 GB).
- 2026-03-13: full home snapshot → backupMarch26 (67 GB) + backupMARCH26-compressed (21 GB); HUB2 updated 24.0.4 → 24.0.12.
- 2026-10-01: nextclouddata copied to HUB2bu26; HUB2 code moved to trash.
- 2026-10-06: v35 install attempt (cloud_v35_old, clouddata_v35).
- 2026-10-07: **fresh Nextcloud 33.0.9 installed** at cloud.longviewhub.io, new DB hfppyjna_cloud33, data dir clouddata (1.4 GB); apex .htaccess written.
- Reading: Tony is mid-rebuild. The live instance is one week old and holds 1.4 GB. The 17 GB of HUB2 data has not (yet) been brought into it. Whether that 17 GB is wanted is the key open question (T32).

## Where the 213 GB sits (du, 2026-10-09)
| Path | Size |
|------|------|
| backupMarch26 | 67 GB |
| RJ | 44 GB (contents UNKNOWN beyond NC_old) |
| Downloads | 27 GB |
| backupMARCH26-compressed | 21 GB |
| nextclouddata | 17 GB |
| HUB2bu26 | 17 GB |
| public_html | 13 GB |
| ziDVB66i (single file in home, 2022-06-15, mode 600) | 4.8 GB — UNKNOWN blob |
