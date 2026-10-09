# Nextcloud Baseline Capture
Instance: https://cloud.longviewhub.io (the apex holds only a .htaccess, presumably a redirect). Captured 2026-10-09 via cPanel Terminal. Redact anything that looks like a secret.

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
| Nextcloud version | **33.0.9.1** live (CONFIRMED 2026-10-09). Older copies: 35.0.1 (abandoned attempt), 24.0.12 (previous production), 21.0.9 (2021). See install_inventory.md |
| PHP version (web) | ea-php83 on longviewhub.io and cloud.longviewhub.io (CONFIRMED 2026-10-09) |
| PHP version (CLI) | 8.3.35 (CONFIRMED 2026-10-09) |
| Database type / version | MariaDB 10.6.28; DB hfppyjna_cloud33 on localhost, prefix oc_ |
| Data directory path | /home/hfppyjna/clouddata |
| Data directory size | 1.4 GB (live). Old HUB2 data: 17 GB in nextclouddata, not in the live instance |
| Background job mode / last run | cron; lastcron 57 s old at check → working (duplicate crontab line, K9) |
| Setup warnings (count + list) | UNKNOWN — admin Overview page still needed; at least "no memory cache" expected |
| Enabled apps | 76 (list captured 2026-10-09) |
| Incompatible / outdated apps | none flagged by app:list |
| Users (count) | UNKNOWN |
| External storage configured? | UNKNOWN |
| Server-side encryption on? | `encryption` app disabled; `end_to_end_encryption` enabled (client-side keys, user-held) |
| Hosting provider / plan | HostISO (INFERRED), cPanel shared hosting on CloudLinux 7, 700 GB quota, 213 GB used |
| Shell access? | Yes, cPanel Terminal (CONFIRMED) |

## Access policy
- Tony's personal password is never pasted into chat or stored here.
- If Claude ever needs API access to the instance (WebDAV / OCS) from a session that can reach longviewhub.io, use a Nextcloud **app password** (Personal settings → Security → Devices & sessions → Create new app password). It is scoped, visible in the device list, and revocable with one click. Record only that one exists, never its value.
- This cloud session cannot reach longviewhub.io at all (egress blocked), so no credential helps here. Read-only inspection happens either by Tony pasting the fields above, or in a session running on Tony's own computer.
