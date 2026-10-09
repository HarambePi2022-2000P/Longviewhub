# Risk Register
Updated 2026-10-03.

| ID | Risk | Probability | Impact | Mitigation | Status |
|----|------|-------------|--------|------------|--------|
| R1 | Domain loss: Jun 18 annual expiry, history of third-notice renewals, 2026 renewal state unverified | Low–Medium | Critical | Verify at registrar; auto-renew + lock on; record contact email | OPEN |
| R2 | Data loss: only old, unverified backups exist (external HDs; maybe workstation / 2 TB SSD). Currency and completeness unknown; nothing automated. | High | Critical | Inventory the drives (dates, sizes, whether DB dump + config.php + data are all present); fresh Phase 0 snapshot before any change; then 3-2-1 | OPEN — refined 2026-10-06 |
| R3 | Account-recovery chain through longviewhub@gmail.com (lost/recovered May 2026) | Medium | High | Inventory what it recovers; MFA everywhere; password manager | OPEN |
| R4 | Outbound mail rejected by strict gateways (incident 2026-09-03) | High | High | Fix SPF/DKIM/DMARC; check IP reputation; consider SMTP relay | OPEN |
| R5 | Unsupported Nextcloud/PHP exposed publicly | Medium | High | Baseline versions; patch path in Phase 2/3 | OPEN |
| R6 | Undocumented single-operator system | Certain | Medium | This documentation set; decide permanent home (D-002) | OPEN |
| R7 | Discovery latency: session cannot reach server or public DNS | Certain | Low–Medium | Batched read-only command blocks for Tony to run | OPEN |

# Known Issues
| ID | Issue | First seen | Evidence | Status |
|----|-------|------------|----------|--------|
| K1 | Mail from johnson.ross@longviewhub.io refused by a California state agency gateway | 2026-09-03 | Gmail thread (Tony re-sent via another address) | OPEN — bounce text needed |
| K2 | No 2026 domain-renewal notices in johnson.ross.a@gmail.com, unlike 2023–2025 | 2026-06 (absence) | Gmail | OPEN — verify at registrar |
