# Domain / DNS Registry — longviewhub.io
Updated 2026-10-09 from Tony's `Resolve-DnsName` run (public records, CONFIRMED unless marked).

## Authority
| Item | Value | Status |
|------|-------|--------|
| Authoritative nameservers | ns1.server.plus, ns2.server.plus, ns3.server.plus, ns4.server.plus | CONFIRMED |
| SOA primary / admin | ns1.server.plus / monitor.corp.hostiso.com | CONFIRMED |
| SOA serial | 2026100905 (format YYYYMMDDnn: zone last edited 2026-10-09, 5th edit that day) | CONFIRMED — who/what edited it is UNKNOWN |
| DNS provider | The web host's DNS cluster (server.plus nameservers administered by hostiso.com), not the registrar and not Cloudflare | INFERRED (strong) |
| Registrar | eNom/Tucows reseller "qunatum.com" (from renewal notices) | INFERRED — confirm via ICANN lookup |
| Expiry / auto-renew / lock / 2FA | UNKNOWN — registrar login or ICANN lookup | UNKNOWN |

## Records
| Hostname | Service | Type | Value | TTL | TLS | Class | Status |
|----------|---------|------|-------|-----|-----|-------|--------|
| longviewhub.io | Nextcloud + web | A | 172.241.164.114 | 7200 | HSTS 2y incl. subdomains; cert issuer UNKNOWN | public | CONFIRMED |
| longviewhub.io | (IPv6) | AAAA | none | — | — | — | CONFIRMED absent |
| www.longviewhub.io | alias | CNAME | longviewhub.io | 7200 | — | public | CONFIRMED |
| longviewhub.io | mail | MX | longviewhub.io, priority 0 (mail is on the same server) | 7200 | — | public | CONFIRMED |
| longviewhub.io | SPF | TXT | `v=spf1 +a +mx +ip4:172.241.167.65 +ip4:172.241.164.43 +ip4:172.241.167.110 ~all` | 7200 | — | public | CONFIRMED |
| _dmarc.longviewhub.io | DMARC | TXT | **none** | — | — | — | CONFIRMED absent |
| default._domainkey.longviewhub.io | DKIM | TXT | UNKNOWN — not yet queried | — | — | — | UNKNOWN |
| longviewhub.io | CAA | CAA | UNKNOWN (Windows resolver cannot query CAA) | — | — | — | UNKNOWN |
| (other subdomains) | — | — | UNKNOWN — crt.sh listing pending | — | — | — | UNKNOWN |

Notes
- The three extra SPF IPs are in the same 172.241.0.0/16 block as the server. Reading: the host's outbound mail relays or sibling nodes, put there by the host's default SPF template. INFERRED.
- SPF ends in `~all` (softfail). Acceptable; `-all` is stricter and can come later once DKIM and DMARC are in place.
- No DMARC record at all. Many receiving gateways, government ones included, now reject or quarantine unauthenticated mail from domains without DMARC. This is the leading candidate for the 2026-09-03 rejection (K1).
- Zone serial shows five edits on 2026-10-09. If Tony did not touch DNS that day, this is host automation (cPanel-style hosts rewrite zones for AutoSSL validation and cluster syncs). Confirm with Tony; not an alarm by itself.

## Web server response (HEAD https://longviewhub.io, from Tony's machine, 2026-10-09)
Server: Apache · Upgrade: h2,h2c (HTTP/2 via mod_http2) · Strict-Transport-Security max-age=63072000; includeSubDomains · X-Frame-Options SAMEORIGIN · Content-Type text/html;charset=ISO-8859-1.
Reading: the ISO-8859-1 content type and the absence of Nextcloud's usual headers (X-Robots-Tag, Content-Security-Policy, X-Content-Type-Options) mean this HEAD request most likely received an Apache error page, not the Nextcloud front door. Working hypothesis: a web-application firewall (ModSecurity, common on cPanel hosts) blocked the non-browser client. Not a problem for users in a browser; it does mean automated checks need a browser-like User-Agent. HYPOTHESIS, low priority.

## Subdomain strategy
Deferred until crt.sh listing and hosting-panel inventory are in. Do not create or change DNS before then, except the mail-authentication fixes tracked in the backlog (T23, T24), which are YELLOW changes with their own verification steps.
