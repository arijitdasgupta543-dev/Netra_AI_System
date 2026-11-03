# Architecture Decision Records (ADRs), updated November 2026
# গুরুত্বপূর্ণ নকশা-সিদ্ধান্তের রেকর্ড (নভেম্বর ২০২৬ পর্যন্ত)

Each record states the context, the decision, the alternatives considered, the consequence, and when to revisit it. ADR-1 to ADR-8 come from the first design document (v0.1, October 2026); ADR-9 onward were added after verification.

**বাংলা:** প্রতিটা বড় সিদ্ধান্তের জন্য লেখা আছে: কেন সিদ্ধান্তটা দরকার হলো, কী বেছে নেওয়া হলো, আর কী বিকল্প ছিল। আরও আছে এর ফলে কী লাভ বা খরচ হলো, আর কখন আবার ভাবতে হবে। ১–৮ নম্বর প্রথম নকশা থেকে নেওয়া; ৯ থেকে বাকিগুলো যাচাইয়ের পরে যোগ হয়েছে।

| # | Decision | Alternatives | Reason / consequence | Revisit when |
|---|---|---|---|---|
| ADR-1 | Installable web application (PWA) for the pilot | Flutter / native Android | No app store, instant updates, one code base; less camera control | Camera focus or torch control is insufficient on real phones |
| ADR-2 | PostgreSQL with row-level security (Supabase) as the backend | Firebase, custom server | SQL export for research; access rules enforced in the database, not the interface | More than 1,000 workers or data-residency rules |
| ADR-3 | Identifiers generated on the phone + offline queue | Server-generated IDs, sync library | Retries are idempotent without extra infrastructure | Offline editing of existing records is needed |
| ADR-4 | Pupil crop stored for every eye; full photograph for a 10% sample | Store every full photograph | Privacy, about 7× less storage, weaker background shortcut | Researchers need full photographs for a new task |
| ADR-5 | On-device MobileNetV2 (TF.js in the app, TFLite ready) | Cloud inference | Works offline; no image leaves the phone for scoring | A larger model is clearly better and the network is reliable |
| ADR-6 | AI score hidden from the doctor until the label is saved | Show the score | Reference labels stay independent of the AI | Never during validation |
| ADR-7 | Manual iris-circle editor alongside automatic detection | Automatic only | Automatic detection fails on dense cataract and poor light | Automatic detection is acceptable in ≥ 95% of field photos |
| ADR-8 | Model files versioned and registered in `model_runs` | Unversioned files | Every score is traceable to a model version and threshold | Commercial deployment |
| ADR-9 | **Two-tier triage band computed by a database trigger** (refer ≥ 0.5; review ≥ validation threshold) | Single sensitivity-first threshold in the client | A single low threshold flagged about 85% of eyes (fault F5); a trigger cannot be bypassed by a stale client | Doctor capacity or field prevalence changes |
| ADR-10 | **Random audit of AI-negative patients with a permanent draw** (`audit_sampled`, `audit_p`) and a stratified Horvitz–Thompson estimator | Audit flag stored in the referral reason | The old design lost the draw when a field referral arrived and overstated sensitivity by 3–11 points (fault F3) | Audit rate or referral rules change |
| ADR-11 | **Separate blinded research grading queue** | One clinical queue for everything | Queue order and priority revealed the AI result (fault F4) | Never during validation |
| ADR-12 | **Referral guard keyed to the executing database role** | Session setting checked by the guard | A direct SQL session could set the flag and bypass the guard | Role model changes |
| ADR-13 | **RLS role checks as scalar sub-selects (evaluated once per query)**; server-side review queue | Per-row function calls | Referral queue 4,424 ms → 17 ms at 200,401 patients (fault F2) | Query plans change after major upgrades |
| ADR-14 | **Sequence-based study identifiers** | `lpad(n,4)` default | The 10,000th patient would have collided with NETRA-1000 (fault F1) | Never |
| ADR-15 | **Deterministic photo object names; "already exists" treated as success** | Random names per upload attempt | Random names left 2,619–17,185 orphan objects under faults (fault F6) | Storage provider changes semantics |
| ADR-16 | **Jittered synchronization** (start 0–30 s after reconnect, retry every 45–75 s) | Fixed 60 s retry | Avoids a sync storm when many phones reconnect after a camp day | Measured server load at national scale |
| ADR-17 | **WebAssembly (WASM SIMD) inference back end before the CPU fallback** | TF.js default (WebGL, then pure-JavaScript CPU) | Without a GPU the CPU back end took 7.6 s per eye at 6× throttling; WASM took 247 ms with identical decisions (fault F7) | A native client replaces the web app |
| ADR-18 | **Conversion parity as a release gate** (TF.js and TFLite must reproduce every reference decision on the locked test split) | Assume conversion is lossless | Published apps lost up to 30 points of accuracy after conversion | Never |
