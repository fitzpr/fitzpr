# Thadius

_A Python-driven automation and notification engine featuring secure, scalable, and concurrent job execution with real-world security assurance._

---

## 📈 System Architecture

```mermaid
flowchart TB
  %% Batch Orchestrator Block
  subgraph 0 [Batch Orchestrator]
    BO["batch_scan.py\n(cron or CLI)"]
  end

  %% Database Block
  subgraph 1 [MySQL Database]
    DB[(MySQL)]
  end

  %% Scanner Scripts Block
  subgraph 2 [Scanner Scripts]
    S1a["Subdub.py"]
    S1b["Filezer.py"]
    S1c["Panelz.py"]
    S1d["Cveez.py"]
    S1e["Cnamer.py"]
    S1f["Hoster2.py"]
    S1g["Miscon.py"]
  end

  %% Worker Pools Block
  subgraph 3 [Worker Pools (per script)]
    TP1["ThreadPool"]
    TP2["ThreadPool"]
    TP3["ThreadPool"]
    TP4["ThreadPool"]
    TP5["ThreadPool"]
    TP6["ThreadPool"]
    TP7["ThreadPool"]
  end

  %% In-memory / Files Block
  DT["Datatables & Files (lists, output)"]

  %% Slack Notifier
  SLK{{"Slack\nNotifier"}}

  %% Azure Takeover Automation
  AZ["Azure Takeover\nAutomation"]

  %% Test Automation Block
  subgraph 4 [Test Automation]
    T1["Unit Tests"]
    T2["Concurrency Tests"]
    T3["Integration Tests"]
  end

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

  S1a --"Results/Status"--> DB
  S1b --"Results/Status"--> DB
  S1c --"Results/Status"--> DB
  S1d --"Results/Status"--> DB
  S1e --"Results/Claims"--> DB
  S1f --"Results/Status"--> DB
  S1g --"Results/Status"--> DB

  S1a --"Notify"--> SLK
  S1b --"Notify"--> SLK
  S1c --"Notify"--> SLK
  S1d --"Notify"--> SLK
  S1e --"Notify"--> SLK
  S1f --"Notify"--> SLK
  S1g --"Notify"--> SLK

  S1e --"Claim"--> AZ

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
