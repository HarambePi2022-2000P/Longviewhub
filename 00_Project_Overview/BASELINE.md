# LongviewHub.io Infrastructure Reboot — Project Baseline

**Baseline date:** 2026-10-03
**Project phase:** Phase 1 — Discover (Phase 0 Preserve runs in parallel the moment server access exists)
**Prepared by:** Claude, acting as LongviewHub.io Project Manager
**Evidence sources used:** connected Gmail (johnson.ross.a@gmail.com), connected Google Drive (longviewhub@gmail.com), connected Dropbox, this session's container. Only infrastructure-relevant facts were extracted; nothing else from those sources is recorded here.
**Evidence sources NOT available this session:** the server itself, public DNS, WHOIS/RDAP, certificate-transparency logs, the site at longviewhub.io. The session's egress proxy blocks all of them. Those lookups must run from Tony's machine (see 10_Runbooks/discovery_checklist.md).

Legend used throughout: **CONFIRMED** (evidence seen) · **INFERRED** (strong inference from evidence) · **HYPOTHESIS** (plausible, untested) · **UNKNOWN — discovery required**.

---

## A. Confirmed Infrastructure

| # | Fact | Evidence | Status |
|---|------|----------|--------|
| A1 | The domain **longviewhub.io** is registered and live. Mail to and from `johnson.ross@longviewhub.io` was flowing on 2026-09-23. | Gmail threads, 2026-09 | CONFIRMED |
| A2 | Domain **expiry anniversary is June 18**, renewed annually. Renewal notices arrive from `donotreply@name-services.com` and direct to `qunatum.com`. Notices were received in 2023, 2024 and 2025, each year through a "THIRD NOTICE" two days before expiry. **No 2026 notices exist in the observed mailbox.** The domain did not lapse (A1). | Gmail, 7 registrar notices 2023–2025 | CONFIRMED |
| A3 | A working **mailbox `johnson.ross@longviewhub.io`** exists. In use since at least 2023-05-10. Used as the identity address for a ChatGPT account, banking alerts, tax e-file and other important correspondence. | Gmail (forwards to/from it), Drive `user.json` | CONFIRMED |
| A4 | Mail client on Android is **FairEmail** (Pro features purchased 2025-07-08). Message formatting from the longviewhub.io address matches FairEmail. | Google Play receipt | CONFIRMED (client) / INFERRED (that it is the client for this mailbox) |
| A5 | **Nextcloud is in use**: "Photos (for Nextcloud)" Android app purchased 2025-07-08. | Google Play receipt | CONFIRMED (use) — location, version, host all UNKNOWN |
| A6 | A **Tailscale tailnet exists** under johnson.ross.a@gmail.com. Account notices 2024-05 → 2025-07 reference "nodes in your tailnet". | Tailscale account mail | CONFIRMED (tailnet exists) — nodes UNKNOWN |
| A7 | A **Google account `longviewhub@gmail.com`** exists with johnson.ross.a@gmail.com as recovery address. It was **lost and recovered on 2026-05-19**; new-sign-in alerts 2026-05-09, 05-19, 06-16. The Drive connected to this session belongs to it. | Google security alerts | CONFIRMED |
| A8 | That Drive holds a **ChatGPT data export** uploaded 2026-05-09 (`GptData/Conversations` 000–004 JSON ≈110 MB, `chat.html` 77 MB, Images, Misc files) and a `Claude` folder. The ChatGPT account was registered to `johnson.ross@longviewhub.io`. **No server, Nextcloud, DNS or hosting documentation exists in Drive.** | Drive search | CONFIRMED |
| A9 | **Dropbox** contains nothing LongviewHub- or Nextcloud-related. (It is used by the unrelated LiPoNarc battery app via an App folder.) | Dropbox search | CONFIRMED |
| A10 | **Email deliverability incident 2026-09-03:** a message from `johnson.ross@longviewhub.io` to a California state agency address was not accepted; Tony re-sent to a second address. The rejection reason is not in the observed mailbox (the bounce went to the longviewhub.io mailbox). | Gmail thread 2026-09-03 | CONFIRMED (incident) — cause UNKNOWN |
| A11 | **No hosting, VPS, cPanel, or server-vendor correspondence** exists in johnson.ross.a@gmail.com for the last four years. No hosting payment receipts either. | Gmail searches | CONFIRMED (absence) |
| A13 | **Nextcloud is served at the apex, https://longviewhub.io.** | Tony, 2026-10-06 | CONFIRMED |
| A14 | **Old Nextcloud backups exist on external hard drives**; possibly also on Tony's workstation and a 2 TB SSD. Age, contents and restorability unknown. | Tony, 2026-10-06 | CONFIRMED (existence) / UNKNOWN (currency) |
| A12 | The GitHub repo this session launched from (**LiPoNarc**, Android battery manager) is **out of scope**. Nothing LongviewHub-related will be committed there. | Tony, 2026-10-03 | CONFIRMED / Decision D-001 |

