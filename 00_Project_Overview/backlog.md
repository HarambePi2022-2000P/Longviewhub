# Master Backlog — LongviewHub.io Reboot
Updated 2026-10-03. Categories: BLOCKER · CRITICAL · HIGH · NORMAL · LOW · LATER. Track: Recovery (R) / Modernization (M) / Future (F).

| ID | Cat | Track | Task | Depends on | Status |
|----|-----|-------|------|------------|--------|
| T01 | HIGH | R | Hosting access: **cPanel, SSH available** (2026-10-09). Provider INFERRED HostISO on Leaseweb USA (NYC). Remaining: plan type, provider name on the bill, panel URL recorded (location only). | Tony | MOSTLY ANSWERED — no longer a blocker |
| T02 | HIGH | R | Server baseline: OS, disk, quota, PHP, DB, cron captured 2026-10-09 (02_Server_Inventory/server_us05.md). Remaining: live Nextcloud occ output, docroot map. | — | MOSTLY DONE |
| T03 | HIGH | R | Domain: expiry 2027-06-18 and transfer lock confirmed 2026-10-09. Remaining at registrar login: auto-renew ON, contact email current, 2FA on. Registrar is eNom via qunatum.com. | Registrar login | OPEN — narrowed, downgraded from CRITICAL |
| T04 | CRITICAL | R | Take a fresh Phase 0 snapshot (config.php, DB dump, data copy or at minimum data listing) before any change. Old backups exist (T22) but are not current. | T01, T02 | OPEN |
| T05 | CRITICAL | R | Credential and recovery-email inventory for registrar, host, SSH, Nextcloud admin, DB, SMTP, Google accounts (locations only) | Tony | OPEN |
| T06 | CRITICAL | R | Record Nextcloud version, PHP version (web + CLI), DB type/version, data dir, enabled apps, setup warnings | T02 | OPEN |
| T07 | HIGH | R | Pull public DNS record set + WHOIS + TLS cert + crt.sh subdomain list from Tony's machine. DNS set done 2026-10-09; IP owner, WHOIS, cert, crt.sh pending | Tony | PARTLY DONE |
| T08 | HIGH | R | Diagnose 2026-09-03 mail rejection. SPF present, DMARC absent (2026-10-09). Still to do: DKIM check (T23), bounce text, blocklist check of 172.241.164.114 | T23 | OPEN — narrowed |
| T23 | HIGH | R | Query DKIM: `Resolve-DnsName default._domainkey.longviewhub.io -Type TXT` (and check the host panel's Email Deliverability page). GREEN. | Tony | OPEN |
| T24 | HIGH | R | Publish DMARC: `_dmarc.longviewhub.io TXT "v=DMARC1; p=none; rua=mailto:<a mailbox Tony reads>"`. YELLOW (DNS change; reversible by deleting the record). Do after T23 so reports are meaningful. Tighten to p=quarantine later. | T23 | OPEN |
| T09 | HIGH | R | Determine Nextcloud upgrade path (sequential majors), PHP compatibility per step, DB compatibility | T06 | OPEN |
| T10 | HIGH | R | Confirm TLS issuer, expiry, renewal mechanism; cover all live hostnames. crt.sh history captured 2026-10-09; superseded by T26 for the fix. | T07 | PARTLY DONE |
| T25 | HIGH | R | Identify spoke, stratus, nvr1. Answered 2026-10-09: old projects, retire (D-003). | Tony | DONE |
| T27 | NORMAL | R | Retire spoke, stratus, nvr1 per D-003, staged: record targets and contents → remove DNS records and panel subdomains (YELLOW) → delete directories/databases per item on Tony's go (RED). Also remove any related Tailscale nodes. | T01 | OPEN |
| T26 | NORMAL | R | Certificates: serving cert verified healthy 2026-10-09 (expires 2026-12-04). Remaining: confirm the renewal mechanism (AutoSSL?) in the panel; note the non-serving GoDaddy cert's origin. GREEN. | T01 | OPEN — downgraded |
| T11 | HIGH | R | Map Tailscale nodes; identify which is the server; confirm whether admin UIs are public or tailnet-only | Tailscale console | OPEN |
| T12 | NORMAL | R | Complete service registry and DNS registry from discovery output | T07, T02 | OPEN |
| T13 | NORMAL | R | Write restore runbook and upgrade runbook for Nextcloud | T04, T09 | OPEN |
| T14 | NORMAL | R | Decide: keep longviewhub@gmail.com as a dependency or retire it; document what it recovers | T05 | OPEN |
| T15 | NORMAL | M | Backup architecture toward 3-2-1 with scheduled verification and one tested restore | T04 | OPEN |
| T16 | NORMAL | M | Monitoring: disk, cert expiry, cron heartbeat, backup success, service up | T02 | OPEN |
| T17 | NORMAL | M | Hosting platform decision: stay / upgrade plan / migrate (risk, effort, data preservation) | T06, T09 | OPEN |
| T18 | LOW | M | Subdomain strategy and DNS registry cleanup | T07, T17 | OPEN |
| T19 | LOW | M | Outbound SMTP relay through a reputable provider if host IP reputation is the deliverability cause | T08 | OPEN |
| T20 | LATER | F | Containerization / reverse proxy / SSO / infra-as-code | Stable, backed-up, documented system | PARKED |
| T22 | CRITICAL | R | Inventory existing backup media: each external HD, Tony's workstation, the 2 TB SSD. For each: date, size, what it contains (data dir? DB dump? config.php?), readable? Record in 06_Backups_Recovery. | Tony's machine | OPEN |
| T28 | HIGH | R | Map the account: domains → docroots, what sits in public_html (apex), the live Nextcloud version from version.php, config.php safe keys, data directory and size, lastcron, app list. GREEN. | Tony | OPEN |
| T29 | NORMAL | R | Inventory backupMarch26 (size, contents, whether it has a DB dump) and decide archive-offsite vs delete (K10). Deletion is RED. | T28 | OPEN |
| T21 | LATER | F | Mine the 2026-05-09 ChatGPT export in Drive for prior setup notes (≈110 MB JSON; needs local grep, not this session) | Tony's machine | PARKED |

## Active Work Queue (3–7 items)
1. T23 — DKIM query. Next: Tony runs the one-liner.
2. T28 — account map + live Nextcloud baseline. Next: Tony pastes the second read-only block in cPanel Terminal.
3. T03 — registrar hygiene (auto-renew, contact, 2FA). Next: registrar login readout.
4. T22 — backup media inventory. Next: Tony plugs drives in one at a time.
5. T06 — Nextcloud baseline. Next: admin Overview/System pages.
6. T27 — retire spoke/stratus/nvr1. Next: waits on T01.

## Blocker Log
| ID | Blocker | Blocks | Opened | Status |
|----|---------|--------|--------|--------|
| B01 | Session egress proxy blocks DNS, RDAP, crt.sh, and longviewhub.io itself | T07 from this session | 2026-10-03 | OPEN — workaround: run from Tony's machine |
| B02 | No server access from this session | T02, T04, T06 | 2026-10-03 | OPEN — Tony relays via cPanel Terminal; workable |
| B03 | Hosting provider unknown | T01 → nearly everything | 2026-10-03 | CLOSED 2026-10-09 — cPanel + SSH confirmed; provider inferred HostISO |
| B04 | Credential locations unknown | T05 | 2026-10-03 | OPEN |
| B05 | Documentation has no permanent home | Continuity across sessions | 2026-10-03 | CLOSED 2026-10-09 — repo github.com/HarambePi2022-2000P/Longviewhub (D-002) |
