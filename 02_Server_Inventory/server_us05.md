# Server — hosting account on "us05"
Captured 2026-10-09 from cPanel → Terminal (read-only). CONFIRMED unless marked.

| Item | Value | Status |
|------|-------|--------|
| Hostname (as seen from shell) | us05 | CONFIRMED |
| Public IP | 172.241.164.114 (Leaseweb USA, NYC) | CONFIRMED |
| Hosting model | **cPanel shared hosting** on CloudLinux: kernel 3.10.0-962.3.2.lve1.5.83.el7 (CloudLinux 7, LVE kernel). No root; everything runs as the account user. | CONFIRMED (kernel) / INFERRED (CloudLinux 7) |
| Provider | HostISO (INFERRED from nameservers and SOA; "us05" fits a numbered shared fleet) | INFERRED |
| Account user / home | hfppyjna / /home/hfppyjna | CONFIRMED |
| Account quota | 700 GB (716,800 MB) limit; **213 GB used** (218,111 MB); 896,845 inodes, no inode limit | CONFIRMED |
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
| /home/hfppyjna/public_html/ | apex docroot (longviewhub.io). Contents UNKNOWN. | UNKNOWN |
| /home/hfppyjna/public_html/cloud.longviewhub.io/ | **live Nextcloud** (both cron lines run its cron.php under ea-php83) | INFERRED (strong) |
| /home/hfppyjna/backupMarch26/public_html/HUB2/ | an **older Nextcloud copy** in a March-2026 backup folder; its `occ` refuses PHP ≥ 8.2, so that copy is Nextcloud 25 or earlier | CONFIRMED (error text) |
| spoke.longviewhub.io docroot | exists as a vhost; path UNKNOWN; slated for retirement (D-003) | UNKNOWN |

## Cron (account crontab)
```
*/5 * * * * /opt/cpanel/ea-php83/root/usr/bin/php -f /home/hfppyjna/public_html/cloud.longviewhub.io/cron.php
*/5 * * * * /usr/local/bin/ea-php83 -f /home/hfppyjna/public_html/cloud.longviewhub.io/cron.php
```
Two entries, same job, same binary by two paths, both every 5 minutes, output discarded. Harmless duplication (Nextcloud locks its job runner) but worth collapsing to one line with logging (K9).
