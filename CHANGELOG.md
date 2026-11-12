# Changelog

## v1.3

- New **Spray report** (By account tab): is it a distributed spray, which real accounts are hit, can they reach the lockout count, did any attacking IP log in successfully.
- **IP lookup** accepts wildcard ranges: `47.236.*.*` (/16), `47.236.1.*` (/24), `47.*.*.*` or `47.236`. A range shows totals and the busiest addresses from your logs and every log line in it; **Block this IP** blocks the whole range. AbuseIPDB and DNS blocklists stay per address.
- **Log search** matches IP patterns such as `47.236.*.*` against whole addresses only (never inside `147.236.1.1`).

## v1.2

- New **IP lookup** tab: type one IP and see, side by side, AbuseIPDB's details (confidence, reports, last report, ISP, usage type, Tor, recent reports; uses your API key), free DNS blocklist results (Spamhaus ZEN, SpamCop, UCEPROTECT-1, DroneBL, PSBL) with reverse DNS, and what your own SMTP / POP / IMAP logs say about it, plus every log line of that IP. Block or whitelist it from the tab. Stop button, and an **Open in AbuseIPDB** button that needs no key.
- Blocklist status and reverse DNS now also appear in the details of the IP you select on the Attackers and By account tabs (cached 6 hours, one DNS query per list, never in bulk). Settings: switch it off or change the lists.
- **Log search** and **IP lookup** can search the recent days (a number you set), every day that has a log file, or one day picked from a list of the log days that really exist on the server.
- Log search: selecting a result shows every line of that IP in the same log (the whole session).
- Log search: new **Result** column for SMTP rows (EHLO refused, Login failed (535), Rejected, Temp refused), **HELO / EHLO name** and **Blocked by** columns, and a **Time** column. **Only EHLO refused** lists the connections MailEnable refused at EHLO/HELO with the EHLO name and the entry of your block list that caught it; **Summary** groups the refused names and the busiest IPs. Click a header to sort.
- The About box links to MEMailTrace, VardynTech's free mail trace tool for MailEnable.
- New **Block-list helper** on the Blocked HELO tab, for MailEnable's *EHLO strings to block* box (about 2048 characters):
  - learns from a history you choose (30 days by default, up to 365), because attackers change HELO names from day to day; finished days are cached;
  - checks a wildcard pattern against your SMTP logs before you add it (matched HELO names, blocked IPs, other IPs, warning if a matching IP logged in successfully or is whitelisted);
  - removes duplicates and entries that another entry already covers;
  - suggests the patterns that cover the most blocked IPs per character, within a character budget you set, and copies the new list for you to paste;
  - can read your current list from the registry (read only).

## v1.1

- New **Log search** tab: find any text (HELO name, account, domain, IP) in the raw SMTP / POP / IMAP log lines, whether or not they are failed logins. Selecting a result shows every line of that IP in the same log, so you can read the whole session. Optional regex, Debug logs and all-services search; block the IPs of selected results or export them.
- Reports: "VardynTech" in the report header is now a link; exported and emailed single reports no longer carry a number in the title.
- Blocked tab details show country and AbuseIPDB score for POP / IMAP / SMTP entries.
- Trend tab no longer selects the first row by default.
- Reports tab scrolls back to the top when you switch reports.
- Block-effectiveness chart sorted by dropped count.

## v1.0 - first public release

First public release.

**Download:** `MELogAnalyzer-Shield-by-VardynTech-v1.zip` (one exe plus its config file).
Unzip to a permanent folder on your mail server and run it as administrator.

## Highlights
- Reads MailEnable SMTP, POP and IMAP logs and lists every IP with failed logins: accounts and domains tried, HELO names, country, flags.
- Blocks IPs and /24 ranges in Windows Firewall. Dry run is on by default.
- Auto-block modes: off, ask first (review queue), automatic. Separate limits and on/off per service.
- Reputation rule using AbuseIPDB scores, to catch slow attackers (needs your free API key).
- Background task so protection continues after you log off.
- Home-country, whitelist and "successful logins" protection, so real users are not blocked.
- Blocked tab with reasons, unblock, unblock + whitelist and export; Blocked attempts (from the firewall drop log) and Blocked HELO tabs.
- Reports with charts: compromised-account hints, targeted domains and accounts, countries and networks, daily summary, block effectiveness, repeat offenders. Export to HTML / CSV, or email on a schedule.
- Follows the server's time zone; the live view re-reads today's logs every 3 seconds and rolls over to the new day at midnight by itself.

## Notes
- Windows Server 2008 / Vista or later needed for firewall rules. IPv4 only.
- Optional AbuseIPDB reporting is off by default and sends only the IP, a general comment and the time.
- Not published as source. See LICENSE.txt.

## Known limitations
- Tested on Windows Server 2008 R2 and 2012 (log reading, firewall blocking, background task and email reports). Please report anything unusual in the log wording of other MailEnable versions.
- The background task cannot queue home-country proposals for review; those wait until you open the program.
