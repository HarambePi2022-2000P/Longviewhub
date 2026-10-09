# Nextcloud Baseline Capture
Instance: Tony says https://longviewhub.io; the server says the app directory is public_html/cloud.longviewhub.io (cron, vhost list). Whether the apex serves Nextcloud directly, redirects to cloud.longviewhub.io, or holds something else is UNKNOWN (next block). Redact anything that looks like a secret.

## From the admin UI (no shell needed)
Log in as admin → profile icon → **Administration settings**.
1. **Overview**: copy the version line and every item under "Security & setup warnings" verbatim.
2. **System**: PHP version, database type and version, memory, disk usage of the data directory.
3. **Logging**: current log level and the 10 most recent entries (redact file paths that include usernames if you prefer).
4. **Apps → Your apps**: list enabled apps; note anything flagged "not compatible" or "update available".
5. **Basic settings → Background jobs**: which mode is selected (AJAX / Webcron / Cron) and "last job ran" time.

## Fields to populate
| Field | Value |
|-------|-------|
| Nextcloud version | UNKNOWN for the live install. The March-2026 backup copy (backupMarch26/public_html/HUB2) is ≤ 25 (refuses PHP ≥ 8.2). |
| PHP version (web) | ea-php83 on longviewhub.io and cloud.longviewhub.io (CONFIRMED 2026-10-09) |
| PHP version (CLI) | 8.3.35 (CONFIRMED 2026-10-09) |
| Database type / version | MariaDB 10.6.28 (client; server to confirm) |
| Data directory path | UNKNOWN |
| Data directory size | UNKNOWN |
| Background job mode / last run | Cron every 5 min against public_html/cloud.longviewhub.io/cron.php (duplicated); whether it succeeds is UNKNOWN until `lastcron` is read |
| Setup warnings (count + list) | UNKNOWN |
| Enabled apps | UNKNOWN |
| Incompatible / outdated apps | UNKNOWN |
| Users (count) | UNKNOWN |
| External storage configured? | UNKNOWN |
| Server-side encryption on? | UNKNOWN (matters for backups: encrypted data needs the keys) |
| Hosting provider / plan | HostISO (INFERRED), cPanel shared hosting on CloudLinux 7, 700 GB quota, 213 GB used |
| Shell access? | Yes, cPanel Terminal (CONFIRMED) |

## Access policy
- Tony's personal password is never pasted into chat or stored here.
- If Claude ever needs API access to the instance (WebDAV / OCS) from a session that can reach longviewhub.io, use a Nextcloud **app password** (Personal settings → Security → Devices & sessions → Create new app password). It is scoped, visible in the device list, and revocable with one click. Record only that one exists, never its value.
- This cloud session cannot reach longviewhub.io at all (egress blocked), so no credential helps here. Read-only inspection happens either by Tony pasting the fields above, or in a session running on Tony's own computer.
