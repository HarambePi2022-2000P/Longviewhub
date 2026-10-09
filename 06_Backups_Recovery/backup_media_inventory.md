# Backup Media Inventory
Fill one block per drive or location. Read-only: look, don't move or delete. Record dates and sizes, never credentials.

Known so far (Tony, 2026-10-06): old backups on external HDs; maybe a copy on the workstation; maybe a copy on a 2 TB SSD (currently locked/encrypted).
**Server-side (2026-10-09): every copy on the server lives on the same disk and account as production. Nothing is known to exist off the server except whatever is on the home drives. The database state of the old HUB2 install (dump or live DB) is UNKNOWN.**

| ID | Media | Label / model / capacity | Last written (newest file date) | Total size of backup | Contains data dir? | Contains DB dump (.sql/.sql.gz)? | Contains config.php? | Nextcloud version in backup (from version.php or config) | Readable today? | Notes |
|----|-------|--------------------------|----------------------------------|----------------------|--------------------|----------------------------------|----------------------|-----------------------------------------------------------|-----------------|-------|
| M1 | External HD #1 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M2 | External HD #2 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M3 | Workstation (Tycho) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M4 | 2 TB SSD, now unlocked | D: "2TB pSSD1", SanDisk hardware-encrypted (Unlocker utility shows as E:), exFAT, 1.82 TB, **915 GB free** (2026-10-09) | UNKNOWN (not yet inventoried) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | yes | **Phase 0 destination.** Also note W: WorkingFiles 128 GB NTFS (108 free), C: 952 GB (372 free). |
| S1 | **on-server** /home/hfppyjna/backupMarch26 | full home snapshot | 2026-03-13 | 67 GB | yes (nextclouddata inside) | UNKNOWN — no .sql found at depth ≤3 | yes (HUB2/config) | 24.0.12 | yes | same disk as production: a restore point, not a backup |
| S2 | **on-server** backupMARCH26-compressed | hubdata.tar.gz (12 GB) + nextclouddata.tar.gz (9.7 GB) | 2026-03-13 | 21 GB | yes (both data dirs) | no | no | 24.x data | yes | portable; copy off-server in Phase 0 |
| S3 | **on-server** HUB2bu26 | copy of old data dir | 2026-10-01 | 17 GB | yes | UNKNOWN | UNKNOWN | 24.0.12 data | yes | same disk |
| S4 | **on-server** nextclouddata | old production data dir itself | 2026-10-01 | 17 GB | yes | n/a | n/a | 24.0.12 data | yes | not a backup; the original |
| S5 | **on-server** .trash/HUB2, .trash/HUB2bu26 | old code in cPanel trash | 2026-10 | UNKNOWN | no | no | config? | 24.0.12 | yes | trash is purged by cPanel; do not rely on it |

## How to fill it in (Windows, read-only)
Plug in one drive at a time. In PowerShell, replace `E:` with the drive letter:
```powershell
Get-Volume | Select-Object DriveLetter, FileSystemLabel, FileSystem, Size, SizeRemaining | Format-Table -AutoSize
Get-ChildItem E:\ -Directory | Select-Object Name, LastWriteTime | Format-Table -AutoSize
Get-ChildItem E:\ -Recurse -Include *.sql,*.sql.gz,config.php,version.php -ErrorAction SilentlyContinue | Select-Object FullName, Length, LastWriteTime | Format-Table -AutoSize
```
What it does: lists volumes, lists top-level folders with dates, and finds the three files that tell us whether a backup is complete (a database dump, the Nextcloud config, and the version marker). Nothing is written.

A Nextcloud backup is only complete if it has all three: the **data directory**, a **database dump**, and **config.php**. Data without the database restores files but loses shares, users, calendars, contacts and app data. Database without data is a skeleton.


## Phase 0 snapshot plan (2026-10-09)
Goal: one complete, off-server copy of everything that cannot be recreated, before any migration or deletion.
1. **Databases** (≈120 MB total): cPanel has no Backup page, and the cPanel user is not a MySQL login (K23). Use `mysqldump` with each database's own user and password read from the matching Nextcloud config.php into shell variables (never printed): cloud33 ← live config; next815, hub2bu26, net2f13 ← HUB2 config (backupMarch26/public_html/HUB2); cloud35 ← ~/retired/cloud_v35_old config. Success test: the dump ends with "Dump completed". Fallback for any FAILED database: cPanel → phpMyAdmin → database → Export → Go.
2. **Data directories and mail**: first run 2026-10-09 16:36–16:51. clouddata-2026-10-09.tar.gz (1.1 G) and mail-2026-10-09.tar.gz (731 M) **verified OK**. nextclouddata-2026-10-09.tar.gz **truncated at ≈4.8 GiB** (K22); being redone as split parts `nextclouddata-2026-10-09.tar.gz.part-aa, -ab, …` with a server-side verification of the concatenation. Reassemble with `cat *.part-* > nextclouddata-2026-10-09.tar.gz` (Linux) or `copy /b` (Windows) only when a restore needs it; the parts are the backup.
3. **Existing March tarballs** (21 GB): hubdata.tar.gz and nextclouddata.tar.gz from backupMARCH26-compressed.
4. **Download** items 2–3 to D:\LongviewHub\phase0\2026-10-09\ on the 2 TB SSD via SFTP (WinSCP or FileZilla: SFTP, the cPanel hostname, port 22, user hfppyjna, cPanel password). ≈41 GB; hours on a home connection; run overnight. exFAT handles files over 4 GB.
5. **Verify**: file sizes match; `tar -tzf` lists each archive without error; one .sql.gz opens.
6. Later: RJ/NC_old.tar.gz (11 GB) and Downloads/public_html once Tony has said what they are (T37).
Not done until step 5 passes. Only then: v35 cleanup, migration, deletions.
