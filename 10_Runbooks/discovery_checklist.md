# Discovery Checklist — Phase 1
All commands here are READ-ONLY (GREEN). None change the server, DNS, or any account.
Paste raw output back. Redact anything that looks like a password, key, token, or `secret`/`passwordsalt`/`dbpassword` line.

## Step 1 — Public records, from your own machine (not the server). ~10 min.

What it does: reads the domain's public DNS records, registration data, the HTTPS certificate, and the web server's response headers. Reveals nameserver provider, mail provider, SPF/DMARC state, cert issuer/expiry, and usually the hosting provider (from who owns the IP).

**Linux / macOS (Terminal):**
```bash
for t in NS A AAAA MX TXT SOA CAA; do echo "== $t"; dig +short $t longviewhub.io; done
echo "== DMARC"; dig +short TXT _dmarc.longviewhub.io
echo "== WHOIS"; whois longviewhub.io | grep -Ei 'registrar|expir|status|name server|updated'
echo "== HTTP"; curl -sI https://longviewhub.io | head -20
echo "== CERT"; echo | openssl s_client -servername longviewhub.io -connect longviewhub.io:443 2>/dev/null | openssl x509 -noout -issuer -subject -dates
echo "== IP owner"; whois "$(dig +short A longviewhub.io | head -1)" | grep -Ei 'orgname|org-name|netname|descr' | head -5
```

**Windows (PowerShell):**
```powershell
foreach ($t in 'NS','A','AAAA','MX','TXT','SOA') { "== $t"; Resolve-DnsName longviewhub.io -Type $t -ErrorAction SilentlyContinue | Format-Table -AutoSize }
"== DMARC"; Resolve-DnsName _dmarc.longviewhub.io -Type TXT -ErrorAction SilentlyContinue | Format-Table -AutoSize
"== HTTP"; (Invoke-WebRequest -Uri https://longviewhub.io -Method Head -MaximumRedirection 0 -ErrorAction SilentlyContinue).Headers
```
WHOIS and the certificate on Windows: open https://lookup.icann.org/ and https://www.ssllabs.com/ssltest/analyze.html?d=longviewhub.io in a browser.

**Any OS, browser:** open `https://crt.sh/?q=%25.longviewhub.io` and paste the list of hostnames that have had certificates. This is the fastest way to find forgotten subdomains.

## Step 2 — Registrar (browser). Read-only.
Log into the registrar (the renewal notices point to qunatum.com). Read off and paste:
expiry date · auto-renew on/off · registrar lock on/off · registrant/admin contact email · nameservers · whether 2FA is on.
If auto-renew or lock is off, turning them on is YELLOW (billing/config change) and your call; I recommend both on.

## Step 3 — Hosting control panel, or confirm self-hosted (browser).
Provider name · plan type (shared / VPS / dedicated / home machine) · OS · PHP versions offered and which is selected for the Nextcloud domain · disk used/total · list of mail accounts · list of databases · is SSH offered · is there a built-in backup feature and when it last ran.

## Step 4 — Shell baseline on the server (SSH). Read-only.
What it does: identifies the OS, disk state, PHP CLI version, finds Nextcloud's `occ` tool, and reads Nextcloud's own status report. Nothing is modified.

```bash
echo "== OS"; cat /etc/os-release 2>/dev/null | head -3; uname -r
echo "== DISK"; df -h
echo "== PHP CLI"; php -v | head -1
echo "== FIND OCC"; find / -name occ -path '*nextcloud*' -not -path '*/vendor/*' 2>/dev/null; find ~ -name occ -not -path '*/vendor/*' 2>/dev/null
```
Then, substituting the directory that contains `occ` for `/PATH`:
```bash
cd /PATH
echo "== STATUS"; php occ status
echo "== APPS"; php occ app:list
echo "== KEY CONFIG"; php occ config:system:get version; php occ config:system:get datadirectory; php occ config:system:get dbtype; php occ config:system:get dbhost; php occ config:system:get trusted_domains; php occ config:system:get overwrite.cli.url
echo "== JOB MODE"; php occ config:app:get core backgroundjobs_mode
echo "== CRON"; crontab -l 2>/dev/null
echo "== DATA SIZE"; du -sh "$(php occ config:system:get datadirectory)" 2>/dev/null
echo "== LOG"; ls -lh "$(php occ config:system:get datadirectory)/nextcloud.log" 2>/dev/null; tail -n 20 "$(php occ config:system:get datadirectory)/nextcloud.log" 2>/dev/null
echo "== DB"; (mysql --version || mariadb --version || psql --version) 2>/dev/null
```
Notes:
- On a VPS where the web server owns the files, prefix each `php occ` with `sudo -u www-data` (Debian/Ubuntu) or `sudo -u apache` (RHEL family).
- On cPanel shared hosting there is no sudo; run as your account user. If `php -v` shows a different version than the panel assigns to the site, use the panel's PHP binary, e.g. `/usr/local/bin/ea-php82 occ status`. The two versions matter: occ must run on the same PHP major that serves the site.
- Do NOT run `occ upgrade`, `updater.phar`, or anything under `occ maintenance:` yet. Baseline first.

## Step 5 — Nextcloud admin UI (browser). If no shell.
Settings → Administration → Overview: paste the version line and every item under "Security & setup warnings".
Settings → Administration → System: PHP version, database type/version, memory, disk.
Settings → Administration → Logging: level and the most recent 10 entries.
Apps: list of enabled apps and anything flagged as incompatible.

## Step 6 — Backups.
Host-side snapshots or backup feature (name, schedule, last run, retention) · any manual copies (where, how old) · any off-site copy · has a restore ever been done.

## Step 7 — Tailscale admin console (browser).
List of machines with OS and last-seen; mark which one is the server (if any). Note any exposed services or subnet routes.

## Step 8 — Credential inventory. Locations only, never values.
For each of: registrar · hosting panel · SSH · Nextcloud admin · DB · SMTP · longviewhub@gmail.com · johnson.ross.a@gmail.com · Tailscale: where is the credential stored, is MFA on, which email recovers it.
