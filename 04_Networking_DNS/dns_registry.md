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
| *.longviewhub.io | wildcard, serves the apex | — | Let's Encrypt wildcard, renewed ~every 60 days; **current cert issued 2026-09-05, expires 2026-12-04** (read from the browser padlock) | — | valid | — | CONFIRMED (browser, 2026-10-09) |
| spoke.longviewhub.io, www.spoke.longviewhub.io | old project — **RETIRE (D-003)** | A/CNAME UNKNOWN | certs 2026-03-13, 05-13, 07-13 visible in crt.sh (index lags) | — | irrelevant once retired | UNKNOWN | CONFIRMED exists; target UNKNOWN |
| stratus.longviewhub.io, www.stratus.longviewhub.io | old project — **RETIRE (D-003)** | UNKNOWN | cert 2026-02-04 visible in crt.sh | — | irrelevant once retired | UNKNOWN | CONFIRMED exists; target UNKNOWN |
| nvr1.longviewhub.io | old project — **RETIRE (D-003)** | UNKNOWN | certs 2026-02-07, 05-08, 08-07 visible in crt.sh | — | irrelevant once retired | UNKNOWN | CONFIRMED exists; target UNKNOWN |

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
| longviewhub.io + *.longviewhub.io | Let's Encrypt | 2025-12-31, 2026-03-02, 2026-05-06, 2026-07-06, **2026-09-05 (serving; not yet in crt.sh)** | current expires **2026-12-04** | ~60-day cadence, healthy; next renewal expected ~2026-11-04 |
| longviewhub.io (apex only) | GoDaddy DV (TLS Intermediate CA DV R1v1) | 2026-06-19 | 2027-01-03 | **not serving**; one-off; issued the day after the domain's June 18 renewal date; origin UNKNOWN. If the registrar turns out to be GoDaddy, their domain-forwarding feature auto-issues certs like this. |
| spoke + www.spoke | Let's Encrypt | 2026-03-13, 05-13, 07-13 (crt.sh lags) | — | being retired (D-003) |
| nvr1 | Let's Encrypt | 2026-02-07, 05-08, 08-07 (crt.sh lags) | — | being retired (D-003) |
| stratus + www.stratus | Let's Encrypt | 2026-02-04 (crt.sh lags) | — | being retired (D-003) |

Readings (corrected 2026-10-09 after reading the serving certificate in the browser)
- **crt.sh is a lower bound, not the current state.** Its index lagged by more than a month here; the September wildcard was missing. Rule for this project: never declare a renewal broken from crt.sh alone; read the serving certificate first.
- The apex wildcard renews on a ~60-day cadence and is healthy. That cadence, with DNS on the host's cluster, fits cPanel AutoSSL issuing wildcards via DNS-01. INFERRED.
- The five zone edits on 2026-10-09 (K5) are still unexplained. Revised hypothesis: AutoSSL attempting validation for the dead subdomains (spoke, stratus, nvr1) and failing daily. Retiring them should quiet it. Testable from the panel's SSL/TLS Status page.
- The GoDaddy certificate is an unexplained artifact, not a risk. Its date (one day after the domain's renewal anniversary) raises the question of whether the domain moved to GoDaddy in June 2026, which would also explain the missing 2026 renewal notices from the old registrar (K2). Ask Tony; confirm at the registrar.
- HSTS is set with includeSubDomains for two years: any subdomain that stays in DNS must stay on valid HTTPS. Retired names must leave DNS entirely rather than be left pointing at nothing.

## Subdomain strategy
Deferred until crt.sh listing and hosting-panel inventory are in. Do not create or change DNS before then, except the mail-authentication fixes tracked in the backlog (T23, T24), which are YELLOW changes with their own verification steps.
