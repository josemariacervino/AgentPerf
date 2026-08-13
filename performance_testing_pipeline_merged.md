# Performance Testing Pipeline — Merged Templates (v1)

Refined five-stage template set, built on the engineer's existing company templates with gap-fixes applied. Designed for a solo Senior Performance Test Engineer running a prompt-chained workflow (one chat per requirement, five stages in sequence).

**Design note on stages 1 & 2:** the Ticket captures the *initial, lightweight* request at intake (what you know the moment the requirement arrives). The Information Gathering doc is the *expanded, confirmed* version after the kickoff meeting — same themes, but validated and detailed. The ticket seeds; the gathering confirms and amplifies.

---

## Stage 1 — Ticket / Requirement Intake

### Kickoff questions (top 5 — refined)

Kept to the five that unblock the most context. Merged the original Q1+Q2 (system + dependencies) into one, and folded "who to follow up with" into the intake fields below rather than a standalone question.

1. **What is under test, and does it depend on any third-party or downstream systems?** (app / app family / DB / API — and its integrations)
2. **What is the context or purpose of the test?** (new release, seasonal peak, incident follow-up, migration, SLA validation)
3. **If the component is exposed through multiple front-ends, which one(s) should we focus on?**
4. **What production load multiplier should be targeted?** (e.g. 2x, 3x normal peak)
5. **When does the test need to run?** (if the answer is "ASAP", the request must come from a Manager)

### Work item hierarchy (Azure DevOps)

- **Epic** — `Load and Performance Tests [YEAR]` — groups all effort for the year.
- **Feature** (child of Epic) — one per incoming requirement.
- **Product Backlog Item** (child of Feature) — one per stage of effort:
  - Analysis and Information Gathering
  - Planning and Technical Design
  - Execution and Monitoring
  - Results Analysis and Findings Sharing
- **Task** (child of each PBI) — the granular work under each stage, for effort tracking (est. vs. actual).

### Initial capture (lightweight — confirmed later in Stage 2)

| Item | Description |
| --- | --- |
| Objective of the test | e.g. load validation, regression, scalability under peak |
| Application / flow / API | e.g. Booking Flow, Payment API |
| Environment | e.g. Preprod1, UAT |
| Test type(s) needed | Load / Stress / Soak / Spike / Scalability / Failover |
| Expected load / peak concurrency | e.g. 500 users, ramp 50/min |
| Target SLOs / KPIs | Response time (P90), throughput, acceptable error rate |
| Monitoring tools | Dynatrace, AppDynamics, Prometheus |
| Requested by / on | Requesting person or team / date |
| Target completion date | When results or sign-off are needed |
| Follow-up contact | Who the perf engineer reaches out to with questions |

---

## Stage 2 — Analysis & Information Gathering

> Expanded, confirmed version of the ticket after the kickoff meeting.

### 1. Test Objective & Scope

| Item | Description |
| --- | --- |
| Test objective | What to validate (load validation, regression, peak scalability, bottleneck discovery) |
| Component / feature | What is being tested (Booking Flow, Payment API, ...) |
| Test environment | Where it runs (Preprod, UAT, Staging) |
| Test type(s) | ☐ Load ☐ Stress ☐ Soak ☐ Spike ☐ Scalability ☐ Failover |

### 2. System / SUT Overview  *(new — closes the architecture gap)*

| Item | Description |
| --- | --- |
| High-level architecture | Components the flow touches (front-end, services, DB, queues) |
| Integration points | Upstream / downstream / third-party systems |
| Tech stack | Runtime, framework, DB engine |

### 3. Workload Design

| Item | Description |
| --- | --- |
| Expected load | Peak users / concurrency (e.g. 500 VUs, 18K rpm, 30 TPS) |
| Ramp-up strategy | e.g. 50 VUs every 5 min, steady-state 30 min |
| User journeys / scenarios | Key flows (e.g. Login → Search → Book) |
| Entry / exit criteria | e.g. 95% of logins complete in <2s |
| Dependencies | e.g. Auth service, Payment Gateway, APIs |
| Data requirements | e.g. unique accounts, pre-filled carts |

