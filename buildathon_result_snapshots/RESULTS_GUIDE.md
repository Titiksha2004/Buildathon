# Results and figure placement guide

**Source:** `agentic_5g_step6_8_backend_evidence.zip` (exact uploaded ZIP SHA-256 `f780aa9d37ba20f0ea3d6fddb9e97c03b739498a0e7a20bb4e663dbac0f182ab`). The builder checked the SHA-256 of every entry against its manifest. These PNGs were freshly rendered from saved JSON; they are not screenshots of Colab windows. `archived_baseline_comparisons_original.png` was copied byte for byte from the ZIP.

## Suggested results sequence

1. **Urban demand and incomplete evidence** — `urban_capacity_and_allocations.png`, then `urban_chunked_sequence_B01_to_B05.png`. Script: telemetry and receipts arrive at different times. Resource allocation follows the actual capacity; the chart compares all five recorded controllers on six activities.
2. **Rural fluctuating connectivity** — `rural_capacity_and_allocations.png`, then `rural_chunked_sequence_B01_to_B05.png`. Script: agents cannot create connectivity during a zero-link interval; examine holds, resumption and timely payload.
3. **Matched comparison and stress cases** — The other six `*_B01_to_B05.png` figures cover operator revoke and rollback, timeout and permission recovery, stale capacity, matched service pairs and rural fluctuations. Each graph uses all B01–B05 methods and all six activities.
4. **SA and NSA** — `SA_NSA_matched_urban_rural.png` compares B01–B04 in both urban and rural matched architecture cases. Their measured bars coincide, because application inputs and budgets were matched. There is no B05 run for these four cases.
5. **Safety and explainability** — `four_fresh_agent_safety_probes.png` renders actual Context and Security/Policy assessments and guard outcomes. It shows forged urgency, unknown sensitivity, conflicting receipts and a zero link. These four are fresh snapshots, whereas eight full agent traffic workflows are in the earlier nested evidence.
6. **Optional model experiment** — `isolated_candidate_before_after.png` shows an exact-label score improvement from 8/16 to 13/16 on a small authored split. Three explanation labels remain wrong. The candidate was not promoted.

## What to say when discussing results

B05 matched B02 and B04 on the principal timely-payload measure in the displayed recorded scenarios. B01 sometimes delivered more of the urgent clinical call’s payload, but often served the live assessment less well. There is no established across-baseline superiority. The agent contribution demonstrated here is coordination of partial evidence, grounded policy retrieval, explicit review timing and fixed safety enforcement; benchmark throughput superiority remains an open research question.

The lower five-method charts show a *fraction of generated payload delivered before its deadline*, not the fraction of urgent windows meeting a minimum rate. These are separate measures. For exact values, use `all_48_cases_every_available_method.csv`; it includes both, along with holds, completion, throughput and invariant fields.

**Glossary:** B01 fixed service order; B02 urgency ordering with independent profile rules; B03 separate QoS/security then feasible projection; B04 joint allocator without agents; B05 validated agents plus the joint numerical controller and fallback. All methods share fixed protection, permission and resource guards.

## Presentation boundaries

- 48 recorded numerical cases contain B01–B04. Exactly eight have a recorded B05 full traffic run.
- The four named SA/NSA cases contain B01–B04; no B05 architecture-specific result was recorded. Equal architecture bars are expected under matched application assumptions.
- Policy passage selection succeeded in four current adviser questions, but substantive legal interpretation and document completeness require review.
- The archived figure is the only image copied from the ZIP. All other PNGs are accurate renderings of numerical records or final agent packets.
- This is an application-level synthetic experiment, not a radio scheduler or native emergency-call network run.

## Data file provenance

`all_48_cases_every_available_method.csv`: each row corresponds to one scenario, method and request. No missing B05 result has been filled with a simulated value.

### Figure inventory

- `SA_NSA_matched_urban_rural.png`
- `archived_baseline_comparisons_original.png`
- `four_fresh_agent_safety_probes.png`
- `isolated_candidate_before_after.png`
- `rural_capacity_and_allocations.png`
- `rural_capacity_fluctuations_B01_to_B05.png`
- `rural_chunked_sequence_B01_to_B05.png`
- `rural_operator_revoke_rollback_B01_to_B05.png`
- `rural_stale_capacity_permission_change_B01_to_B05.png`
- `urban_capacity_and_allocations.png`
- `urban_chunked_sequence_B01_to_B05.png`
- `urban_matched_pair_window_B01_to_B05.png`
- `urban_operator_revoke_rollback_B01_to_B05.png`
- `urban_timeout_permission_recovery_B01_to_B05.png`
