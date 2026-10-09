# LongviewHub Service Registry
Updated 2026-10-03. "UNKNOWN — discovery required" is a real value, not a placeholder to be guessed at.

## Nextcloud
- Purpose: files, photos (Android "Photos for Nextcloud" client), sync
- Domain/Subdomain: **https://longviewhub.io** (apex) — CONFIRMED by Tony 2026-10-06
- Server/Host: 172.241.164.114 (Leaseweb USA address space, AS396362, New York City; one domain on the IP). Apache with HTTP/2. Operator INFERRED: HostISO (nameservers ns1–4.server.plus, SOA admin monitor.corp.hostiso.com) running on Leaseweb infrastructure. Plan type INFERRED VPS or dedicated (dedicated IP); confirm via Tony's billing or panel.
- Application / Version: Nextcloud / UNKNOWN
- Runtime: PHP UNKNOWN; web server Apache (CONFIRMED 2026-10-09)
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
- Provider / MX: self-hosted on the same server as Nextcloud (MX → longviewhub.io, 172.241.164.114). CONFIRMED 2026-10-09. Mail software UNKNOWN (Exim if cPanel).
- Client: FairEmail on Android (Pro, 2025-07-08)
- Auth records: SPF present (`+a +mx` + three host IPs, `~all`); **DMARC absent**; DKIM UNKNOWN. CONFIRMED 2026-10-09
- Current Status: inbound and outbound working 2026-09-23; one outbound rejection by a government gateway 2026-09-03
- Security Concerns: deliverability; whether IMAP/SMTP are TLS-only UNKNOWN
- Recovery Priority: 1

## DNS (authoritative)
- Provider: the web host's DNS cluster — ns1–ns4.server.plus, administered by hostiso.com (CONFIRMED 2026-10-09). Edited through the hosting panel, INFERRED.
- Records: see 04_Networking_DNS/dns_registry.md (populated 2026-10-09)
- Recovery Priority: 1

## TLS certificates
- Apex: Let's Encrypt wildcard `*.longviewhub.io`, issued 2026-09-05, expires 2026-12-04, renewing on a ~60-day cadence (CONFIRMED in browser 2026-10-09). A non-serving GoDaddy DV cert (2026-06-19 → 2027-01-03) also exists; origin UNKNOWN. HSTS 2 years with includeSubDomains.
- Renewal mechanism: INFERRED cPanel AutoSSL (DNS-01 for the wildcard); confirm from the panel.
- Recovery Priority: 2
- Recovery Priority: 2

## Tailscale tailnet
- Account: johnson.ross.a@gmail.com
- Nodes: UNKNOWN; which (if any) is the server UNKNOWN
- Purpose: remote access (INFERRED)
- Recovery Priority: 2

## Web root — https://longviewhub.io
- Content: Nextcloud itself (CONFIRMED by Tony 2026-10-06). Merged into the Nextcloud entry; no separate web service known.

## spoke.longviewhub.io — RETIRE (D-003)
- Old project, no longer worked on (Tony, 2026-10-09). Where it points and whether its directory/database holds anything: UNKNOWN — inventory before deletion (T27).
- Recovery Priority: none

## stratus.longviewhub.io — RETIRE (D-003)
- Old project, no longer worked on (Tony, 2026-10-09). Inventory before deletion (T27).
- Recovery Priority: none

## nvr1.longviewhub.io — RETIRE (D-003)
- Old project, no longer worked on (Tony, 2026-10-09). Name suggests a video recorder; if recordings exist anywhere, decide archive vs discard before deletion (T27).
- Recovery Priority: none

## Dependency — Google account longviewhub@gmail.com
- Not a hosted service. Holds the Drive connected to this session (ChatGPT export, "Claude" folder). Recovered 2026-05-19 after access loss. Recovery email: johnson.ross.a@gmail.com.
- What it is the recovery/admin address for: UNKNOWN — must be inventoried (T05).

## Explicitly out of scope
- LiPoNarc (GitHub repo, Android battery manager) and its Dropbox App folder. Decision D-001.
