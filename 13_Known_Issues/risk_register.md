# Risk Register
Updated 2026-10-03.

| ID | Risk | Probability | Impact | Mitigation | Status |
|----|------|-------------|--------|------------|--------|
| R1 | Domain loss: Jun 18 annual expiry, history of third-notice renewals, 2026 renewal state unverified | Low–Medium | Critical | Verify at registrar; auto-renew + lock on; record contact email | OPEN |
| R2 | Data loss: only old, unverified backups exist (external HDs; maybe workstation / 2 TB SSD). Currency and completeness unknown; nothing automated. | High | Critical | Inventory the drives (dates, sizes, whether DB dump + config.php + data are all present); fresh Phase 0 snapshot before any change; then 3-2-1 | OPEN — refined 2026-10-06 |
| R3 | Account-recovery chain through longviewhub@gmail.com (lost/recovered May 2026) | Medium | High | Inventory what it recovers; MFA everywhere; password manager | OPEN |
| R4 | Outbound mail rejected by strict gateways (incident 2026-09-03). DMARC confirmed absent 2026-10-09; DKIM unknown. | High | High | Confirm DKIM; publish DMARC p=none with reporting; then tighten. Check IP reputation of 172.241.164.114. | OPEN — root cause narrowed |
| R5 | Unsupported Nextcloud/PHP exposed publicly | Medium | High | Baseline versions; patch path in Phase 2/3 | OPEN |
| R6 | Undocumented single-operator system | Certain | Medium | This documentation set; decide permanent home (D-002) | OPEN |
| R7 | Discovery latency: session cannot reach server or public DNS | Certain | Low–Medium | Batched read-only command blocks for Tony to run | OPEN |
| R8 | **Certificate renewal has stopped.** Apex wildcard (Let's Encrypt) lapsed 2026-10-04; apex now rides a GoDaddy cert with unknown renewal, cliff 2027-01-03. spoke cert expires 2026-10-11. HSTS includeSubDomains means a lapsed subdomain is hard-blocked in browsers. | High | High | Confirm serving cert via browser padlock; open hosting panel SSL/TLS Status; repair AutoSSL or certbot; decide whether GoDaddy cert stays. | OPEN — new 2026-10-09 |
| R9 | Three undocumented hosts under the domain (spoke, stratus, nvr1) with unknown purpose, location and exposure. | Certain | Medium | Identify each; add to registry; decide keep/retire. | OPEN — new 2026-10-09 |

# Known Issues
| ID | Issue | First seen | Evidence | Status |
|----|-------|------------|----------|--------|
| K1 | Mail from johnson.ross@longviewhub.io refused by a California state agency gateway | 2026-09-03 | Gmail thread (Tony re-sent via another address) | OPEN — bounce text needed |
| K2 | No 2026 domain-renewal notices in johnson.ross.a@gmail.com, unlike 2023–2025 | 2026-06 (absence) | Gmail | OPEN — verify at registrar |
| K3 | No DMARC record for longviewhub.io | 2026-10-09 | Resolve-DnsName _dmarc.longviewhub.io TXT → empty | OPEN — fix is T24 |
| K4 | HEAD / from a non-browser client returns an Apache error-style page (ISO-8859-1, no Nextcloud headers). Likely WAF/ModSecurity. Users unaffected. | 2026-10-09 | PowerShell Invoke-WebRequest headers | OPEN — low priority, note for monitoring design |
| K5 | DNS zone serial 2026100905: five zone edits on 2026-10-09, origin unknown | 2026-10-09 | SOA record | OPEN — leading hypothesis: AutoSSL DNS-01 retries for the lapsed wildcard (see K6) |
| K6 | Let's Encrypt renewals stopped after July 2026 for the apex wildcard and for spoke; apex on a GoDaddy cert to 2027-01-03; spoke expires 2026-10-11 | 2026-10-09 | crt.sh capture | OPEN — T26 |
| K7 | ICANN RDAP lookup cannot query .io; registrar/expiry still unverified | 2026-10-09 | lookup.icann.org | OPEN — use .io registry lookup or registrar login |
