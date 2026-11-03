# Verification methods, design faults and data-layer safeguards
# যাচাইয়ের পদ্ধতি, ধরা পড়া ভুল ও ডাটাবেসের সুরক্ষা

**বাংলা:** সিস্টেম কীভাবে পরীক্ষা করা হয়েছে, কোন ৭টা নকশার ভুল ধরা পড়েছে আর কীভাবে ঠিক হয়েছে, আর ডাটাবেসের ২৯টা পরীক্ষার ফল।

> Extracted from the paper (Netra AI, 2026). Table and section numbers refer to the paper; citations are omitted here — see the paper for references.

## Verification methods

- **Database behavior tests.** All migrations were applied to PostgreSQL 16 with a shim of the authentication schema; 29 scripted checks exercise every access rule and trigger (Section 3.6).
- **Unit and component tests.** 57 tests (district list, phone normalization, optotype size, scoring and stopping rules, WHO cut-offs, triage bands, hospital ranking, message content, red-flag detection, distance estimate, full vision-test interaction, fault injection).
- **Fault injection.** In the spirit of chaos engineering, an in-memory backend failed each request, at random, either before it reached the server (lost request) or after it was applied (lost response), with probability *p* ∈ {0.05, 0.10, 0.20} per phase. One thousand screenings were retried until saved; afterwards the stored rows and photo objects were compared with what one clean save produces.
- **Load test.** 200,401 synthetic patients, 400,802 eye photographs and ≈50,000 referrals (about ten field days at 1,000 workers) on a container with two virtual CPUs (vCPUs), pgbench as authenticated users with RLS on. Network and API-gateway latency are not included.
- **Audit simulation.** Monte-Carlo study (20,000 replicates per setting) of the bias and precision of the sensitivity estimate for audit rates 5–20% and 300–20,000 screened patients, comparing the stratified estimator with the earlier design in which the audit flag could be overwritten.
- **Growth model.** Load at four growth stages (10 to 10,000 workers, 20 patients per worker per day) was derived from the measured costs: ≈55 KB per eye for the crop and thumbnail, full photographs kept for a 10% sample, 300 working days per year, and a worst-case sync storm in which a whole day's screenings arrive within one hour, compared with the measured save capacity.
- **Hazard analysis.** An ISO 14971-style table linking each hazard to its mechanism and verification test (Appendix A).

## Verification findings

Verification found seven faults; all were fixed before field use (Table 6).

| # | Found by | Fault | Consequence if not fixed | Fix |
|---|---|---|---|---|
| F1 | Load test | Study-identifier default `lpad(n,4)` truncates numbers above 9,999 | 10,000th patient collides with NETRA-1000; every later save fails | sequence-based `next_study_id()` |
| F2 | Load test | RLS role checks evaluated per row | referral queue 4,424 ms; review query 5,298 ms at 200k patients | scalar sub-selects (one evaluation per query): 17 ms / 369 ms; server-side review queue 21 ms |
| F3 | Design review + simulation | A field referral overwrote the reason of an audit row, so the audit draw was lost | field sensitivity overstated by +0.03 to +0.11 (Table 8) | permanent `audit_sampled`/`audit_p`; stratified estimator |
| F4 | Design review | Clinical review queue revealed AI result through priority and order | reference labels not blinded, biasing the measured sensitivity | separate blinded research grading queue; `reviews.blinded` |
| F5 | Design review + workload model | Registered threshold of the earlier model (sensitivity-first, specificity 19%) would flag about 85% of eyes, contradicting the triage assumption | doctor workload close to reviewing every eye | two-tier band (refer ≥ 0.5, review ≥ validation threshold) computed in the database |
| F6 | Fault injection | Random photo names on retry | 2,619–17,185 orphan photo objects per 7,000 needed (p = 0.05–0.20) | deterministic object names; "already exists" treated as success |
| F7 | Browser benchmark | Without a graphics processor, TF.js fell back to its pure-JavaScript CPU back end | median 7.6 s per eye under 6× throttling, violating R3 | WASM back end loaded before the CPU fallback: median 247 ms (31× faster) |
| — | Behavior test | Referral guard trusted a session setting that a direct SQL session could set | worker could bypass the guard outside the API | guard keyed to the executing database role |

: Table 6. Faults found during verification.

## Data-layer safeguards

All 29 database checks passed (Table 7).

| Property | Check | Result |
|---|---|---|
| Triage | refer score → urgent referral visible to worker; review score → routine; client-sent "positive" overridden by registry threshold | pass |
| Upgrade | review → refer upgrades the referral; field referral upgrades an audit row and keeps the draw | pass |
| Audit rate | 87 of 1,000 AI-negative patients sampled at r = 10% (expected 100; 95% range 81–119); invisible to workers | pass |
| Immutability | administrator update of the audit draw silently reverted | pass |
| Guard | worker cannot change type, priority, reason or visibility, also after setting the old bypass flag | pass |
| Blinding | doctor sees no score before labeling and sees it after; grading queue exposes no band, reason or priority | pass |
| Estimator | `audit_sensitivity()` equals an independent computation (0.2051 in a constructed scenario); workers cannot call it | pass |
| Privacy | worker 2 cannot read worker 1's contacts; invalid phone rejected; no names or numbers in the audit log | pass |
| Configuration | audit rate changeable by administrator only | pass |

: Table 7. Database behavior checks (PostgreSQL 16, migrations 0000–0008).
