# Risk Register
Updated 2026-10-03.

| ID | Risk | Probability | Impact | Mitigation | Status |
|----|------|-------------|--------|------------|--------|
| R1 | Domain loss. Expiry confirmed 2027-06-18 and transfer lock on (WHOIS 2026-10-09). Residual: auto-renew state, contact email and 2FA at the registrar unknown; history of last-minute manual renewals. | Low | Critical | Registrar login: turn on auto-renew, verify contact email and 2FA; calendar reminder 2027-05-01 regardless. | OPEN — downgraded 2026-10-09 |
| R2 | Data loss. Live Nextcloud is one week old with 1.4 GB. The historical data (17 GB HUB2 data dir, 67 GB March snapshot, 44 GB RJ, 27 GB Downloads) exists only on the production server's own disk, partly in trash. Old HUB2 database state unknown. Nothing automated, nothing confirmed off-server. | High | Critical | Phase 0 now: dump every database; copy backupMARCH26-compressed (21 GB) and HUB2bu26 (17 GB) off-server; then decide what the live instance must absorb (T32). | OPEN — sharpened 2026-10-09 |
| R3 | Account-recovery chain through longviewhub@gmail.com (lost/recovered May 2026) | Medium | High | Inventory what it recovers; MFA everywhere; password manager | OPEN |
| R4 | Outbound mail rejected by strict gateways (incident 2026-09-03). DMARC confirmed absent 2026-10-09; DKIM unknown. | High | High | Confirm DKIM; publish DMARC p=none with reporting; then tighten. Check IP reputation of 172.241.164.114. | OPEN — root cause narrowed |
| R5 | Unsupported Nextcloud/PHP exposed publicly | Medium | High | Baseline versions; patch path in Phase 2/3 | OPEN |
| R6 | Undocumented single-operator system | Certain | Medium | This documentation set; decide permanent home (D-002) | OPEN |
| R7 | Discovery latency: session cannot reach server or public DNS | Certain | Low–Medium | Batched read-only command blocks for Tony to run | OPEN |
| R8 | Certificate renewal: apex wildcard renews on schedule (serving cert 2026-09-05 → 2026-12-04, verified in browser). Residual risk: the mechanism is inferred, not confirmed, and nobody is alerted if a renewal fails. | Low | High | Confirm AutoSSL in the panel; add cert-expiry monitoring in Phase 6. | OPEN — downgraded 2026-10-09 after a false alarm from crt.sh lag |
| R9 | Dead hosts and abandoned installs reachable from the internet: spoke, stratus (docroots in public_html), and **cloud_v35_old (an unmaintained Nextcloud being probed by scanners today)**. | Certain | Medium–High | Take cloud_v35_old off the web first (YELLOW: rename out of public_html or deny in .htaccess); retire spoke/stratus per D-003. | OPEN — raised 2026-10-09 |
| R10 | Admin account is named `admin`, a universal brute-force target; five failed password confirmations for it on 2026-10-09 from 153.66.15.100. Brute-force protection is on. | Medium | High | Confirm whether 153.66.15.100 is Tony; later (Phase 5) create a distinctly named admin, enforce 2FA, demote `admin`. | OPEN — new 2026-10-09 |

# Known Issues
| ID | Issue | First seen | Evidence | Status |
|----|-------|------------|----------|--------|
| K1 | Mail from johnson.ross@longviewhub.io refused by a California state agency gateway | 2026-09-03 | Gmail thread (Tony re-sent via another address) | OPEN — bounce text needed |
| K2 | No 2026 domain-renewal notices in johnson.ross.a@gmail.com, unlike 2023–2025. Domain was nonetheless renewed to 2027-06-18. Either renewed early, auto-renew is on, or the notice address changed. | 2026-06 (absence) | Gmail; WHOIS | OPEN — resolve at registrar login (auto-renew + contact email) |
| K9 | Duplicate cron entries for Nextcloud cron.php (same job twice every 5 minutes, output discarded) | 2026-10-09 | crontab -l | OPEN — collapse to one line with a log, YELLOW, after baseline |
| K10 | An older Nextcloud copy (≤ v25) sits in /home/hfppyjna/backupMarch26/public_html/HUB2 on the same server; counts toward the 213 GB quota; its data directory and DB, if any, are UNKNOWN | 2026-10-09 | find + occ error | OPEN — inventory before anything is deleted |
| K11 | Host platform age: CloudLinux 7 kernel, MariaDB 10.6 (both past upstream EOL unless the host buys extended support) | 2026-10-09 | uname, mysql --version | OPEN — input to Phase 3 stay/move decision |
| K12 | cloud_v35_old (Nextcloud 35.0.1 attempt) is web-reachable at longviewhub.io/cloud_v35_old/, scanned by bots on 2026-10-09, and logs into the live instance's nextcloud.log → its config likely points at the live data directory. Two installs sharing a data dir is unsafe. | 2026-10-09 | nextcloud.log, ls public_html | OPEN — T31 |
| K13 | Five "Login failed: 'admin'" at /login/confirm from 153.66.15.100, 2026-10-09 14:53–15:28 UTC | 2026-10-09 | nextcloud.log | OPEN — ask Tony if that is his IP |
| K14 | Old HUB2 data (17 GB, nextclouddata) is not in the live instance (1.4 GB). Migration intent UNKNOWN. | 2026-10-09 | du, config | OPEN — T32 |
| K15 | No memory cache configured in the live instance (no memcache.local / memcache.locking). Performance and locking warning expected on the admin Overview page. | 2026-10-09 | config.php | OPEN — check APCu availability, T34 |
| K16 | Subdomain docroots sit inside public_html, so cloud.longviewhub.io is also reachable as longviewhub.io/cloud.longviewhub.io/ (log shows such requests). Standard cPanel layout; can be blocked in the apex .htaccess. | 2026-10-09 | nextcloud.log | OPEN — low priority |
| K8 | Registrar record "Updated 2026-10-09", same day as five DNS zone edits (K5). Cause unknown. | 2026-10-09 | WHOIS | OPEN — ask Tony whether he changed anything that day |
| K3 | No DMARC record for longviewhub.io | 2026-10-09 | Resolve-DnsName _dmarc.longviewhub.io TXT → empty | OPEN — fix is T24 |
| K4 | HEAD / from a non-browser client returns an Apache error-style page (ISO-8859-1, no Nextcloud headers). Likely WAF/ModSecurity. Users unaffected. | 2026-10-09 | PowerShell Invoke-WebRequest headers | OPEN — low priority, note for monitoring design |
| K5 | DNS zone serial 2026100905: five zone edits on 2026-10-09, origin unknown | 2026-10-09 | SOA record | OPEN — revised hypothesis: AutoSSL validation attempts for the dead subdomains; expect it to stop after D-003 |
| K6 | (Withdrawn) Renewal failure inferred from crt.sh was a false alarm: serving wildcard issued 2026-09-05, expires 2026-12-04. Residual oddity: a non-serving GoDaddy cert from 2026-06-19, origin unknown. | 2026-10-09 | browser padlock | CLOSED as false alarm; GoDaddy question folded into T03 |
| K7 | ICANN RDAP lookup cannot query .io; registrar/expiry still unverified | 2026-10-09 | lookup.icann.org | OPEN — use .io registry lookup or registrar login |
