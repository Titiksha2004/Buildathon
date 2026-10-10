# Context-Aware Agentic Orchestration of QoS and Adaptive Security in Resource-Constrained 5G Networks for Health and Education Sector

Team **NetAxis**, RV College of Engineering, for the ITU AI for Good Build-a-thon 5.0.
Team: Dr. Nethravathi K A (leader), Titiksha Siva Priya K, Jaya Sri Jawahar Sripathy, Dr. Nagamani K.

This is a **synthetic, health and education application-level research prototype**. It is not a live 5G network, a radio scheduler, or a certified emergency-communication system.

---

## 1. The problem

Hospitals, clinics, schools, and colleges depend on mobile networks that can become congested. When many people use the same cell at once, for example during a local outbreak, a public examination, or a campus event, demand exceeds the capacity available. Two kinds of service then compete for the same limited capacity:

- **Health services:** an urgent clinical call (for example, a remote consultation or an emergency referral) needs immediate service. A confidential patient file may tolerate some delay, but it must keep strong protection, and it does not gain emergency priority just because it is sensitive.
- **Education services:** a live class or a live assessment needs a steady, timely stream. Routine clinical calls and background traffic can wait.

Existing prioritisation usually looks only at an application label. That is not enough, because:

- An urgent call and a confidential file are both "important" in different ways, and they need different treatment.
- Protection itself costs bandwidth, CPU time, and delay, so a secure transfer may not fit when capacity is low.

The system must therefore decide, together, **how much capacity each flow gets** and **which protection it receives**, without ever removing required protection. Non-priority traffic, such as public photo uploads and entertainment video, is treated as background load in the simulation.

## 2. Novelty

The contribution is the combination below.
1. **Urgency and sensitivity are separate inputs.** A sensitive file does not automatically receive emergency priority, and an emergency does not automatically remove a file's protection.
2. **Joint choice of resources and security.** The allocator evaluates pre-approved protection profiles using their measured costs (overhead, CPU, added delay) and picks a feasible combination with the rates.
3. **Agents propose, fixed code decides.** Language-model agents interpret context and propose actions. A numerical allocator and a rule-based validator decide what can run. A document can never grant permission to act.
4. **Mandatory protection boundary.** Authentication, confidentiality, and integrity controls are never dropped. When capacity is short, the system delays or reduces optional traffic instead.
5. **Grounded policy evidence.** Policy answers must select passages that were actually retrieved from a versioned knowledge base, and the code copies the exact text. Unsupported legal claims are rejected.
6. **Timed, role-scoped information.** Each agent sees only the events meant for its role and only up to the current simulated time. Future events are hidden, and the system is tested against stale, delayed, forged, and conflicting inputs.
7. **Traceability and rollback.** Each decision records its evidence, its explanation, and its outcome. Operator approval can be revoked, with rollback to the last safe configuration.

## 3. Architecture

```
  Scenario events (released over simulated time, per role)
            │
            ▼
  ┌───────────────────────────────────────────────────────┐
  │ Agents (language model, scoped tools only)            │
  │  • Context agent    – purpose and urgency             │
  │  • Security agent   – sensitivity and allowed profiles│
  │  • Resource agent   – free capacity and CPU           │
  │  • Operator (sim.)  – approvals, revoke, rollback     │
  │  Policy adviser searches the knowledge base           │
  └───────────────────────────┬───────────────────────────┘
                              │ proposals ("recipes")
                              ▼
  ┌───────────────────────────────────────────────────────┐
  │ Allocator (fixed code, deterministic)                 │
  │  – reserves minimum floors for every flow             │
  │  – searches combinations of allowed protection        │
  │  – grows rates in urgency order, then scores plans    │
  └───────────────────────────┬───────────────────────────┘
                              ▼
  ┌───────────────────────────────────────────────────────┐
  │ Validator (fixed code, rejects unsafe plans)          │
  │  – covers exactly the eligible requests               │
  │  – versions and expiry match                          │
  │  – no rate above the authorised cap                   │
  │  – no removal of the shared minimum floor             │
  │  – profile allowed and includes required controls     │
  └───────────────────────────┬───────────────────────────┘
                 pass │                 │ fail
                      ▼                 ▼
        Traffic simulator applies plan   Rejected, logged, approved
        and measures outcome             fallback used
```

### Backend modules (embedded in the notebooks)

