# Rob Fitzpatrick

Security researcher based in Dublin. I build automated tooling for bug bounty hunting — continuous subdomain discovery, vulnerability scanning, and AI-driven triage at scale.

---

## Thadius

[github.com/fitzpr/thadius](https://github.com/fitzpr/thadius)

A full-stack bug bounty automation platform running continuously in production. Key parts of the architecture:

**Intelligence layer** —  runs weekly across 7 sources: nuclei-templates PRs (unmerged templates), CISA KEV, NVD filtered by EPSS exploit probability (≥0.3), researcher blogs, Exploit-DB, and HackerOne Hacktivity. New nuclei templates are generated per finding category and routed automatically to the relevant scanner. Every H1 disclosure is stored in a local bounty knowledge base used for RAG-augmented AI validation.

**Discovery** — subdomain enumeration via subfinder, amass, chaos, and crt.sh across 864 bug bounty programs. Weekly scope sync from arkadiyt/bounty-targets-data; newly added domains are queued for immediate scanning.

**Scanning** — 11 nuclei-based scanners orchestrated by a 4-tier priority scheduler: subdomain takeover (with automated Azure/AWS/Surge claim scripts and GitHub Pages verification), CVE detection, vulnerability scanning, misconfigurations, sensitive file exposure, admin panel detection, JS endpoint discovery via katana, GitHub Actions misconfigs (pull_request_target, workflow_call, OIDC, artifact poisoning), Codespaces/DevContainer escapes, OAuth bypass vectors.

**AI validation** — every finding passes through a two-model gate before being written to the database or triggering a Slack alert.  runs first with scanner-specific evaluation criteria (separate rules for CVEs, subdomain takeovers, GitHub Actions). Uncertain findings (confidence 0.4–0.7) are escalated to  with the prompt augmented by similar past H1 reports retrieved from the local bounty knowledge base — accepted and rejected precedents with payouts inform the confidence score. Findings with consensus confidence ≥ 0.7 trigger Slack alerts.

**Subdomain takeover automation** — cnamer.py detects dangling CNAMEs then attempts automated claim: Azure App Service/Blob/Traffic Manager via service principal, AWS S3/ElasticBeanstalk via boto3, Surge.sh via CLI. GitHub Pages CNAMEs are verified (account existence check) and returned with manual claim instructions since GitHub has no account creation API.

**Current scale** — ~694k subdomains tracked, ~617k live, 864 root domains across HackerOne, Bugcrowd, Intigriti, and YesWeHack.

---

## Other work

- Security automation tooling (Python, Bash)
- CTF competitions
- Vulnerability research
