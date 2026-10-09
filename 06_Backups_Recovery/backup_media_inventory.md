# Backup Media Inventory
Fill one block per drive or location. Read-only: look, don't move or delete. Record dates and sizes, never credentials.

Known so far (Tony, 2026-10-06): old backups on external HDs; maybe a copy on the workstation; maybe a copy on a 2 TB SSD (currently locked/encrypted).
**Server-side (2026-10-09): every copy on the server lives on the same disk and account as production. Nothing is known to exist off the server except whatever is on the home drives. The database state of the old HUB2 install (dump or live DB) is UNKNOWN.**

| ID | Media | Label / model / capacity | Last written (newest file date) | Total size of backup | Contains data dir? | Contains DB dump (.sql/.sql.gz)? | Contains config.php? | Nextcloud version in backup (from version.php or config) | Readable today? | Notes |
|----|-------|--------------------------|----------------------------------|----------------------|--------------------|----------------------------------|----------------------|-----------------------------------------------------------|-----------------|-------|
| M1 | External HD #1 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M2 | External HD #2 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M3 | Workstation (Tycho) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M4 | 2 TB SSD (locked) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | Needs unlocking by Tony; encryption type UNKNOWN |
| S1 | **on-server** /home/hfppyjna/backupMarch26 | full home snapshot | 2026-03-13 | 67 GB | yes (nextclouddata inside) | UNKNOWN — no .sql found at depth ≤3 | yes (HUB2/config) | 24.0.12 | yes | same disk as production: a restore point, not a backup |
| S2 | **on-server** backupMARCH26-compressed | archive of S1? | 2026-03-13 | 21 GB | presumably | UNKNOWN | presumably | 24.0.12 | yes | portable; best candidate to copy off-server first |
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