| Module | Role |
|---|---|
| `step6_scenario_inputs.py` | Builds synthetic scenarios, signed receipts, role-scoped event feeds |
| `step6_input_checks.py` | Regression tests for input integrity and visibility |
| `step62_engine.py` | Time-based traffic model, allocator, validator |
| `step64_traffic.py` | Traffic function with one guarded decision hook |
| `step64_interface.py` | Guarded model selection |
| `benchmark51_models.py` | Local Hugging Face models (Qwen3, Granite, MiniLM embeddings); no paid APIs |
| `step63_vector_kb.py` | Vector search over policy passages |
| `step63_agents.py` | Timed, scoped agent reviews with tool-access control |
| `step64_baselines.py` | Four non-AI comparison methods on identical evidence |
| `step64_scenarios.py` | Temporal interactions and matched service pairs |
| `step64_supervision.py` | Operator approval, revocation, rollback; protection cannot be weakened |
| `step64_agents.py` | Model interface, frozen audit replies, injected timing faults |
| `step63_checks.py`, `step64_checks.py` | Invariant tests (fixtures are not model evidence) |
| `step64_reporting.py` | Coverage and comparison reports |
| `step64_pipeline.py` | Local ITU-T Y.3172-style pipeline boundaries |
| `step65_repair.py` | Targeted security-interface repair |
| `step66_backend.py` | Backend completion additions |

### Methods compared

| ID | Method |
|---|---|
| B01 | Fixed service order (voice, then video, then files) |
| B02 | Urgency ordering with separate protection rules |
| B03 | QoS and security handled separately, then projected onto feasible rates |
| B04 | Joint numerical allocator, no agents |
| B05 | Validated agents plus the joint allocator and fallback (the full system) |

All methods share the same fixed protection, permission, and resource guards.

### Traffic classes

Six requests (R1–R6): urgent clinical call, routine clinical call, confidential clinic file, public photo upload, live assessment, entertainment video.

## 4. Repository contents

| File | What it is |
|---|---|
| `agentic_5g_final_colab.ipynb` | **Start here.** Presentation notebook. Verifies and loads recorded results, then opens an interactive dashboard. Runs on CPU. |
| `agentic_5g_step6_7_focused_repair.ipynb` | First repair round. Fixes four policy answers and runs one isolated training experiment. Needs a T4 GPU. |
| `agentic_5g_step6_8_grounded_repair.ipynb` | Second repair round. Grounded policy answers with time gates and deadlines, stable training, four fresh controller checks. Needs a T4 GPU. |

Each notebook embeds its own backend source as text. Every module's SHA-256 fingerprint is checked before it is loaded, so modified code stops the run.

## 5. How to run

### A. Presentation (CPU, no AI calls)

1. Open `agentic_5g_final_colab.ipynb` in Colab.
2. Choose a standard CPU runtime and click **Run all**.
3. When prompted, upload `agentic_5g_step6_8_backend_evidence.zip`. The notebook checks its SHA-256 and refuses any other file.
4. Use the dashboard: introduction, conditions, timeline, results, agent evidence, policy coverage. Export the table with the CSV button.

The default run does not train or download a model. **Rerun numerical baselines** repeats the non-AI methods on CPU.

### B. Repair round 1 (GPU)

1. Open `agentic_5g_step6_7_focused_repair.ipynb` on a **T4 GPU** runtime.
2. Upload `agentic_5g_step6_6_backend_evidence.zip` (the Step 6.6 results ZIP, not the assets ZIP).
3. Run all cells in order. Expect roughly 15–35 minutes, plus downloads. The estimate is not a measured runtime.

### C. Repair round 2 (GPU)

1. Open `agentic_5g_step6_8_grounded_repair.ipynb` on a **T4 GPU** runtime.
2. Upload the latest Step 6.7 evidence ZIP, `agentic_5g_step6_7_backend_evidence.zip`.
3. Run all cells in order. Expect roughly 15–35 minutes, plus downloads.
4. Download the resulting Step 6.8 evidence ZIP for review.

### Optional: live agent run

The final notebook's last cell adds a button that runs one recorded agent scenario live on a GPU. Nothing runs until you press it. Live results are saved separately and never overwrite recorded evidence.

### Requirements

- Google Colab (CPU for the presentation, T4 GPU for the repair rounds).
- No API keys. All models are public Hugging Face models run locally.
- Pinned versions: `transformers==4.51.3`, `accelerate==1.7.0`, `huggingface-hub==0.30.2`, `lm-format-enforcer==0.11.3`, `pydantic==2.13.5`, `peft==0.15.2`, `pypdf==5.4.0`, `fastapi==0.115.12`.

The evidence ZIPs are **not stored in this repository**. They are the outputs of earlier runs and must be supplied when a notebook asks for them.

## 6. Results (recorded, synthetic)

Source: `all_48_cases_every_available_method.csv` (48 cases × methods × requests).

