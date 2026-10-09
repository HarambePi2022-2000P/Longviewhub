# Risk Register
Updated 2026-10-03.

| ID | Risk | Probability | Impact | Mitigation | Status |
|----|------|-------------|--------|------------|--------|
| R1 | Domain loss. Expiry confirmed 2027-06-18 and transfer lock on (WHOIS 2026-10-09). Residual: auto-renew state, contact email and 2FA at the registrar unknown; history of last-minute manual renewals. | Low | Critical | Registrar login: turn on auto-renew, verify contact email and 2FA; calendar reminder 2027-05-01 regardless. | OPEN — downgraded 2026-10-09 |
| R2 | Data loss: only old, unverified backups exist (external HDs; maybe workstation / 2 TB SSD). Currency and completeness unknown; nothing automated. | High | Critical | Inventory the drives (dates, sizes, whether DB dump + config.php + data are all present); fresh Phase 0 snapshot before any change; then 3-2-1 | OPEN — refined 2026-10-06 |
| R3 | Account-recovery chain through longviewhub@gmail.com (lost/recovered May 2026) | Medium | High | Inventory what it recovers; MFA everywhere; password manager | OPEN |
| R4 | Outbound mail rejected by strict gateways (incident 2026-09-03). DMARC confirmed absent 2026-10-09; DKIM unknown. | High | High | Confirm DKIM; publish DMARC p=none with reporting; then tighten. Check IP reputation of 172.241.164.114. | OPEN — root cause narrowed |
| R5 | Unsupported Nextcloud/PHP exposed publicly | Medium | High | Baseline versions; patch path in Phase 2/3 | OPEN |
| R6 | Undocumented single-operator system | Certain | Medium | This documentation set; decide permanent home (D-002) | OPEN |
| R7 | Discovery latency: session cannot reach server or public DNS | Certain | Low–Medium | Batched read-only command blocks for Tony to run | OPEN |
| R8 | Certificate renewal: apex wildcard renews on schedule (serving cert 2026-09-05 → 2026-12-04, verified in browser). Residual risk: the mechanism is inferred, not confirmed, and nobody is alerted if a renewal fails. | Low | High | Confirm AutoSSL in the panel; add cert-expiry monitoring in Phase 6. | OPEN — downgraded 2026-10-09 after a false alarm from crt.sh lag |
| R9 | Three dead hosts under the domain (spoke, stratus, nvr1): attack surface and AutoSSL noise for nothing. | Certain | Low–Medium | Retire per D-003 (T27), staged: inventory, DNS/panel removal, then data deletion on per-item go-ahead. | OPEN — decided 2026-10-09 |

# Known Issues
| ID | Issue | First seen | Evidence | Status |
|----|-------|------------|----------|--------|
| K1 | Mail from johnson.ross@longviewhub.io refused by a California state agency gateway | 2026-09-03 | Gmail thread (Tony re-sent via another address) | OPEN — bounce text needed |
| K2 | No 2026 domain-renewal notices in johnson.ross.a@gmail.com, unlike 2023–2025. Domain was nonetheless renewed to 2027-06-18. Either renewed early, auto-renew is on, or the notice address changed. | 2026-06 (absence) | Gmail; WHOIS | OPEN — resolve at registrar login (auto-renew + contact email) |
| K8 | Registrar record "Updated 2026-10-09", same day as five DNS zone edits (K5). Cause unknown. | 2026-10-09 | WHOIS | OPEN — ask Tony whether he changed anything that day |
| K3 | No DMARC record for longviewhub.io | 2026-10-09 | Resolve-DnsName _dmarc.longviewhub.io TXT → empty | OPEN — fix is T24 |
| K4 | HEAD / from a non-browser client returns an Apache error-style page (ISO-8859-1, no Nextcloud headers). Likely WAF/ModSecurity. Users unaffected. | 2026-10-09 | PowerShell Invoke-WebRequest headers | OPEN — low priority, note for monitoring design |
| K5 | DNS zone serial 2026100905: five zone edits on 2026-10-09, origin unknown | 2026-10-09 | SOA record | OPEN — revised hypothesis: AutoSSL validation attempts for the dead subdomains; expect it to stop after D-003 |
| K6 | (Withdrawn) Renewal failure inferred from crt.sh was a false alarm: serving wildcard issued 2026-09-05, expires 2026-12-04. Residual oddity: a non-serving GoDaddy cert from 2026-06-19, origin unknown. | 2026-10-09 | browser padlock | CLOSED as false alarm; GoDaddy question folded into T03 |
| K7 | ICANN RDAP lookup cannot query .io; registrar/expiry still unverified | 2026-10-09 | lookup.icann.org | OPEN — use .io registry lookup or registrar login |