## B. Suspected Infrastructure

| # | Item | Basis | Confidence |
|---|------|-------|-----------|
| B1 | **Registrar is eNom/Tucows via a reseller branded "qunatum.com".** `name-services.com` is the notification domain eNom resellers use. | A2 | INFERRED (medium-high) |
| B2 | **Hosting correspondence goes to `johnson.ross@longviewhub.io` or `longviewhub@gmail.com`**, which is why none appears in the observed mailbox. Alternative: the server is self-hosted (home) behind Tailscale and there is no hosting vendor at all. | A11, A6 | HYPOTHESIS — two competing readings, both open |
| B3 | **Nextcloud runs on a PHP + MySQL/MariaDB web host**, plausibly cPanel-style shared hosting or a small VPS, with the longviewhub.io mailboxes on the same account. The project brief's own vocabulary (cPanel, PHP versions, SFTP) points this way. | Brief framing, A5, A3 | HYPOTHESIS |
| B4 | **Mail is hosted on the web host, not on Google Workspace.** If Workspace carried the domain, a separate plain `longviewhub@gmail.com` account would be redundant. | A3, A7 | INFERRED (medium) |
| B5 | **Domain renewal is manual and last-minute**, or the registrar contact email changed in 2026. Three consecutive years of "THIRD NOTICE" argues against auto-renew having been on. | A2 | INFERRED (medium) |
| B6 | **SPF / DKIM / DMARC are missing or misaligned, or the sending IP has poor reputation.** State and federal mail gateways enforce these; a plain rejection of a legitimate message fits that pattern. | A10 | HYPOTHESIS (leading) |
| B7 | **Nextcloud version is behind current and will need a sequential major-version upgrade path.** An install from 2023–2024 that has not been actively maintained would be 2–4 majors behind late-2026 releases. | A5, timeline | HYPOTHESIS |
| B8 | **No verified backups exist.** Nothing in any reachable source suggests a backup regime. | Absence of evidence | HYPOTHESIS (treat as true until disproven) |
| B9 | **A Tailscale node is either the server itself or a home machine used to reach it.** | A6 | HYPOTHESIS |

## C. Unknowns (discovery required)

**Hosting / server**
- Hosting provider, account owner email, plan type (shared / VPS / dedicated / home).
- Operating system and version. Control panel (cPanel/WHM, Plesk, none).
- Access paths: SSH (user, key or password, root or not), SFTP/FTP, control-panel login.
- CPU/RAM/disk, current disk usage, inode usage (shared hosting).
- What else is hosted: other sites, subdomains, apps, cron jobs, databases.

**DNS / domain**
- Registrar identity (confirm B1), current expiry date, auto-renew state, registrar lock, registrant/admin contact email, 2FA on registrar account.
- Authoritative nameservers (registrar default, host, or Cloudflare).
- Full record set: A/AAAA, MX, TXT (SPF), DKIM selectors, DMARC, CAA, all subdomains.

