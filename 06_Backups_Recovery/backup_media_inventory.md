# Backup Media Inventory
Fill one block per drive or location. Read-only: look, don't move or delete. Record dates and sizes, never credentials.

Known so far (Tony, 2026-10-06): old backups on external HDs; maybe a copy on the workstation; maybe a copy on a 2 TB SSD (currently locked/encrypted).

| ID | Media | Label / model / capacity | Last written (newest file date) | Total size of backup | Contains data dir? | Contains DB dump (.sql/.sql.gz)? | Contains config.php? | Nextcloud version in backup (from version.php or config) | Readable today? | Notes |
|----|-------|--------------------------|----------------------------------|----------------------|--------------------|----------------------------------|----------------------|-----------------------------------------------------------|-----------------|-------|
| M1 | External HD #1 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M2 | External HD #2 | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M3 | Workstation (Tycho) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | |
| M4 | 2 TB SSD (locked) | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | UNKNOWN | Needs unlocking by Tony; encryption type UNKNOWN |

## How to fill it in (Windows, read-only)
Plug in one drive at a time. In PowerShell, replace `E:` with the drive letter:
```powershell
Get-Volume | Select-Object DriveLetter, FileSystemLabel, FileSystem, Size, SizeRemaining | Format-Table -AutoSize
Get-ChildItem E:\ -Directory | Select-Object Name, LastWriteTime | Format-Table -AutoSize
Get-ChildItem E:\ -Recurse -Include *.sql,*.sql.gz,config.php,version.php -ErrorAction SilentlyContinue | Select-Object FullName, Length, LastWriteTime | Format-Table -AutoSize
```
What it does: lists volumes, lists top-level folders with dates, and finds the three files that tell us whether a backup is complete (a database dump, the Nextcloud config, and the version marker). Nothing is written.

A Nextcloud backup is only complete if it has all three: the **data directory**, a **database dump**, and **config.php**. Data without the database restores files but loses shares, users, calendars, contacts and app data. Database without data is a skeleton.
