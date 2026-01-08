# Thadius

_A Python-driven automation and notification engine featuring secure, scalable, and concurrent job execution with real-world security assurance._

---

## 📈 System Architecture

```mermaid
flowchart TD
    %% Batch Orchestration
    BO[Batch Orchestrator<br>(batch_scan.py via cron or CLI)] -->|Pulls subdomains to scan| DB[(MySQL Database)]
    BO -->|Launches per-target scans| S1[Scanner Scripts]

    %% Scanner Scripts
    subgraph "Scanner Scripts (per target or batch)"
        S1a[Subdub.py<br>Subdomain Enum]
        S1b[Filezer.py<br>File Discovery]
        S1c[Panelz.py<br>Admin Panel Scan]
        S1d[Cveez.py<br>CVE Scan]
        S1e[Cnamer.py<br>Takeover Detection]
        S1f[Hoster2.py<br>Service Enumeration]
        S1g[Miscon.py<br>Misconfig Detection]
    end
    S1 --> S1a
    S1 --> S1b
    S1 --> S1c
    S1 --> S1d
    S1 --> S1e
    S1 --> S1f
    S1 --> S1g

    %% Script-centric Worker Pools
    S1a --"ThreadPoolExecutor (per script)"--> TP1[Worker Pool]
    S1b --"ThreadPoolExecutor (per script)"--> TP2[Worker Pool]
    S1c --"ThreadPoolExecutor (per script)"--> TP3[Worker Pool]
    S1d --"ThreadPoolExecutor (per script)"--> TP4[Worker Pool]
    S1e --"ThreadPoolExecutor (per script)"--> TP5[Worker Pool]
    S1f --"ThreadPoolExecutor (per script)"--> TP6[Worker Pool]
    S1g --"ThreadPoolExecutor (per script)"--> TP7[Worker Pool]

    %% In-memory Data Processing
    TP1 --"File & memory lists"--> DT1[Datatables<br>(in-memory/filelists)]
    TP2 --> DT1
    TP3 --> DT1
    TP4 --> DT1
    TP5 --> DT1
    TP6 --> DT1
    TP7 --> DT1

    %% Central MySQL DB
    S1a -->|Findings| DB
    S1b -->|Findings| DB
    S1c -->|Findings| DB
    S1d -->|Findings| DB
    S1e -->|Findings/Claims| DB
    S1f -->|Findings| DB
    S1g -->|Findings| DB

    %% Slack Notification Utility
    S1a --"Status/Errors"--> SLK[Slack Webhook Utility]
    S1b --"Status/Errors"--> SLK
    S1c --"Status/Errors"--> SLK
    S1d --"Status/Errors"--> SLK
    S1e --"Status/Errors"--> SLK
    S1f --"Status/Errors"--> SLK
    S1g --"Status/Errors"--> SLK

    %% Azure Takeover Automator Callout
    S1e --"Azure Claims"--> AZ[AUTOMATED AZURE TAKEOVER<br>multi-region fallback<br>evidence logging]

    %% Test Automation Callout
    subgraph Test_Automation
      T1[Unit Tests]
      T2[Concurrency Tests]
      T3[Integration Tests]
    end
    T1 -.-> S1a
    T2 -.-> TP1
    T3 -.-> S1e
```

**Key Components:**

- **Job Scheduler:** Handles all cron-like and ad-hoc tasks using `schedule` or `APScheduler`.
- **Worker Pool:** Manages concurrency via Python's `ThreadPoolExecutor`, processing jobs from a thread-safe queue.
- **Datatables & MySQL Storage:**  
  - **Datatables**: In-memory data manipulation (e.g., with Pandas or custom classes).  
  - **MySQL**: Reliable persistent storage for results, job configs, and logs.
- **Slack Integration:** Automated timely notifications using `slack_sdk`.
- **Extensibility:** Designed for additional integrations and scaling.
- **Testing:**  
  - **Unit:** Mocks for DB and Slack.  
  - **Integration/E2E:** Covers full flows across components.  
  - **Threading/Performance:** Stress tests for race conditions and stability.

---

## 🛡️ Security Achievements

Thadius was built and operated with a security-first approach, directly informed by my ongoing offensive security research and real bug bounty impact.

### 🔒 [View my HackerOne Profile →](https://hackerone.com/atoma/hacktivity?type=user)

**Select Bounty Highlights:**
- FanDuel: $1,000
- AT&T: $750
- Upserve: $300

**Vulnerability Experience:**

| CWE | Vulnerability Type                       | Submissions | Bounties | Severity Highlights |
|-----|-----------------------------------------|-------------|----------|--------------------|
| 200 | Information Disclosure                  | 20          | $300     | -                  |
| 287 | Improper Authentication (Generic)       | 6           | $750     | -                  |
| 284 | Improper Access Control (Generic)       | 5           | $100     | 1 Critical         |
| 352 | Cross-Site Request Forgery (CSRF)       | 5           | -        | -                  |
| 79  | Cross-site Scripting (XSS) - Reflected  | 5           | -        | -                  |
| 22  | Path Traversal                          | 4           | -        | 1 Critical         |
| 601 | Open Redirect                           | 4           | -        | -                  |
| 918 | Server-Side Request Forgery (SSRF)      | 4           | -        | -                  |
| 260 | Password in Configuration File          | 2           | $50      | -                  |

**OWASP Top 10 2017 Coverage:**
- A2: Broken Authentication (3)
- A5: Broken Access Control (4)
- A6: Security Misconfiguration (1)
- A7: Cross-Site Scripting (1)

**Bug Bounty Report Examples:**
- Path Traversal (Critical)
- Improper Access Control (Critical)
- Information Disclosure
- Improper Authentication

> _I apply a security mindset and lessons learned from real-world offensive testing to every codebase I own—including Thadius._

---

## 🚦 Testing Methodology

- **Unit Testing:** Isolates core logic, mocks all integrations.
- **Integration/E2E:** Validates workflows with real databases (test), and full notification loops.
- **Concurrency & Race Tests:** Uses stress scenarios to validate thread safety.

---

## 🤖 Technologies

- Python 3.x, threading/pools
- Pandas
- MySQL (with SQLAlchemy or native connector)
- Slack SDK
- pytest, unittest

---

## 🌟 Contact

- **Security, Collaboration, or Consultation:**  
  [atoma on HackerOne](https://hackerone.com/atoma/hacktivity?type=user)

---

## 📜 License

[MIT](LICENSE)