**Mail**
- Mail provider for the domain (host-based IMAP, Workspace, other).
- Whether a catch-all exists. Spam/blocklist status of the sending IP. Bounce text from 2026-09-03.

**Nextcloud** (every item below is UNKNOWN)
- Install path, version, operational state, PHP version (web and CLI), DB type/version/location, data directory, config.php sanity, installed/enabled/disabled apps, background-job mode and cron, file permissions, storage used, external storage, user accounts, current errors, log location and size, backup state, upgrade path.

**TLS**
- Certificate issuer, expiry, renewal mechanism (AutoSSL, certbot, none), which hostnames are covered.

**Backups**
- Any host-side snapshots or backup feature. Any manual copies. Any off-site copy. Has anything ever been restored.

**Remote access / security**
- Tailscale nodes and which is the server. Open ports. Admin UIs exposed publicly. MFA on Nextcloud, control panel, registrar, Google accounts.

**Credentials**
- Where registrar, hosting, SSH, Nextcloud admin, DB, and SMTP credentials are kept. Which account is the recovery email for each.

## D. Immediate Risks

| ID | Risk | Probability | Impact | Mitigation | Status |
|----|------|-------------|--------|------------|--------|
| R1 | **Domain loss.** Annual Jun 18 expiry, history of last-minute renewal, 2026 notices absent from the observed mailbox, renewal mechanism unverified. Losing the domain kills mail, Nextcloud, and the identity address used for important correspondence. | Low–Medium | Critical | Verify expiry, auto-renew, lock, contact email at the registrar this week. Record in DNS registry. | OPEN |
| R2 | **Data loss with no backup.** Any upgrade, host failure, or disk fill could destroy Nextcloud data and DB. | Unknown (assume High) | Critical | Phase 0: establish whether any backup exists; if not, take a one-time config.php + DB dump + data copy before any change. | OPEN |
| R3 | **Account-recovery chain.** `longviewhub@gmail.com` was lost and recovered in May 2026. If it is the recovery or admin address for the registrar, host, or Nextcloud, a repeat means lockout. | Medium | High | Inventory which account recovers which service; put MFA and a password manager under all of them. | OPEN |
| R4 | **Outbound mail deliverability.** A government gateway refused mail from the domain in Sep 2026. Likely SPF/DKIM/DMARC gaps or IP reputation. | High (recurring) | High | Pull DNS TXT records and the bounce text; fix auth records; consider relaying through a reputable SMTP service. | OPEN |
| R5 | **Unsupported software exposed publicly.** Nextcloud and PHP versions unknown; if old and internet-facing, exploitable. | Medium | High | Baseline versions first; patch path decided in Phase 2/3. | OPEN |
| R6 | **Undocumented single-operator system.** All knowledge is in Tony's memory and scattered accounts. | Certain | Medium | This documentation set; decide its permanent home (D-002). | OPEN |
| R7 | **Discovery latency.** This session cannot see the server or public DNS; every fact needs Tony to run a command and paste. Slows Phase 1. | Certain | Low–Medium | Batch commands into one short block per step; use read-only commands only. | OPEN |

## E. Discovery Checklist

Shortest sequence to a reliable map. Full command blocks with explanations are in `10_Runbooks/discovery_checklist.md`. All steps are GREEN (read-only).

1. **Public records from Tony's machine (10 min).** DNS record set, WHOIS, TLS cert, HTTP headers, certificate-transparency subdomain list. Unblocks the DNS/mail/TLS map and usually reveals the hosting provider from the IP's owner.
2. **Registrar login.** Expiry date, auto-renew, lock, contact email, nameservers, 2FA. Closes R1.
3. **Hosting control panel (or confirm self-hosted).** Provider, plan, OS, PHP versions available, disk, mail accounts, databases, SSH availability, built-in backups.
4. **Shell baseline on the server.** OS, disk, PHP CLI version, locate `occ`, `occ status`, app list, config pointers (data dir, DB), cron, log size. Paste with secrets redacted.
5. **Nextcloud admin UI.** Settings → Administration → Overview (version, setup warnings), System, Logging, Apps.
6. **Backups.** What exists, where, how old, ever restored.
7. **Tailscale admin console.** Node list; identify the server.
8. **Credential inventory.** Where each credential lives and which email recovers it. Locations only, never values.

