# MELogAnalyzer Shield by VardynTech

**Stop brute-force attacks on your MailEnable server.**
Shield reads your MailEnable SMTP, POP and IMAP logs, finds who is guessing passwords or hammering unknown accounts, and blocks them in Windows Firewall - automatically, or only after you approve.

Free for everyone running MailEnable. No licence key, no install, no cloud account, no telemetry.

[Download the latest release](https://github.com/VardynTech/MELogAnalyzer-Shield/releases/latest) · [Buy the dev a coffee](https://buymeacoffee.com/vardyntech) · [vardyntech.com](https://vardyntech.com)

![Attackers tab](screenshots/01-attackers.png)

---

## What it does

- **Finds the attackers.** Failed logins per IP, with the accounts and domains each IP tried, the HELO names it used, its country, and flags such as `MULTI-DOMAIN`, `RANDOM-HELO`, `TEMP-DENIED`, `HAS-GOOD-LOGINS`.
- **Blocks them in Windows Firewall.** Single IPs or whole /24 ranges, in inbound block rules it creates and manages itself. A dry run is on by default, so nothing is changed until you turn it off.
- **Protects real users.** Home-country addresses, IPs that also logged in successfully, whitelisted IPs, your private ranges and this server's own addresses are never auto-blocked.
- **Auto-block, your way.** Off, ask-first (a review queue) or fully automatic. Separate limits for SMTP, POP and IMAP if you want them, and you can switch protection off per service.
- **Catches slow attackers.** A reputation rule blocks an IP whose AbuseIPDB score is high even when its failures are hours apart.
- **Finds anything in the logs.** The Log search tab looks for any text (a HELO name, account, domain, IP) in the raw logs, flagged or not, and shows the whole session of the IP you select. Search the recent days, every day that has a log file, or one chosen day from a list of the log days that really exist on the server. SMTP rows get a **Result** (EHLO refused, Login failed, Rejected...) and a Time column, and **Only EHLO refused** lists every connection your MailEnable *EHLO strings to block* list refused, with the EHLO name used and the entry that caught it.
- **Looks up any IP.** The IP lookup tab shows AbuseIPDB's details (with your free key, or open the AbuseIPDB page in your browser without one), free DNS blocklist results (Spamhaus, SpamCop, UCEPROTECT, DroneBL, PSBL) with reverse DNS, and everything your own SMTP, POP and IMAP logs say about it. Blocklist status also appears in the details of the IP you select on the Attackers tab.
- **Makes the most of MailEnable's HELO block box.** The block-list helper checks a wildcard pattern before you add it, cleans duplicates out of your current list and suggests the patterns that cover the most attackers within the box's length limit.
- **Keeps working when you log off.** One button installs a background task (every 5 minutes) that applies your auto-block rules.
- **Shows that blocks work.** The Blocked tab shows whether a blocked address still reached MailEnable, and how many attempts Windows Firewall dropped (needs the firewall's drop log, which Shield can switch on for you).
- **Reports.** Compromised-account hints, most targeted domains and accounts, countries and networks, daily summary, block effectiveness and repeat offenders. Export to HTML or CSV, or email the report on a schedule.

![By account](screenshots/02-by-account.png)

## Tabs

| Tab | What it is for |
|---|---|
| Attackers (by IP) | Every IP with failed logins, details, AbuseIPDB score, block / whitelist |
| By account | Pick a targeted account, see and block every IP that attacked it |
| By range (/24) & history | Ranges that keep coming back day after day |
| Temp-denied by MailEnable | IPs MailEnable itself already refused |
| Auto-block queue | Proposals waiting for your OK |
| Blocked | What is blocked, why and when; unblock, unblock + whitelist, export |
| Blocked attempts | Connection attempts Windows Firewall dropped from blocked addresses |
| Blocked HELO | The HELO names used by blocked IPs, to spot what they have in common; **Block-list helper** to build a MailEnable EHLO block list that fits |
| Log search | Find any text (a HELO name, account, domain, IP) in the raw log lines, flagged or not, with a Result and Time column; **Only EHLO refused** lists the connections your EHLO block list refused; block IPs from the results, export |
| IP lookup | One IP: AbuseIPDB details, DNS blocklists, reverse DNS and every log line of that IP; block or whitelist it |
| Trend | Failed logins per hour or per day, per account |
| Reports | Six ready-made reports with charts, export and email |

![Trend](screenshots/04-trend.png)

![Countries report](screenshots/05-countries-report.png)

## Fitting your HELO block list into MailEnable

MailEnable can drop a connection by its HELO / EHLO name (SMTP properties, *EHLO strings to block*, `*` is a wildcard), but the box accepts only about 2048 characters in total. Attackers often use whole families of names (`wsip-*.cox.net`, `[2*.*` for address-literal HELOs), so a few good wildcards beat hundreds of exact names. Shield helps you choose them.

![HELO block-list helper](screenshots/06-helo-helper.png)

1. Open the **Blocked HELO** tab and click **Block-list helper...**. Choose how many days of history to learn from (30 by default, up to 365) and click **Read logs**. HELO names change from day to day, so a longer history gives better suggestions. The first read of a long history can take a few minutes; finished days are cached. IPs that failed to log in at least the number of times you set count as attackers too, not only the IPs already blocked.
2. Paste your current MailEnable list into box 1, or click **Load from registry** to read it. Shield only reads; it never changes MailEnable.
3. **Check a pattern** before adding it. Shield reads your SMTP logs and shows how many HELO names the pattern matches, how many blocked IPs and other IPs used them, and warns if any matching IP logged in successfully or is whitelisted. Use this to avoid blocking real mail servers.
4. Set the character budget (2048 unless your MailEnable allows more) and click **Suggest**. Shield removes duplicates and entries that another entry already covers, then adds the patterns that catch the most blocked IPs per character. It never suggests a pattern that matches a good login.
5. Click **Copy new list** and paste it into MailEnable's *EHLO strings to block* box.

Tips: keep **Max other IPs matched** at 0 until you trust the result. Random one-off names cannot be covered by a wildcard; the Windows Firewall block remains the main defence and HELO blocking is an extra layer.

## Quick start

1. Download the zip from the [Releases page](https://github.com/VardynTech/MELogAnalyzer-Shield/releases/latest) and unzip it on the mail server (a permanent folder, not Downloads or Temp).
2. Run `MELogAnalyzer-Shield-by-VardynTech.exe` and accept the administrator prompt (needed to change firewall rules).
3. Pick the MailEnable **Logging** folder, the one that contains `SMTP`, `POP` and `IMAP`.
4. Choose a service and a period. The first load of a large folder takes a while; finished days are cached.
5. Leave **DRY RUN** on until you are happy with what it would block, then turn it off in Settings.
6. Optional: add your free [AbuseIPDB](https://www.abuseipdb.com/register) API key for reputation scores, set up the email report, and click **Run in background** to keep protecting the server after you log off.

## Requirements

- Windows Server 2008 / Vista or later for firewall rules (on older Windows it still reads logs and exports lists, but cannot add rules). Tested on Windows Server 2008 R2 and 2012, including blocking, the background task and email reports.
- .NET Framework 4.0 or later, which is built into Windows Server 2008 R2 and later.
- Read access to the MailEnable log folder, and administrator rights for blocking.
- IPv4 only.
- Internet access is optional. It is used only for AbuseIPDB lookups and reports, the email report, and the DNS blocklist checks (DNS queries only), when you use them.

## Privacy

- Everything runs on your server. Logs are read locally and never uploaded.
- There is no telemetry and no phone-home.
- AbuseIPDB lookups send only an IP address. Reporting to AbuseIPDB is **off by default**; when you turn it on it sends the IP, category 18 (brute force), a general comment and the time of the last attempt. Never account names, domains, mailboxes, counts or mail content.
- The DNS blocklist check (Spamhaus, SpamCop and similar) is an ordinary DNS query that contains the IP address being looked up, sent through your server's DNS resolver to the blocklist provider, and cached for 6 hours. It runs for the IP you select, never in bulk. You can switch it off or change the lists in Settings.
- Your AbuseIPDB key and email password are stored encrypted for this machine.

## Safety notes

- Dry run is on by default.
- Every action (block, unblock, export, report) is written to an action log, with a "Remove ALL rules" panic button.
- Shield adds only block rules of its own (default name prefix `Vardyn-MEBlock`) and never touches your other firewall rules.
- Review the home-country, whitelist and "successful logins" protections in Settings before switching to Automatic.

## Support

Questions, bugs, ideas: open an [issue](https://github.com/VardynTech/MELogAnalyzer-Shield/issues). Please attach the exact message and, if you can, a screenshot with addresses and account names hidden.

If Shield saved your server some pain, [buy the dev a coffee](https://buymeacoffee.com/vardyntech).

Also from VardynTech: [MEMailTrace](https://github.com/VardynTech/MEMailTrace) - free log search and mail trace for MailEnable: see why a mail was refused, deleted or relayed.

## Licence and trademarks

Free to use, including on commercial servers. See [LICENSE.txt](LICENSE.txt). The program is provided as is, without warranty.

MailEnable, Windows, AbuseIPDB and all other product names are trademarks or copyrights of their respective companies. This program is independent and is not made or endorsed by them.

(c) 2026 VardynTech. All rights reserved.

