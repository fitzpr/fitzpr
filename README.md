# Rob Fitzpatrick

Security researcher based in Dublin. I build automated tooling for bug bounty hunting — continuous subdomain discovery, vulnerability scanning, and AI-driven triage at scale.

---

## Thadius

[github.com/fitzpr/thadius](https://github.com/fitzpr/thadius)

A full-stack bug bounty automation platform I have been building and running in production. Key parts of the architecture:

**Intelligence layer** — runs daily, sweeping security researcher blogs, Twitter/Nitter RSS feeds, Reddit r/netsec, HackerOne Hacktivity, and Hacker News Security. Suggested nuclei tags from the sweep are automatically injected into the next scan run so new techniques get tested the same day they are published.

**Discovery** — subdomain enumeration via subfinder, amass, chaos, and crt.sh against 215 bug bounty programs. Weekly scope sync from arkadiyt/bounty-targets-data; newly added domains are queued for immediate scanning.

**Scanning** — 12 nuclei-based scanners orchestrated by a 4-tier priority scheduler: subdomain takeover, CVE detection, vulnerability scanning, misconfigurations, sensitive file exposure, admin panel detection, JS endpoint discovery (katana), GitHub Actions misconfigs, Codespaces/DevContainer escapes, OAuth bypass vectors. Tech-aware: the detected stack (MongoDB, Jenkins, Spring Boot, etc.) is queried per host before each scan and matching nuclei tags are injected automatically.

**AI validation** — every finding passes through a three-tier gate before being written to the database or triggering a Slack alert. The validator receives the full nuclei output including raw HTTP request and response, then decides: confident false positive (discarded), uncertain (database only), or confirmed real (alert). The same gate applies to auto-generated CVE templates — a template is only written to disk if AI confirms it would realistically fire against the actual live estate.

**Current scale** — ~560k subdomains tracked, ~23k live, 215 root domains across HackerOne, Bugcrowd, Intigriti, and YesWeHack.

---

## Other work

- Security automation tooling (Python, Go, Bash)
- CTF competitions
- Vulnerability research