### 4. Performance Criteria & Metrics

| Item | Description |
| --- | --- |
| NFRs / SLAs | Response times (P90), CPU thresholds, throughput targets |
| Monitoring requirements | Tools and metrics (Dynatrace, logs, Prometheus) |
| Error thresholds | Tolerance for errors / timeouts |
| Known risks | Dependencies, limitations, blockers |

### 5. Stakeholders & Alignment

| Item | Description |
| --- | --- |
| Business stakeholders | Product Owner, Business Analyst |
| Technical stakeholders | Dev Lead, QA Lead, SRE |
| Alignment meeting held? | ☐ Yes ☐ No (notes) |

### 6. Timeline & Ownership

| Item | Description |
| --- | --- |
| Requested by / on | Person or team / date |
| Target completion date | Deadline for analysis or test |
| Notes / comments | Recent prod changes, previous findings |

---

## Stage 3 — Planning & Technical Design

### 1. Test Scope & Strategy

| Item | Description |
| --- | --- |
| Scope description | In scope (APIs, user flows, modules) |
| Out of scope | Explicitly not covered |
| Test objectives | e.g. validate stability under peak, detect memory leaks, benchmark |
| Success criteria | e.g. <5% error rate, 95% of requests under 2s, CPU < 75% |

### 2. Test Scenarios & Load Model

| Item | Description |
| --- | --- |
| User journeys / scenarios | Steps or APIs per use case |
| Scenario descriptions | What each scenario represents |
| Load profile / distribution | % of users per scenario (Search 60%, Book 25%, Cancel 15%) |
| Concurrency & load targets | e.g. 500 concurrent users, 15K req/min |
| Ramp-up plan | e.g. 50 users every 2 min until steady state |
| Test duration | Per phase (ramp-up, steady, ramp-down) |
| Pacing / think time | e.g. 5s between actions |

### 3. Entry & Exit Criteria  *(new — made explicit)*

| Item | Description |
| --- | --- |
| Entry criteria | What must be true to start (env ready, data seeded, scripts reviewed) |
| Exit criteria | What ends the test (all scenarios run, or abort threshold hit) |
| Abort conditions | e.g. error rate > 5%, environment instability |

### 4. Tooling & Script Design

| Item | Description |
| --- | --- |
| Tool(s) used | JMeter, Gatling, LoadRunner |
| Script status | ☐ To be developed ☐ In progress ☐ Completed ☐ Reviewed |
| Scripts per scenario | Link / reference per script |
| Data handling | CSVs, dynamic data, correlation logic |
| Validation logic | Assertions, checkpoints, status-code validation |
| Parameterization needed? | ☐ Yes ☐ No (explain) |

### 5. Environment & Test Setup

| Item | Description |
| --- | --- |
| Test environment | UAT, Pre-Prod, performance cluster |
| Baseline / reference | ☐ Yes ☐ No — reference run to compare against |
| Dependencies / stubs | Any components mocked / stubbed? |
| Monitoring enabled? | ☐ Dynatrace ☐ Logs ☐ Prometheus ☐ New Relic |
| Test data setup needed? | ☐ Yes ☐ No (how data is prepared) |
| Access / permissions confirmed? | ☐ Yes ☐ No |

### 6. Review & Sign-Off

| Item | Description |
| --- | --- |
| Reviewed by | Dev Lead, QA Lead, or Architect |
| Review date | YYYY-MM-DD |
| Action items from review | Script updates, scope clarification |
| Ready for execution? | ☐ Yes ☐ No — if no, list blockers |

---

## Stage 4 — Execution & Monitoring

> Percentile note: report the **same percentile used in Stage 3 success criteria** (default P90) so results are directly comparable. Add P95/P99 columns only as supporting detail.

### 1. Execution Overview

| Item | Description |
| --- | --- |
| Execution window | e.g. 2026-07-17 |
| Test type | Load / Stress / Soak / Spike / Failover |
| Status | ✅ Success / ❌ Failed at [phase/component] |
| SRE / DevOps support | Who was present during the run |
| Environment state | e.g. Pre-Prod (isolated), no other deployments |

