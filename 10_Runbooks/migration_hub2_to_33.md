# Runbook — Migrate HUB2 (Nextcloud 24.0.12) into the live 33.0.9 instance, Option A (D-004)
Status: READY, blocked on Phase 0 verification (T04). Written 2026-10-09. Every step says GREEN / YELLOW / RED and has a rollback.

## Source and target
| | Source (HUB2) | Target (live) |
|---|---|---|
| Code | ~/.trash/HUB2 and backupMarch26/public_html/HUB2 (24.0.12.1, not runnable on PHP 8.3) | ~/public_html/cloud.longviewhub.io (33.0.9.1) |
| Data dir | /home/hfppyjna/nextclouddata (17 GB; users admin, claire) | /home/hfppyjna/clouddata (1.4 GB) |
| Database | hfppyjna_next815 (36 MB; copy hfppyjna_hub2bu26) | hfppyjna_cloud33 |
| Table prefix | oc_ (assumed; confirm) | oc_ |

Principle: the source is never modified. Everything is copied. HUB2 stays intact until the migration is verified and a fresh post-migration snapshot exists.

## Pre-flight (GREEN)
```bash
cd ~/public_html/cloud.longviewhub.io
php occ user:list
php occ maintenance:mode   # expect: disabled
ls ~/nextclouddata/*/files -d
du -sh ~/nextclouddata/admin/files ~/nextclouddata/claire/files
```
- Users present in the new instance must match the source usernames (admin, claire). Create any missing user in the admin UI or with `php occ user:add <name>` (YELLOW) before copying files.
- Quota check: clouddata grows by ≈17 GB; 487 GB free. Fine.

## Step 1 — Files (YELLOW: writes into the live data dir; reversible by deleting the copied trees)
Copy, don't move. One user at a time. Run under nohup like the tar job.
```bash
nohup bash -c 'for u in admin claire; do mkdir -p ~/clouddata/$u/files; rsync -a --info=progress2 ~/nextclouddata/$u/files/ ~/clouddata/$u/files/; done; echo RSYNC_DONE' > ~/phase0/rsync.log 2>&1 &
```
Wait for RSYNC_DONE, then register the files with Nextcloud:
```bash
cd ~/public_html/cloud.longviewhub.io && php occ files:scan --all
```
Verify: file counts in the Files app match `find ~/nextclouddata/<user>/files -type f | wc -l` per user. Rollback: `rm -rf ~/clouddata/<user>/files/<copied folders>` then `occ files:scan --all` (RED; only if the copy is wrong).

Note on conflicts: the new instance already holds 1.4 GB. rsync without `--delete` never removes target files; a same-named file is overwritten by the HUB2 version. If any of the 1.4 GB was re-uploaded from HUB2 already, nothing is lost. If something newer was created in the last week under the same path, it would be overwritten; run `rsync -a --dry-run --itemize-changes` first and read the list.

## Step 2 — Calendars and tasks (GREEN to extract, YELLOW to import)
Calendars live in the old DB as iCalendar blobs (oc_calendarobjects, one VEVENT/VTODO per row) grouped by oc_calendars. Extract one .ics per calendar with the script below (reads the DB only; writes files to ~/phase0/export/). Then import each .ics through the Calendar app (Calendar settings → Import calendar), choosing the target calendar.
```bash
mkdir -p ~/phase0/export && cd ~/phase0/export
read -rsp "cPanel password: " MYSQL_PWD; echo; export MYSQL_PWD
mysql -u hfppyjna -N -r -B hfppyjna_next815 -e "SELECT id, principaluri, displayname FROM oc_calendars" | while IFS=$'\t' read -r id principal name; do
  user=${principal##*/}; safe=$(echo "${user}_${name}" | tr -c 'A-Za-z0-9_-' '_')
  { echo "BEGIN:VCALENDAR"; echo "VERSION:2.0"; echo "PRODID:-//LongviewHub migration//EN";
    mysql -u hfppyjna -N -r -B hfppyjna_next815 -e "SELECT calendardata FROM oc_calendarobjects WHERE calendarid=$id AND deleted_at IS NULL" \
      | awk '/^BEGIN:(VEVENT|VTODO|VTIMEZONE|VJOURNAL)/{p=1} p{print} /^END:(VEVENT|VTODO|VTIMEZONE|VJOURNAL)/{p=0}';
    echo "END:VCALENDAR"; } > "$safe.ics"
  echo "$safe.ics: $(grep -c '^BEGIN:VEVENT' "$safe.ics") events, $(grep -c '^BEGIN:VTODO' "$safe.ics") tasks"
done
unset MYSQL_PWD
```
If `deleted_at` does not exist in NC 24's schema, drop `AND deleted_at IS NULL`. Verify: event counts per calendar match what the Calendar app shows after import.

## Step 3 — Contacts (GREEN to extract, YELLOW to import)
```bash
cd ~/phase0/export
read -rsp "cPanel password: " MYSQL_PWD; echo; export MYSQL_PWD
mysql -u hfppyjna -N -r -B hfppyjna_next815 -e "SELECT id, principaluri, displayname FROM oc_addressbooks" | while IFS=$'\t' read -r id principal name; do
  user=${principal##*/}; safe=$(echo "${user}_${name}" | tr -c 'A-Za-z0-9_-' '_')
  mysql -u hfppyjna -N -r -B hfppyjna_next815 -e "SELECT carddata FROM oc_cards WHERE addressbookid=$id" > "$safe.vcf"
  echo "$safe.vcf: $(grep -c '^BEGIN:VCARD' "$safe.vcf") contacts"
done
unset MYSQL_PWD
```
Import each .vcf through the Contacts app (Settings → Import). Verify counts.

## Step 4 — Shares (GREEN to list; recreated by hand)
Produce a readable list of what was shared with whom, then recreate the ones that still matter in the new instance.
```bash
read -rsp "cPanel password: " MYSQL_PWD; echo; export MYSQL_PWD
mysql -u hfppyjna hfppyjna_next815 -e "SELECT s.id, s.share_type, s.uid_owner, s.share_with, s.permissions, s.expiration, f.path FROM oc_share s LEFT JOIN oc_filecache f ON f.fileid=s.file_source ORDER BY s.uid_owner, f.path" > ~/phase0/export/shares.txt
unset MYSQL_PWD; cat ~/phase0/export/shares.txt
```
share_type 0 = user, 1 = group, 3 = public link, 4 = email. Public links get new URLs; anyone holding an old link must be sent the new one.

## Step 5 — Anything else from HUB2 (ask Tony)
Covered automatically by Step 1: Notes (files), Photos, Music, documents. Not covered: PhoneTrack sessions, Deck boards, Bookmarks, Forms, Talk history, app passwords, mail-app account settings. If any of those were used in HUB2, add a targeted export step before cleanup.

## Step 6 — Verify, then snapshot again
- Log in as each user; spot-check folders, a calendar, a contact, a share.
- `php occ files:scan --all` reports no errors; `php occ status` unchanged.
- Take a fresh snapshot of clouddata + cloud33 (same method as Phase 0) and copy it off-server.

## Step 7 — Cleanup (RED, each item on Tony's explicit go after Step 6)
Candidates: ~/retired/*, hfppyjna_cloud35, ~/.trash/HUB2 and ~/.trash/HUB2bu26, HUB2bu26 and hfppyjna_hub2bu26 (duplicates of nextclouddata/next815). nextclouddata and next815 themselves stay until Tony is satisfied the new instance is complete, then are archived off-server before deletion.
