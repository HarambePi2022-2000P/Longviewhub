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
| Web PHP handler | `ea-php83___lsphp` in the apex .htaccess → Apache with CloudLinux mod_lsapi (LiteSpeed PHP SAPI). PHP modules: bcmath, gd, gmp, intl, redis (client module only), Zend OPcache. **No APCu, no imagick.** CloudLinux PHP Selector is present (`~/.cl.selector`), so extensions can be enabled self-service (T34). |
| Apps | 76 enabled (incl. activity, calendar, contacts, mail, maps, memories, music, notes, phonetrack, photos, spreed/Talk, tasks, text, oidc, mcp, secrets, end_to_end_encryption, twofactor_totp/email/backupcodes, bruteforcesettings, suspicious_login); 31 disabled (incl. encryption, files_antivirus, recognize, user_ldap). Full list in the 2026-10-09 capture. |
| Admin account | username is literally `admin` (from log entries). Old HUB2 had users `admin` and `claire` (data dir listing). |

## Databases in the account (uapi Mysql list_databases, 2026-10-09)
| Database | Size | Belongs to | Status |
|----------|------|-----------|--------|
| hfppyjna_cloud33 | 32 MB | live 33.0.9 | LIVE |
| hfppyjna_cloud35 | 12 MB | abandoned v35 attempt | retire after Phase 0 snapshot |
| hfppyjna_hub2bu26 | 41 MB | copy of HUB2's DB made ~2026-10-01 (by name and size) | KEEP — secondary copy |
| hfppyjna_next815 | 36 MB | **HUB2's production database** (HUB2 config.php: dbname hfppyjna_next815, datadirectory /home/hfppyjna/nextclouddata, version 24.0.12.1). CONFIRMED 2026-10-09. | KEEP — **migration source** |
| hfppyjna_net2f13 | 0.4 MB | UNKNOWN; tiny | KEEP until identified |

## Apex longviewhub.io
The apex `.htaccess` contains only `RewriteEngine on` and the cPanel PHP handler. **No redirect, no index file**: https://longviewhub.io/ serves an Apache 403 (directory listing denied), which is what the earlier HEAD probe saw (K4 explained). Nextcloud lives only at https://cloud.longviewhub.io. Decision pending on whether the apex should redirect there (subdomain strategy, K17).

## Every other Nextcloud / ownCloud copy on the account
| Path | Version | What it looks like | Status |
|------|---------|--------------------|--------|
| ~/retired/cloud_v35_old (moved 2026-10-09, C-001) | 35.0.1.1 | 992 MB of code. Tony installed 35 on 2026-10-06 and reinstalled 33 on 10-07 for app compatibility (Nextcloud cannot downgrade, so 33 is a fresh install). Its config.php points **datadirectory at /home/hfppyjna/clouddata, the live data directory**, and at DB hfppyjna_cloud35. Web-reachable and scanned (K12). Deleting the code directory does not touch data. | RETIRE — authorized by Tony 2026-10-09 (D-005) |
| ~/retired/clouddata_v35 (moved 2026-10-09, C-001) | — | 111 MB, 2026-10-06; an earlier data dir for the v35 attempt | DELETE with cloud_v35_old after Phase 0 snapshot |
| backupMarch26/public_html/HUB2 | 24.0.12 | the previous production install ("HUB2"), upgraded 24.0.4 → 24.0.12 by the built-in updater on 2026-03-13 | BACKUP COPY (67 GB folder = full home snapshot of 2026-03-13) |
| backupMarch26/public_html/hub | 24.0.4 | an even earlier "hub" install | BACKUP COPY |
| nextclouddata (home) | — | **17 GB**; users `admin`, `claire`; appdata for two instance ids; updater backups → **HUB2's data directory**. Migration source. | OLD PRODUCTION DATA, on server only |
| HUB2bu26 (home) | — | 17 GB, dated 2026-10-01; same users and appdata as nextclouddata plus an extra appdata_oc0zgoqljxuh dated 2026-09-28 (a short-lived instance used this dir around 09-28; `backup_2026-09-28` in home is from the same day) | BACKUP COPY |
| .trash/HUB2, .trash/HUB2bu26 | 24.0.12 | HUB2 code moved to cPanel trash | TRASH |
| hubdata (home) | — | now **empty** (4 KB, 2026-04-01). But backupMARCH26-compressed/hubdata.tar.gz is **12 GB** and backupMarch26/hubdata exists → the "hub" (24.0.4) data directory was 12+ GB in March and was emptied afterwards. Whether that content lives on in HUB2's data is UNKNOWN (K20). | ARCHIVE — identify before deletion |
| RJ/NC_old (33 GB) + RJ/NC_old.tar.gz (11 GB) | 21.0.9 | the original 2021-era install **with 33 GB of content**. Whether its data was carried into hub/HUB2 is UNKNOWN (K19). | ARCHIVE — identify before deletion |
| Downloads/public_html (27 GB) incl. nextcloud 21.0.9, OC, Zphoto, LuxCal, subhome | mixed | a copy of an old public_html, 2022 era | ARCHIVE — identify before deletion |
| public_html/paladin (9.1 GB, 2022-09) | — | UNKNOWN; sits in the web root, reachable at longviewhub.io/paladin/ (K18) | UNKNOWN — ask Tony |
| ~/ziDVB66i (4.8 GB file, 2022-06-15, mode 600) | — | UNKNOWN blob; also present in backupMarch26 | UNKNOWN — ask Tony or `file` it |
| Downloads/OC, Downloads/public_html/OC | ownCloud 10.10.0 | ownCloud trial | ARCHIVE |
| Downloads/Zphoto, Downloads/public_html/Zphoto, backupMarch26/Zphoto | Zenphoto | photo gallery trial | ARCHIVE |

## Timeline (INFERRED from dates and versions)
- 2021-06-17: account created (dotfiles). Nextcloud 21.0.9 era.
- 2022–2025: "hub" then "HUB2" on Nextcloud 24.0.4, data in nextclouddata (17 GB).
- 2026-03-13: full home snapshot → backupMarch26 (67 GB) + backupMARCH26-compressed (21 GB); HUB2 updated 24.0.4 → 24.0.12.
- 2026-10-01: nextclouddata copied to HUB2bu26; HUB2 code moved to trash.
- 2026-10-06: v35 install attempt (cloud_v35_old, clouddata_v35).
- 2026-10-07: **fresh Nextcloud 33.0.9 installed** at cloud.longviewhub.io, new DB hfppyjna_cloud33, data dir clouddata (1.4 GB); apex .htaccess written.
- 2026-10-09 Tony: **everything from HUB2 (files, calendars, contacts, shares) is to be migrated into the live 33.0.9 instance** (T32 answered). Approach to be decided (D-004).
- Reading: a sequence of rebuild attempts (09-28, 10-06, 10-07) left several installs and data copies on the box. The live instance is one week old and holds 1.4 GB.

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