### 2. Applied Runtime Configuration (JMeter)

| Item | Description |
| --- | --- |
| JMX repo URL | [Link to Git/Bitbucket] |
| Script version | Commit SHA or tag (e.g. v1.2.4) |
| Virtual users (VUs) | Actual concurrent threads |
| Load generators | e.g. 3 distributed pods / local machine |
| Throughput achieved | e.g. 2,150 RPM (target 2,000) |
| Data source used | e.g. prod_copy_users_v2.csv |

### 3. Infrastructure under Test (SUT) Specs

| Component | Resource Allocation / Limits |
| --- | --- |
| Pod resources | CPU: 1.0 / Memory: 2Gi |
| Autoscaling (HPA) | Min: 4 / Max: 12 (threshold: 70% CPU) |
| Database instance | e.g. RDS Aurora PostgreSQL (db.r5.xlarge) |
| Load generators | e.g. 3 distributed JMeter slaves (K8s pods) |

### 4. Test Run Summary Table

| Run # | Date | P90 Latency | Throughput | Error Rate | POD Count | Dashboard Link |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | | | | | | [View] |
| 2 | | | | | | [View] |
| 3 | | | | | | [View] |

### 5. Observations & Analysis

_Compare against the success criteria defined in Planning._

| Type | Details |
| --- | --- |
| ✅ Successes | |
| ⚠️ Issues | |
| 🧠 Learnings | |
| 🔍 Next steps | |

### 6. Action Items

| Task | Owner | Due Date | Status |
| --- | --- | --- | --- |
| | | | |

---

## Stage 5 — Results Analysis & Findings

`[[_TOC_]]`

### Verdict — Pass / Fail vs. NFRs  *(new — the headline stakeholders read first)*

| NFR / SLO | Target | Result | Verdict |
| --- | --- | --- | --- |
| Response time (P90) | e.g. <2s | | ✅ / ❌ |
| Throughput | e.g. ≥200 req/min | | ✅ / ❌ |
| Error rate | e.g. ≤1% | | ✅ / ❌ |

**Overall:** ☐ Meets requirements ☐ Meets with caveats ☐ Does not meet — retest required

### Test Information

**Objective** — objectives of the test.

| Item | Details |
| --- | --- |
| Date | mm/dd/yyyy |
| Time window | timeframe EST |
| Tool | tool used for execution |
| Monitoring | APM user |
| Application / service | Name (link) |
| Kubernetes workload | name if applicable |
| Namespace | environment name |
| Endpoint(s) monitored | endpoint(s) as bullets |
| Test type | Load / Stress / Smoke / Endurance / Spike |
| Reference test | last test if any |

**Service Instances Information**
- Instance type:
- Physical CPU cores:
- Logical CPU cores:
- Physical memory:

### Test Results

**Summary**

| Scenario | Test | Virtual Users | Throughput | P90 | PODs | CPU | MEM |
| --- | --- | --- | --- | --- | --- | --- | --- |
| 1 | Test 1 | x VU | x req/min | x ms | 1 → 3 | x% | x% |
| 2 | Test 2 | x VU | x req/min | x ms | 3 → 4 | x% | x% |

**Overview Screenshot** — image

#### Scenario 1 — Test 1
**Service Name**
- Brief summary: current execution reached ~X req/min vs. reference of ~Y req/min.

Screenshots: Req/min · P90 · Resources
Observations: ...

#### Scenario 2 — Test 2
**Service Name**
- Brief summary: ...

Screenshots: Req/min · P90 · Resources
Observations: ...

### Bottleneck / Root Cause Analysis  *(new)*

For any failure or degradation: what saturated first (CPU, memory, DB connections, GC, network), the evidence from APM, and the likely root cause.

### Recommendations & Next Steps  *(new)*

- Recommendation 1 (with owner / priority)
- Recommendation 2
- Retest scope, if applicable

### Summary

- Conclusion 1
- Conclusion 2
