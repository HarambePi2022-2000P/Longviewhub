# Nextcloud Baseline Capture
Instance: https://longviewhub.io (apex). Fill from the admin UI or `occ`. Redact anything that looks like a secret.

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
| Nextcloud version | UNKNOWN |
| PHP version (web) | UNKNOWN |
| PHP version (CLI) | UNKNOWN |
| Database type / version | UNKNOWN |
| Data directory path | UNKNOWN |
| Data directory size | UNKNOWN |
| Background job mode / last run | UNKNOWN |
| Setup warnings (count + list) | UNKNOWN |
| Enabled apps | UNKNOWN |
| Incompatible / outdated apps | UNKNOWN |
| Users (count) | UNKNOWN |
| External storage configured? | UNKNOWN |
| Server-side encryption on? | UNKNOWN (matters for backups: encrypted data needs the keys) |
| Hosting provider / plan | UNKNOWN |
| Shell access? | UNKNOWN |

## Access policy
- Tony's personal password is never pasted into chat or stored here.
- If Claude ever needs API access to the instance (WebDAV / OCS) from a session that can reach longviewhub.io, use a Nextcloud **app password** (Personal settings → Security → Devices & sessions → Create new app password). It is scoped, visible in the device list, and revocable with one click. Record only that one exists, never its value.
- This cloud session cannot reach longviewhub.io at all (egress blocked), so no credential helps here. Read-only inspection happens either by Tony pasting the fields above, or in a session running on Tony's own computer.
