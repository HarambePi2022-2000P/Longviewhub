# LongviewHub Service Registry
Updated 2026-10-03. "UNKNOWN — discovery required" is a real value, not a placeholder to be guessed at.

## Nextcloud
- Purpose: files, photos (Android "Photos for Nextcloud" client), sync
- Domain/Subdomain: **https://longviewhub.io** (apex) — CONFIRMED by Tony 2026-10-06
- Server/Host: UNKNOWN — discovery required
- Application / Version: Nextcloud / UNKNOWN
- Runtime: PHP UNKNOWN; web server UNKNOWN
- Database: UNKNOWN type/version/location
- Storage Location: UNKNOWN (data directory)
- Ports: UNKNOWN (443 expected)
- External Dependencies: DNS, TLS cert, DB, cron, SMTP for notifications (UNKNOWN)
- Authentication Method: UNKNOWN (local accounts assumed; MFA UNKNOWN)
- Backup Method: old backups exist on external hard drives (CONFIRMED by Tony 2026-10-06); possibly also on Tony's workstation and/or a 2 TB SSD. Age, contents, completeness (DB + config + data?), and restorability UNKNOWN. No automated backup known.
- Current Status: in use as of 2025-07; current health UNKNOWN
- Security Concerns: version/PHP possibly unsupported; public exposure UNKNOWN
- Upgrade Requirements: UNKNOWN; likely sequential major upgrades
- Owner/Importance: Tony / primary
- Recovery Priority: 1
- Notes: baseline via `occ status`, `occ app:list`, admin Overview page before any change.

## Domain registration — longviewhub.io
- Purpose: identity root for mail and all services
- Registrar: INFERRED eNom/Tucows via reseller "qunatum.com" (notices from name-services.com)
- Expiry: June 18 annually; 2026 state UNKNOWN (no 2026 notices observed; domain still live)
- Auto-renew / Lock / 2FA: UNKNOWN
- Contact email on record: johnson.ross.a@gmail.com received notices 2023–2025; 2026 UNKNOWN
- Nameservers: UNKNOWN
- Backup Method: n/a (export zone file once nameservers known)
- Recovery Priority: 1

## Email — @longviewhub.io
- Purpose: identity mailbox johnson.ross@longviewhub.io (plus possibly others)
- Provider / MX: UNKNOWN
- Client: FairEmail on Android (Pro, 2025-07-08)
- Auth records (SPF/DKIM/DMARC): UNKNOWN
- Current Status: inbound and outbound working 2026-09-23; one outbound rejection by a government gateway 2026-09-03
- Security Concerns: deliverability; whether IMAP/SMTP are TLS-only UNKNOWN
- Recovery Priority: 1

## DNS (authoritative)
- Provider: UNKNOWN (registrar default / host / Cloudflare)
- Records: UNKNOWN — see 04_Networking_DNS/dns_registry.md
- Recovery Priority: 1

## TLS certificates
- Issuer / expiry / renewal mechanism: UNKNOWN
- Hostnames covered: UNKNOWN
- Recovery Priority: 2

## Tailscale tailnet
- Account: johnson.ross.a@gmail.com
- Nodes: UNKNOWN; which (if any) is the server UNKNOWN
- Purpose: remote access (INFERRED)
- Recovery Priority: 2

## Web root — https://longviewhub.io
- Content: Nextcloud itself (CONFIRMED by Tony 2026-10-06). Merged into the Nextcloud entry; no separate web service known.

## Dependency — Google account longviewhub@gmail.com
- Not a hosted service. Holds the Drive connected to this session (ChatGPT export, "Claude" folder). Recovered 2026-05-19 after access loss. Recovery email: johnson.ross.a@gmail.com.
- What it is the recovery/admin address for: UNKNOWN — must be inventoried (T05).

## Explicitly out of scope
- LiPoNarc (GitHub repo, Android battery manager) and its Dropbox App folder. Decision D-001.