- **Safety:** zero protection violations and zero unauthorised dispatched bytes across all 1,200 rows.
- **B05 full runs:** 8 of 48 cases. The other 40 have only B01–B04. No missing B05 value was filled in.
- **Matched comparison on the 8 B05 cases (mean timely payload, %):**

| Method | Clinical file | Entertainment | Live assessment | Public photo | Routine | Urgent |
|---|---|---|---|---|---|---|
| B01 | 100.0 | 20.1 | 20.1 | 100.0 | 81.6 | 76.0 |
| B02 | 100.0 | 8.4 | 36.8 | 100.0 | 34.9 | 62.9 |
| B03 | 100.0 | 8.1 | 34.9 | 100.0 | 34.8 | 60.0 |
| B04 | 100.0 | 8.4 | 36.8 | 100.0 | 34.9 | 62.9 |
| B05 | 100.0 | 8.4 | 36.8 | 100.0 | 34.9 | 62.9 |

**Reading the table**
- B05 matches B04 exactly on these measures. The agent layer adds evidence handling and safety, not extra timely delivery in these runs.
- B01 delivers more urgent and routine payload on time, but less live-assessment payload.
- In the rural chunked-sequence case, B01 reaches 66.7% of urgent payload, against 13.3% for B02–B05.
- There is **no demonstrated across-baseline superiority**. Throughput superiority is an open question.

- **SA/NSA:** four matched cases (B01–B04 only). The bars coincide because inputs and budgets were matched.
- **Fresh controller checks:** four snapshot probes (urgent-unknown, forged urgency, conflicting receipts, zero link). The guard accepted plans that allocated 0 Mbps or 0.08 Mbps to the flagged requests. These are snapshot checks, not four new full traffic runs.
- **Policy adviser:** four current questions produced passages that were retrieved and copied exactly. Legal interpretation still needs human review.
- **Isolated classifier candidate:** exact-label score improved from 8/16 to 13/16 on a small authored held-out split. Three explanation labels remain wrong. **Not promoted** to the controller.

## 7. Project Demo
https://drive.google.com/file/d/1Mk5V1PgFgGRr0rLjsOIO-23dceOSPO20/view?usp=sharing

## 8. References

1. AI for Good. Build-a-thon 5.0, Creating innovative agents in 5G. https://aiforgood.itu.int/event/build-a-thon-5-0-creating-innovative-agents-in-5g/
2. ITU-T Y.3172. Architectural framework for machine learning in future networks including IMT-2020. https://www.itu.int/rec/T-REC-Y.3172/en
3. ITU-T Y.3181. Architectural framework for machine learning sandbox in future networks including IMT-2020. https://www.itu.int/rec/T-REC-Y.3181
4. 3GPP TS 23.501 v16.8.0. System architecture for the 5G System. https://www.etsi.org/deliver/etsi_ts/123500_123599/123501/16.08.00_60/ts_123501v160800p.pdf
5. Large Language Models meet Network Slicing Management and Orchestration, 2024. https://arxiv.org/abs/2403.13721
6. Bozorgchenani et al. Novel modeling and optimization for joint Cybersecurity-vs-QoS Intrusion Detection Mechanisms in 5G networks. Computer Networks, 237, 110051, 2023. https://eprints.whiterose.ac.uk/id/eprint/206017/
7. TRAI. Recommendations on Leveraging Artificial Intelligence and Big Data in Telecommunication Sector, 20 July 2023. https://www.trai.gov.in/sites/default/files/2024-09/Recommendation_20072023.pdf
8. ITU. AI Ready: Analysis Towards a Standardized Readiness Framework. https://www.itu.int/epublications/publication/ai-ready-analysis-towards-a-standardized-readiness-framework
9. Government of India. Digital Personal Data Protection Act, 2023. https://www.meity.gov.in/static/uploads/2024/06/2bf1f0e9f04e6fb4f8fef35e82c42aa5.pdf
10. NITI Aayog. Responsible AI for All, Part 1: Principles for Responsible AI, February 2021. https://www.niti.gov.in/sites/default/files/2021-02/Responsible-AI-22022021.pdf
11. Government of India. National Cyber Security Policy, 2013. https://www.meity.gov.in/writereaddata/files/National_cyber_security_policy-2013_0.pdf

## 9. Team

- Dr. Nethravathi K A: team leader (nethravathika@rvce.edu.in)
- Titiksha Siva Priya K (kamanatitikshas.ec23@rvce.edu.in)
- Jaya Sri Jawahar Sripathy (jayasrijawahar.ec23@rvce.edu.in)
- Dr. Nagamani K (nagamanik@rvce.edu.in)

Organisation: RV College of Engineering, Bengaluru.