## F. Initial Service Registry

Full table in `03_Service_Registry/service_registry.md`. Summary:

| Service | Purpose | Host | Status | Recovery priority |
|---------|---------|------|--------|-------------------|
| Nextcloud | Files, photos, sync | UNKNOWN | In use (2025); health UNKNOWN | 1 |
| Domain registration (longviewhub.io) | Identity root | Registrar (eNom/Tucows reseller, INFERRED) | Live; renewal state UNKNOWN | 1 |
| Email (@longviewhub.io) | Identity mailbox | UNKNOWN provider | Working inbound/outbound; deliverability problem | 1 |
| DNS (authoritative) | Resolution for all of the above | UNKNOWN | Working | 1 |
| TLS certificates | HTTPS for Nextcloud/web | UNKNOWN | UNKNOWN | 2 |
| Tailscale tailnet | Remote access | Tailscale SaaS | Exists; nodes UNKNOWN | 2 |
| Web root at longviewhub.io | UNKNOWN (site? redirect? placeholder?) | UNKNOWN | UNKNOWN | 3 |
| Google account longviewhub@gmail.com | Identity / Drive / possible recovery address | Google | Recovered May 2026 | Dependency, not a service |

## G. Master Backlog

Full backlog in `00_Project_Overview/backlog.md`. Headline split:

**Recovery (core reboot)**
- BLOCKER: identify hosting provider and access path · get read-only shell or panel baseline.
- CRITICAL: verify domain renewal state · establish any backup, take Phase 0 snapshot · inventory credentials and recovery emails · pull Nextcloud/PHP versions.
- HIGH: fix mail authentication (SPF/DKIM/DMARC) · determine Nextcloud upgrade path · confirm TLS renewal mechanism · map Tailscale nodes.
- NORMAL: service registry completion · DNS registry · runbooks for restore and upgrade.

**Modernization (after stabilize)**
- Backup architecture to 3-2-1 with restore test · monitoring for disk/cert/cron/backup · decide stay/upgrade/migrate for the hosting platform · subdomain strategy.

**Future / LATER**
- Containerization, reverse proxy, SSO, infra-as-code. Parked until the system is stable, backed up, and documented.

## H. Immediate Next Actions (max five)

| # | Action | Class | Why | Output I need |
|---|--------|-------|-----|---------------|
| 1 | Run **Step 1** of the discovery checklist from your machine and paste the output. | GREEN | Gives the DNS/mail/TLS map and usually the hosting provider. | Raw command output |
| 2 | Log into the **registrar**; read off expiry date, auto-renew, lock, contact email, nameservers. Turn on auto-renew and lock if off. | GREEN read / YELLOW if you flip auto-renew | Closes R1, the only risk that can take everything down at once. | Five values, no passwords |
| 3 | Tell me **where Nextcloud lives**: provider name, plan type, and how you get in (panel URL, SSH yes/no). | GREEN | Clears the BLOCKER; shapes every later command. | Provider, plan, access method |
| 4 | Run **Step 4** (shell baseline) if SSH exists, or **Step 5** (admin UI) if not. Redact secrets. | GREEN | Nextcloud version, PHP, DB, data dir, errors. Decides repair vs. upgrade path. | Pasted output |
| 5 | Answer one question: **does any backup of Nextcloud exist today, anywhere?** | GREEN | If no, the first change we make is a Phase 0 snapshot, before anything else. | Yes/no + where |

**Documentation home (D-002, decided 2026-10-04):** private GitHub repo **Longviewhub**. This file set lives there from now on; `00_Project_Overview/STATUS.md` is the entry point for every resumed session.
