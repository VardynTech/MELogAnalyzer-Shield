# Changelog

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
