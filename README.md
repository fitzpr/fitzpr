# Thadius

_A Python-driven automation and notification engine featuring secure, scalable, and concurrent job execution with real-world security assurance._

---

## 📈 System Architecture

```mermaid
flowchart TB
  %% Batch Orchestrator (bright blue for visibility)
  subgraph Batch_Orchestrator
    BO[batch_scan.py<br><b>cron/CLI entry</b>]
  end
  style BO fill:#158aff,color:#fff,stroke:#0055aa,stroke-width:2px

  %% Scanner Scripts (violet-pink for visibility)
  subgraph Scanners
    S1a[Subdub.py<br><i>Subdomain discovery</i>]
    S1b[Filezer.py<br><i>File discovery</i>]
    S1c[Panelz.py<br><i>Admin panel finder</i>]
    S1d[Cveez.py<br><i>Vuln/CVE scan</i>]
    S1e[Cnamer.py<br><i>Takeover checks</i>]
    S1f[Hoster2.py<br><i>Host/service enum</i>]
    S1g[Miscon.py<br><i>Misconfiguration scan</i>]
  end
  style S1a fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1b fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1c fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1d fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1e fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1f fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px
  style S1g fill:#e658ea,color:#fff,stroke:#9800a1,stroke-width:2px

  %% Worker Pools (green for visibility)
  subgraph Worker_Pools
    TP1[ThreadPool<br>10 workers]
    TP2[ThreadPool<br>10 workers]
    TP3[ThreadPool<br>10 workers]
    TP4[ThreadPool<br>15 workers]
    TP5[ThreadPool<br>5 workers]
    TP6[ThreadPool<br>10 workers]
    TP7[ThreadPool<br>8 workers]
  end
  style TP1 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP2 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP3 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP4 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP5 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP6 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px
  style TP7 fill:#2fd05c,color:#fff,stroke:#0e6626,stroke-width:2px

  %% Data / Output (yellow for visibility on dark)
  DT[Datatables<br>Files: .csv / .json]
  style DT fill:#f7f323,color:#000,stroke:#888800,stroke-width:2px

  %% Database (orange for contrast)
  DB[(MySQL Database<br><i>findings, claims</i>)]
  style DB fill:#ff9514,color:#fff,stroke:#b55000,stroke-width:3px

  %% Slack Notifier (cyan)
  SLK[Slack Bot<br><i>reports & alerts</i>]
  style SLK fill:#00eaea,color:#222,stroke:#008888,stroke-width:2px

  %% Azure Takeover (deep purple)
  AZ[Azure Automation<br><i>live takeover evidence</i>]
  style AZ fill:#9c60ff,color:#fff,stroke:#370099,stroke-width:2px

  %% Test Automation Block (gray)
  subgraph Test_Automation
    T1[Unit Tests<br><i>(pytest)</i>]
    T2[Concurrency Tests]
    T3[Integration Tests]
  end
  style T1 fill:#333,color:#fff,stroke:#fff,stroke-width:2px
  style T2 fill:#333,color:#fff,stroke:#fff,stroke-width:2px
  style T3 fill:#333,color:#fff,stroke:#fff,stroke-width:2px

  %% Legend
  Legend["<b>Legend:</b><br>
      Blue: Orchestrator<br>
      Violet: Scanners<br>
      Green: Worker Pools<br>
      Yellow: Temporary/process data<br>
      Orange: Database<br>
      Cyan: Slack notifications<br>
      Purple: Azure/Cloud automation<br>
      Gray: Test/QA Coverage"]
  style Legend fill:#222,color:#fff,stroke:none

  %% Flows
  BO --> S1a
  BO --> S1b
  BO --> S1c
  BO --> S1d
  BO --> S1e
  BO --> S1f
  BO --> S1g

  S1a --> TP1 --> DT
  S1b --> TP2 --> DT
  S1c --> TP3 --> DT
  S1d --> TP4 --> DT
  S1e --> TP5 --> DT
  S1f --> TP6 --> DT
  S1g --> TP7 --> DT

  DT --> DB

  S1a --"Live results"--> DB
  S1b --"Live results"--> DB
  S1c --"Live results"--> DB
  S1d --"CVE findings"--> DB
  S1e --"Takeover/claims"--> DB
  S1f --"Host/service data"--> DB
  S1g --"Misconfig discoveries"--> DB

  S1a --Notify--> SLK
  S1b --Notify--> SLK
  S1c --Notify--> SLK
  S1d --Notify--> SLK
  S1e --Notify--> SLK
  S1f --Notify--> SLK
  S1g --Notify--> SLK

  S1e --"Claim workflow"--> AZ

  %% QA coverage (dashed, indirect)
  T1 -.-> S1a
  T2 -.-> TP1
  T3 -.-> S1e

  %% Legend placement
  Legend --- BO
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
