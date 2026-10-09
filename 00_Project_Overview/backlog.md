# Master Backlog — LongviewHub.io Reboot
Updated 2026-10-03. Categories: BLOCKER · CRITICAL · HIGH · NORMAL · LOW · LATER. Track: Recovery (R) / Modernization (M) / Future (F).

| ID | Cat | Track | Task | Depends on | Status |
|----|-----|-------|------|------------|--------|
| T01 | BLOCKER | R | Identify hosting provider, plan type, and access path (panel / SSH / SFTP). Provider INFERRED HostISO on Leaseweb USA (NYC); dedicated IP suggests VPS/dedicated; plan and access still UNKNOWN | Tony | PARTLY ANSWERED |
| T02 | BLOCKER | R | Obtain read-only baseline of the server (OS, disk, PHP, occ status) | T01 | OPEN |
| T03 | CRITICAL | R | Verify domain renewal: expiry date, auto-renew, lock, contact email, 2FA. ICANN lookup cannot do .io; use registrar login or the .io registry lookup. | Registrar login | OPEN |
| T04 | CRITICAL | R | Take a fresh Phase 0 snapshot (config.php, DB dump, data copy or at minimum data listing) before any change. Old backups exist (T22) but are not current. | T01, T02 | OPEN |
| T05 | CRITICAL | R | Credential and recovery-email inventory for registrar, host, SSH, Nextcloud admin, DB, SMTP, Google accounts (locations only) | Tony | OPEN |
| T06 | CRITICAL | R | Record Nextcloud version, PHP version (web + CLI), DB type/version, data dir, enabled apps, setup warnings | T02 | OPEN |
| T07 | HIGH | R | Pull public DNS record set + WHOIS + TLS cert + crt.sh subdomain list from Tony's machine. DNS set done 2026-10-09; IP owner, WHOIS, cert, crt.sh pending | Tony | PARTLY DONE |
| T08 | HIGH | R | Diagnose 2026-09-03 mail rejection. SPF present, DMARC absent (2026-10-09). Still to do: DKIM check (T23), bounce text, blocklist check of 172.241.164.114 | T23 | OPEN — narrowed |
| T23 | HIGH | R | Query DKIM: `Resolve-DnsName default._domainkey.longviewhub.io -Type TXT` (and check the host panel's Email Deliverability page). GREEN. | Tony | OPEN |
| T24 | HIGH | R | Publish DMARC: `_dmarc.longviewhub.io TXT "v=DMARC1; p=none; rua=mailto:<a mailbox Tony reads>"`. YELLOW (DNS change; reversible by deleting the record). Do after T23 so reports are meaningful. Tighten to p=quarantine later. | T23 | OPEN |
| T09 | HIGH | R | Determine Nextcloud upgrade path (sequential majors), PHP compatibility per step, DB compatibility | T06 | OPEN |
| T10 | HIGH | R | Confirm TLS issuer, expiry, renewal mechanism; cover all live hostnames. crt.sh history captured 2026-10-09; superseded by T26 for the fix. | T07 | PARTLY DONE |
| T25 | HIGH | R | Identify spoke, stratus, nvr1: purpose, A records, where they run, whether public. GREEN (resolve + ask Tony). | Tony | OPEN |
| T26 | CRITICAL | R | Certificates: (a) read the serving cert on longviewhub.io and spoke in a browser; (b) in the hosting panel open SSL/TLS Status and read AutoSSL's log for the wildcard and spoke; (c) repair renewal before 2026-10-11 for spoke and well before 2027-01-03 for the apex. (a)(b) GREEN; (c) YELLOW. | T01 access path | OPEN |
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
| T21 | LATER | F | Mine the 2026-05-09 ChatGPT export in Drive for prior setup notes (≈110 MB JSON; needs local grep, not this session) | Tony's machine | PARKED |

## Active Work Queue (3–7 items)
1. T26 — certificates (spoke expires 2026-10-11). Next: Tony reads the padlock on longviewhub.io and spoke; then panel SSL/TLS Status.
2. T25 — identify spoke / stratus / nvr1. Next: Tony resolves the three names and says what they are.
3. T23 — DKIM query. Next: Tony runs the one-liner.
4. T01 — hosting access path. Next: Tony names the panel URL / SSH availability.
5. T03 — domain renewal state. Next: registrar login readout.
6. T22 — backup media inventory. Next: Tony plugs drives in one at a time.

## Blocker Log
| ID | Blocker | Blocks | Opened | Status |
|----|---------|--------|--------|--------|
| B01 | Session egress proxy blocks DNS, RDAP, crt.sh, and longviewhub.io itself | T07 from this session | 2026-10-03 | OPEN — workaround: run from Tony's machine |
| B02 | No server access from this session | T02, T04, T06 | 2026-10-03 | OPEN — Tony runs commands |
| B03 | Hosting provider unknown | T01 → nearly everything | 2026-10-03 | OPEN |
| B04 | Credential locations unknown | T05 | 2026-10-03 | OPEN |
| B05 | Documentation has no permanent home | Continuity across sessions | 2026-10-03 | CLOSED 2026-10-09 — repo github.com/HarambePi2022-2000P/Longviewhub (D-002) |
