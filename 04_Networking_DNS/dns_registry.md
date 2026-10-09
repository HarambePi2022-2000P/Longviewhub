# Domain / DNS Registry — longviewhub.io
Updated 2026-10-09 from Tony's `Resolve-DnsName` run (public records, CONFIRMED unless marked).

## Authority
| Item | Value | Status |
|------|-------|--------|
| Authoritative nameservers | ns1.server.plus, ns2.server.plus, ns3.server.plus, ns4.server.plus | CONFIRMED |
| SOA primary / admin | ns1.server.plus / monitor.corp.hostiso.com | CONFIRMED |
| SOA serial | 2026100905 (format YYYYMMDDnn: zone last edited 2026-10-09, 5th edit that day) | CONFIRMED — who/what edited it is UNKNOWN |
| DNS provider | The web host's DNS cluster (server.plus nameservers administered by hostiso.com), not the registrar and not Cloudflare | INFERRED (strong) |
| IP ownership (172.241.164.114) | AS396362 Leaseweb USA, Inc.; range 172.241.164.0/22; New York City; AS type Hosting; ipinfo counts **1 hosted domain** on this IP | CONFIRMED (ipinfo.io, 2026-10-09) |
| Infrastructure reading | HostISO (DNS/admin) operating on Leaseweb USA address space in NYC. One domain on the IP means a dedicated IP: VPS or dedicated server more likely than crowded shared hosting. | INFERRED (medium) |
| Registrar lookup via ICANN | Not possible: ICANN's RDAP tool does not cover .io (a ccTLD). Use the .io registry's own lookup or the registrar login. | CONFIRMED 2026-10-09 |
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
| *.longviewhub.io | wildcard (what it covers in practice is UNKNOWN) | — | Let's Encrypt wildcard certs issued ~every 60 days through 2026-07-06; **none since** | — | expired 2026-10-04 | — | CONFIRMED (crt.sh) |
| spoke.longviewhub.io, www.spoke.longviewhub.io | UNKNOWN purpose | A/CNAME UNKNOWN | certs 2026-03-13, 05-13, 07-13 (Let's Encrypt); none since | — | current cert **expires 2026-10-11** | UNKNOWN | CONFIRMED exists (crt.sh); target UNKNOWN |
| stratus.longviewhub.io, www.stratus.longviewhub.io | UNKNOWN purpose | UNKNOWN | cert 2026-02-04 (Let's Encrypt); later ones not visible in the captured list | — | UNKNOWN | UNKNOWN | CONFIRMED exists (crt.sh); target UNKNOWN |
| nvr1.longviewhub.io | UNKNOWN; name suggests a network video recorder | UNKNOWN | certs 2026-02-07, 05-08, 08-07 (Let's Encrypt) every 90 days | — | current cert expires 2026-11-05 | UNKNOWN | CONFIRMED exists (crt.sh); target UNKNOWN |

Notes
- The three extra SPF IPs are in the same 172.241.0.0/16 block as the server. Reading: the host's outbound mail relays or sibling nodes, put there by the host's default SPF template. INFERRED.
- SPF ends in `~all` (softfail). Acceptable; `-all` is stricter and can come later once DKIM and DMARC are in place.
- No DMARC record at all. Many receiving gateways, government ones included, now reject or quarantine unauthenticated mail from domains without DMARC. This is the leading candidate for the 2026-09-03 rejection (K1).
- Zone serial shows five edits on 2026-10-09. If Tony did not touch DNS that day, this is host automation (cPanel-style hosts rewrite zones for AutoSSL validation and cluster syncs). Confirm with Tony; not an alarm by itself.

## Web server response (HEAD https://longviewhub.io, from Tony's machine, 2026-10-09)
Server: Apache · Upgrade: h2,h2c (HTTP/2 via mod_http2) · Strict-Transport-Security max-age=63072000; includeSubDomains · X-Frame-Options SAMEORIGIN · Content-Type text/html;charset=ISO-8859-1.
Reading: the ISO-8859-1 content type and the absence of Nextcloud's usual headers (X-Robots-Tag, Content-Security-Policy, X-Content-Type-Options) mean this HEAD request most likely received an Apache error page, not the Nextcloud front door. Working hypothesis: a web-application firewall (ModSecurity, common on cPanel hosts) blocked the non-browser client. Not a problem for users in a browser; it does mean automated checks need a browser-like User-Agent. HYPOTHESIS, low priority.

## TLS certificate history (crt.sh, captured 2026-10-09; each cert appears twice in crt.sh as precert + leaf)
| Hostname(s) | Issuer | Not before | Not after | Pattern |
|-------------|--------|------------|-----------|---------|
| longviewhub.io + *.longviewhub.io | Let's Encrypt | 2025-12-31, 2026-03-02, 2026-05-06, 2026-07-06 | 90 days each; last expired 2026-10-04 | ~60-day cadence, **stopped after 2026-07-06** |
| longviewhub.io (apex only) | **GoDaddy DV (TLS Intermediate CA DV R1v1)** | 2026-06-19 | **2027-01-03** | one-off; issued the day after the domain's June 18 renewal date; who obtained it and how it renews is UNKNOWN |
| spoke + www.spoke | Let's Encrypt | 2026-03-13, 05-13, 07-13 | last expires **2026-10-11** | ~60-day cadence, **stopped after 2026-07-13** |
| nvr1 | Let's Encrypt | 2026-02-07, 05-08, 08-07 | last expires 2026-11-05 | 90-day cadence, still alive as of August |
| stratus + www.stratus | Let's Encrypt | 2026-02-04 | 2026-05-05 | later issuances not visible in the capture; may have lapsed or scrolled off |

Readings
- Tony's HEAD request on 2026-10-09 completed without a certificate error, so the apex is currently served by a valid certificate. With the Let's Encrypt wildcard expired on 10-04, that is almost certainly the GoDaddy certificate. INFERRED; confirm by reading the padlock in a browser.
- Wildcard Let's Encrypt certificates require DNS-01 validation, which fits cPanel AutoSSL issuing wildcards when DNS is hosted on the same cPanel cluster (it is). The five zone edits on 2026-10-09 (K5) fit AutoSSL inserting and removing `_acme-challenge` records while retrying a validation that keeps failing. HYPOTHESIS, testable from the hosting panel's SSL/TLS Status page.
- Whatever the cause, automatic renewal for the apex wildcard and for spoke stopped in July 2026. The apex has a hard cliff on **2027-01-03** unless the GoDaddy cert renews or AutoSSL is repaired; spoke has one on **2026-10-11**.
- HSTS is set with includeSubDomains for two years: browsers will refuse any subdomain whose certificate lapses, with no click-through.

## Subdomain strategy
Deferred until crt.sh listing and hosting-panel inventory are in. Do not create or change DNS before then, except the mail-authentication fixes tracked in the backlog (T23, T24), which are YELLOW changes with their own verification steps.
