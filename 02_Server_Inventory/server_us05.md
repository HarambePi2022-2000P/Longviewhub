# Server — hosting account on "us05"
Captured 2026-10-09 from cPanel → Terminal (read-only). CONFIRMED unless marked.

| Item | Value | Status |
|------|-------|--------|
| Hostname (as seen from shell) | us05; server clock America/New_York (EDT) | CONFIRMED |
| Public IP | 172.241.164.114 (Leaseweb USA, NYC) | CONFIRMED |
| Hosting model | **cPanel shared hosting** on CloudLinux: kernel 3.10.0-962.3.2.lve1.5.83.el7 (CloudLinux 7, LVE kernel). No root; everything runs as the account user. | CONFIRMED (kernel) / INFERRED (CloudLinux 7) |
| Provider | HostISO (INFERRED from nameservers and SOA; "us05" fits a numbered shared fleet) | INFERRED |
| Account user / home | hfppyjna / /home/hfppyjna | CONFIRMED |
| Account quota | 700 GB (716,800 MB) limit; **213 GB used** (218,111 MB); 896,845 inodes, no inode limit. Only 1.4 GB of that is the live Nextcloud. | CONFIRMED |
| Server volume | /dev/sda4: 58 TB, 69% used (shared with other tenants; informational only) | CONFIRMED |
| PHP, shell | 8.3.35 (CLI), built 2026-09-30 | CONFIRMED |
| PHP, per vhost | longviewhub.io → ea-php83 · cloud.longviewhub.io → ea-php83 · spoke.longviewhub.io → ea-php83 | CONFIRMED |
| Database server | MariaDB 10.6.28 (client reports 10.6.28; server version assumed same, confirm) | CONFIRMED (client) |
| Web server | Apache with HTTP/2 (from headers) | CONFIRMED |
| SSH / Terminal | Available (cPanel Terminal; SSH keys UNKNOWN) | CONFIRMED |
| Control-panel backup feature | UNKNOWN — check cPanel → Backup / JetBackup | UNKNOWN |

## Platform-age notes
- CloudLinux 7 is past vendor end-of-life (mid-2024) unless the host pays for extended support. MariaDB 10.6 reached end-of-life in mid-2026. Both are the host's responsibility, not ours, but they bear on the Phase 3 stay-or-move decision. INFERRED from version strings.

## Directory layout (partial)
| Path | What | Status |
|------|------|--------|
| /home/hfppyjna/public_html/ | apex docroot (longviewhub.io): no index file; `.htaccess` (305 B, 2026-10-07), `.well-known/`, `paladin/` (2022), `Ftp/` (2023), plus the subdomain docroots below | CONFIRMED |
| /home/hfppyjna/public_html/cloud.longviewhub.io/ | **live Nextcloud 33.0.9** | CONFIRMED |
| /home/hfppyjna/public_html/cloud_v35_old/ | abandoned Nextcloud 35.0.1 attempt, web-reachable | CONFIRMED |
| /home/hfppyjna/public_html/spoke.longviewhub.io/ | spoke docroot (2026-03-19), retire per D-003 | CONFIRMED |
| /home/hfppyjna/public_html/stratus.longviewhub.io/ | stratus docroot (2026-03-13), no vhost in PHP list, retire per D-003 | CONFIRMED |
| /home/hfppyjna/clouddata | live Nextcloud data, 1.4 GB | CONFIRMED |
| /home/hfppyjna/nextclouddata, HUB2bu26, backupMarch26, backupMARCH26-compressed, RJ, Downloads, ziDVB66i | old data, backups and archives totalling ≈200 GB — see 05_Nextcloud/install_inventory.md | CONFIRMED |
| /home/hfppyjna/mail, etc | cPanel mailboxes and mail config for the domain | CONFIRMED (exists) |
| /home/hfppyjna/.ssh | exists (2026-09-22); keys UNKNOWN | CONFIRMED (exists) |

## Cron (account crontab)
```
*/5 * * * * /opt/cpanel/ea-php83/root/usr/bin/php -f /home/hfppyjna/public_html/cloud.longviewhub.io/cron.php
*/5 * * * * /usr/local/bin/ea-php83 -f /home/hfppyjna/public_html/cloud.longviewhub.io/cron.php
```
Two entries, same job, same binary by two paths, both every 5 minutes, output discarded. Harmless duplication (Nextcloud locks its job runner) but worth collapsing to one line with logging (K9).
