# Thadius

_A Python-driven automation and notification engine featuring secure, scalable, and concurrent job execution with real-world security assurance._

---

## 📈 System Architecture

```mermaid
flowchart TD
    %% SYSTEM GROUPS
    subgraph Scheduler_Layer
        A1[Job Scheduler (APScheduler)]
        A1 -->|Enqueue Jobs| Q1[Job Queue]
    end

    subgraph Worker_and_Execution_Layer
        Q1 --> P1[Worker Pool]
        P1 -.-> T1[Thread Control]
        P1 -->|Parallel Tasks| W1[Job Worker]
        W1 -->|Process Data| DT[Datatables Memory]
        T1 --> P1
    end

    subgraph Persistence_and_Data_Layer
        DT -->|Batch Ops| DB[MySQL Database]
        DB -- Read Write --> DT
    end

    subgraph Integrations
        W1 -- Notify --> SLK[Slack Bot]
        W1 -- API Call --> EXT[API Integrations]
    end

    %% TEST AUTOMATION
    subgraph Test_Automation
      TA1[Unit Tests]
      TA2[Integration Tests]
      TA3[Threading Tests]
    end
    TA1 -.-> A1
    TA2 -.-> W1
    TA2 -.-> DB
    TA3 -.-> P1

    %% STYLES
    style Q1 fill:#b9e3fa,stroke:#333,stroke-width:2px
    style P1 fill:#b4d5ff,stroke:#333,stroke-width:2px
    style W1 fill:#d2e7c5,stroke:#333,stroke-width:2px
    style DT fill:#ffe4b3,stroke:#333,stroke-width:2px
    style DB fill:#fdc,stroke:#333,stroke-width:2px
    style SLK fill:#f9f,stroke:#333,stroke-width:2px
    style EXT fill:#e5e5e5,stroke:#333,stroke-width:2px
    style T1 fill:#f7fafc,stroke:#333,stroke-width:1px
    style TA1 fill:#fffbe6,stroke:#888,stroke-dasharray: 5 5;
    style TA2 fill:#fffbe6,stroke:#888,stroke-dasharray: 5 5;
    style TA3 fill:#fffbe6,stroke:#888,stroke-dasharray: 5 5;
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
