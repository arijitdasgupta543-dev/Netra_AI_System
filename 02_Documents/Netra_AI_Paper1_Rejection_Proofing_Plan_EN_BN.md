# Netra AI — Paper 1: How to avoid a desk rejection
### Lessons from the JBHI decision on MediSathi (JBHI-06595-2026) applied to the Netra AI system-design paper
*Prepared 27 Oct 2026 · Part A English (for professors / co-authors) · Part B বাংলা (আপনার জন্য)*

---

# PART A — ENGLISH

## A0. Verdict in one paragraph

If Paper 1 (draft v0.1, 25 Oct 2026) were sent to JBHI today, it would very likely be desk-rejected for the **same two reasons** as MediSathi: *methods rigor* and *evaluation & evidence*. The engineering work is real, but the draft (i) evaluates the model on 34 web images whose test split was already used in the v1 → v3 design iteration, (ii) uses web labels that no ophthalmologist adjudicated and iris circles annotated with AI assistance, (iii) has no clinician co-author, and (iv) uses words such as "safe" and "safety mechanisms" for properties that were verified in software but not clinically. A careful re-read also found **three internal methodological flaws** (A2) that an expert reviewer would catch. All of them can be fixed, and most fixes need no patient data.

## A1. Map: what JBHI criticised in MediSathi → the same risk in Netra AI → fix

| JBHI criticism (MediSathi) | Same risk in Netra AI Paper 1 | Fix |
|---|---|---|
| Accuracy from only 72 researcher-generated queries | Accuracy from only 34 web images | Re-frame AUC as *pipeline verification*, not accuracy; add repeated splits + an external set; report CIs everywhere |
| Test set informed later training changes (not independent) | The same test split was used for v1 (AUC 0.927) and then for the crop design of v3 | Declare this openly; lock a **new** final test set that was never looked at; pre-specify the analysis |
| Data lack independent clinical adjudication | Web labels not checked by an ophthalmologist ("some labels appear wrong"); 188 circles re-marked with AI assistance | Ophthalmologist re-labels all 226 images; two humans re-mark circles; report Cohen's κ and circle IoU |
| Comparison did not isolate representation / classifier / corpus | Crop vs full image was isolated (good), but only one seed, one split, one backbone | One-factor-at-a-time ablation table, 5 seeds × repeated splits, mean ± SD |
| Safety mechanisms not clinically validated; contraindications incomplete | Triage rules, red-flag rules and assistant knowledge base were written by the developer, not reviewed by a clinician | Clinician review of every rule and KB entry, signed off and versioned; clinician-written question test set for the assistant |
| Pilot of 7 patients, no physician confirmation, no outcomes | No users or patients at all | Either claim nothing clinical, or add a small formative evaluation with ethics approval |
| No ethics determination / consent | "No human participants" — but face images from the web: provenance and licences not documented | Dataset provenance + licence table; ethics exemption letter if possible; approval number for any user study |
| "Prototype development rather than validated intervention" | Same danger | Call the paper what it is: **design and pre-clinical technical verification**; let Paper 2 carry clinical claims |

## A2. Three methodological flaws found in the current design (fix before submission)

**Flaw 1 — The deployed threshold contradicts the triage argument.**
The registered threshold 0.0691 (validation rule, sensitivity ≥ 0.95) gave test specificity 3/16 = 19%. At that operating point about **84–87% of eyes would be flagged positive** (prevalence 15–30%), i.e. ≈34,000 doctor reviews per day out of 40,000 eyes. The paper's capacity estimate assumes a 25% positive rate (≈11,500 reviews/day). At threshold 0.5 (16/18, 15/16) the positive rate would be 19–31% (≈7,500–12,400 reviews/day).
*Fix:* report workload at the actual operating point; justify the threshold with a cost argument (missed cataract vs doctor time); or register two thresholds ("refer" vs "review"). Do not tune it on the test set.

**Flaw 2 — The random-audit estimator is biased by the current database logic.**
`field_referral()` and the AI trigger *upgrade* an audit row (reason `random_audit` → `low_vision` / `ai_suspected`). After the upgrade, nothing records that the patient was randomly sampled. Low-vision patients are exactly the AI-negatives most likely to have cataract, so they silently leave the audit stratum. In a simulation (prevalence 0.30, true sensitivity 0.85, specificity 0.80, 5% audit, 50% of false negatives and 15% of true negatives with low vision), counting only rows still marked `random_audit` **overestimated sensitivity by about +0.07** (0.85 → ≈0.92), at both 2,000 and 20,000 patients.
*Fix:* add a permanent `audit_sampled boolean` set at sampling time and never changed, and use a **stratified (Horvitz–Thompson) estimator**: AI-negatives referred for low vision are reviewed with probability 1; other AI-negatives with probability r. With this fix the simulated bias was ≤ 0.01.
*Precision:* with r = 5% the 95% range of the sensitivity estimate was ±0.18 at 300 patients, ±0.08 at 2,000 and ±0.03 at 20,000 → use **r = 10–20% during the pilot** and lower it at scale (the NHS diabetic eye screening programme likewise sends a minimum 10% of normal grades to second-level grading and allows a higher rate during training or when there are concerns).

**Flaw 3 — Blinding is broken by the review queue.**
The predictions table is blinded, but `review_queue()` returns `priority` (`urgent` = AI positive, `audit` = AI negative) and orders by it. A doctor therefore knows the AI result before labelling, so labels used to *measure* the AI are not blind — especially in the audit sample, where knowing "AI said negative" may lead to under-calling.
*Fix:* separate two queues: a **clinical queue** (unblinded, for patient care and referrals) and a **research grading queue** (random order, reason/priority hidden, mixes positives and audit negatives). Report which labels came from which queue. If both cannot be built, state "partial blinding" as a limitation.

## A3. Strategic decisions (largest effect on acceptance)

1. **Paper type and claims.** Re-title and re-frame: *"Netra AI: design and pre-clinical technical verification of an offline smartphone cataract screening and referral system for rural Bangladesh."* Every claim must be backed by a test in the paper; remove "safe", "efficient", "accurate" unless measured.
2. **Clinical co-author.** One ophthalmologist co-author who (a) re-labels the image set, (b) reviews triage/red-flag rules and the knowledge base, (c) co-signs the hazard analysis. This is the single biggest credibility gain and links to the Lions Eye Hospital approach.
3. **Venue.** JBHI is not the right first target for a paper without patient data. Better fits for a design + verification paper: JMIR Formative Research (formative/early-stage digital-health work), BMC Medical Informatics and Decision Making (technical advance), IEEE Access, or a conference/workshop such as IEEE EMBC, ACM COMPASS or MICCAI-OMIA. Check each scope page before submitting. Keep JBHI / JMIR mHealth for Paper 2 with field data.
4. **Optional but powerful:** add a small formative study (5–8 health workers, task time + System Usability Scale, after ethics approval) — it moves the paper from "prototype" to "formative evaluation".

## A4. New analyses to add (most need no patient data)

| # | Analysis | Answers the editor's concern | Needs |
|---|---|---|---|
| E1 | Monte-Carlo simulation of the audit estimator (bias, CI coverage, required audit rate vs. prevalence, sensitivity, sample size); stratified estimator after the Flaw-2 fix | Statistical support for G2 | Nothing (done in part, see A2) |
| E2 | Doctor-workload model at the real operating points + sensitivity analysis (positive rate, prevalence, review time) | Data sufficiency of the capacity claim | Nothing |
| E3 | Hazard analysis (ISO 14971-style FMEA: hazard → cause → mechanism → verification test → residual risk) + requirement → mechanism → test **traceability matrix** | "Safety" claims become verifiable | Clinician co-sign |
| E4 | Fault-injection test of offline sync (≥ 1,000 random network drops / replays) → lost = 0, duplicates = 0 | Evidence for offline-first claim | Nothing |
| E5 | Model rigor: (a) declare test reuse; (b) new locked test set; (c) 5 seeds × repeated stratified splits, mean ± SD; (d) Wilson CIs for sens/spec, DeLong for AUC; (e) calibration (Brier, reliability plot); (f) one-factor ablation: crop vs full, backbone (MobileNetV2 / V3 / EfficientNet-Lite), input size, augmentation | Methods rigor; isolating effects | Compute only |
| E6 | Label provenance: ophthalmologist re-label of all 226 images; 2 human circle annotators; κ and IoU; remove or flag disputed images | Clinical adjudication; data provenance | Ophthalmologist + 1 helper |
| E7 | External domain-shift stress test on a public anterior-segment set from another source (e.g., SLID slit-lamp images, CC-BY article, GitHub; CatScreen under licence agreement). Report as *stress test*, not accuracy, because slit-lamp ≠ phone photos | Generalisation evidence | Download + licence |
| E8 | Real-device benchmarks on 3 low-cost Android phones: model load time, inference ms, peak RAM, battery per 20 screenings, offline cache size | Evidence for "runs on cheap phones" | Your / friends' phones |
| E9 | Physical verification of the vision test: measure rendered optotype size with a ruler/caliper after card calibration on 3–5 phones (% error); distance estimate against a printed iris target at known distances | Evidence for vision test & distance claims without patients | Ruler, printed target |
| E10 | Assistant evaluation on a **frozen, clinician-written** question set (~100 questions incl. red-flag cases): red-flag recall, refusal to diagnose/prescribe, factual correctness graded by an independent clinician | Exactly the MediSathi failure (researcher-generated queries) avoided | Clinician |
| E11 | Load test against a hosted staging backend (network included), p50/p95 latency | Realism of scalability claim | Separate staging project |

## A5. Section-by-section changes to the draft

| Section | Change |
|---|---|
| Title | Add "pre-clinical technical verification"; remove "safety mechanisms" or say "safeguards" |
| Abstract | State paper type; give simulation and verification results; state test-set reuse and web-image limits in Results, not only in Limitations; conclusions limited to verified properties |
| 1 Introduction | Replace contribution bullets by **research questions** (RQ1 can the safeguards be enforced in the data layer? RQ2 does it scale to 1,000 workers? RQ3 is the audit estimator unbiased and precise enough? RQ4 is the on-device pipeline faithful and robust?) |
| 2 Related work | Add: NHS diabetic eye screening 10% QA sample of normals (prior art for random audit — our novelty is enforcing it in the data layer with a stratified estimator and blinding); the 2025 multimodal smartphone cataract preprint (image + acuity + pinhole, 2,956 training patients, prospective on-device validation on 118 patients — a direct comparator, also multimodal like Netra); SLID and CatScreen datasets; IMDRF SaMD risk categorisation |
| 3 Requirements | Show where each requirement came from (WHO, literature, field constraints, clinician input); add acceptance criteria per requirement |
| NEW 4 Evaluation design | Before any result: questions, metrics, pre-specified pass/fail criteria, datasets with provenance and licences, what was locked when |
| 5 Mechanisms | Implement and describe the Flaw-2 (`audit_sampled`, stratified estimator) and Flaw-3 (two queues) fixes; threshold rationale (Flaw 1) |
| 6 Evaluation | 6.1 correctness via traceability matrix; 6.2 scalability (+ hosted test); 6.3 model *pipeline verification* with E5–E7; new 6.4 audit-estimator simulation; 6.5 hazard verification; 6.6 device benchmarks; 6.7 vision-test physical verification; 6.8 assistant evaluation |
| 7 Guidelines | Keep, but tie each guideline to the evidence that supports it |
| 8 Limitations | Add a box "What this paper does **not** show": accuracy on patients, usability, referral completion, effect on outcomes |
| 9 Declarations | Data provenance and licences; ethics exemption statement; AI-tool use (no labels may rely on AI alone); author contributions incl. clinician; code + data on GitHub with a Zenodo DOI |
| Supplement | Reporting checklists: CLAIM (imaging AI), relevant TRIPOD+AI items; DECIDE-AI for any user study; hazard table; traceability matrix |

## A6. Pre-submission checklist (desk-reject filter)

1. Every number in the abstract has a CI or a test reference.
2. No word "safe/accurate/effective" without a measured outcome.
3. Test set never used for any design decision — or reuse openly declared.
4. Every label has a named human source and agreement statistics.
5. Every rule a clinician relies on is reviewed by a named clinician.
6. Data provenance and licence for every image.
7. Ethics statement (exemption or approval number) and consent where people are involved.
8. Ablation isolates one factor at a time.
9. Threshold and workload numbers are consistent with each other.
10. Audit-sampling probability is recorded and the estimator is stated.
11. Blinding status of every reference label is stated.
12. Limitations box lists what is not shown.
13. Reporting checklist attached.
14. Code and data available with a DOI.
15. Clinician co-author and an independent reader (not the author) checked the manuscript.

## A7. Timeline (IELTS in November)

- **Before IELTS (Claude can do now, no data needed):** E1, E2, E4, E5(c–f), draft E3, fix Flaws 1–3 in code (migration 0008 — needs your OK because the database is live), rewrite the paper sections in A5.
- **After IELTS:** clinician co-author (Lions Eye Hospital / Chittagong contacts), E6, E9, E8, E10, ethics exemption letter, optional formative study, then submit to a formative/technical venue. Paper 2 (field data) goes to JBHI or JMIR mHealth.

---

# PART B — বাংলা (আপনার জন্য)

## B0. এক কথায় সিদ্ধান্ত

Netra AI-এর Paper 1 যদি আজকের অবস্থায় JBHI-তে পাঠানো হয়, তাহলে খুব সম্ভবত MediSathi-র মতো **একই দুই কারণে** review-এ যাওয়ার আগেই রিজেক্ট হবে: (১) পদ্ধতি যথেষ্ট কঠোর নয়, (২) প্রমাণ যথেষ্ট নয়। কাজ ভালো হয়েছে, কিন্তু:
- মডেল পরীক্ষা হয়েছে মাত্র ৩৪টা ইন্টারনেটের ছবিতে, আর সেই একই test ছবি দেখে আমরা v1 থেকে v3 বানিয়েছি (test "আর স্বাধীন নেই")।
- ছবির লেবেল কোনো চক্ষু ডাক্তার যাচাই করেননি; মণির বৃত্ত আঁকা হয়েছে AI-এর সাহায্যে।
- লেখকদের মধ্যে কোনো ডাক্তার নেই।
- "safe" (নিরাপদ) শব্দটা ব্যবহার করা হয়েছে, কিন্তু রোগীর ওপর কোনো প্রমাণ নেই।

ভালো খবর: প্রায় সবকিছুই ঠিক করা যায়, আর বেশিরভাগ ঠিক করতে রোগীর ডেটা লাগে না।

## B1. MediSathi-র ভুল → Netra-তে একই ঝুঁকি → সমাধান

| MediSathi-তে যা বলেছে | Netra-তে একই ঝুঁকি | সমাধান |
|---|---|---|
| মাত্র ৭২টা প্রশ্ন, তাও আপনার নিজের বানানো | মাত্র ৩৪টা ছবি | AUC-কে "নির্ভুলতা" না বলে "pipeline ঠিক চলে কিনা তার যাচাই" বলা; একাধিকবার ভাগ করে পরীক্ষা; সব সংখ্যায় CI |
| test দেখে পরে training বদলানো হয়েছে | একই test ছবি v1 আর v3 দুটোতেই | সৎভাবে লিখে দেওয়া; একদম নতুন, তালাবদ্ধ test set |
| ডাক্তার ডেটা যাচাই করেননি | ছবির লেবেল ডাক্তার দেখেননি; বৃত্ত AI দিয়ে আঁকা | ডাক্তার দিয়ে ২২৬টা ছবি আবার লেবেল; দুজন মানুষ দিয়ে বৃত্ত; মিলের হার (κ, IoU) |
| কোন পরিবর্তনে উন্নতি হলো আলাদা করা হয়নি | crop বনাম full আলাদা করা হয়েছে (ভালো), কিন্তু একবারই চালানো | একবারে একটা জিনিস বদলে টেবিল; ৫ বার ভিন্ন seed |
| নিরাপত্তা নিয়ম ডাক্তার যাচাই করেননি | triage নিয়ম, জরুরি নিয়ম, chatbot-এর উত্তর — সব আমাদের লেখা | ডাক্তার প্রতিটা নিয়ম দেখে সই করবেন |
| মাত্র ৭ জন রোগী | কোনো রোগী/ব্যবহারকারী নেই | ক্লিনিক্যাল দাবি না করা, অথবা ছোট একটা ব্যবহার-পরীক্ষা (ethics নিয়ে) |
| ethics/সম্মতির কাগজ নেই | ইন্টারনেটের মুখের ছবি — কোথা থেকে, কোন লাইসেন্সে লেখা নেই | ডেটার উৎস + লাইসেন্সের টেবিল; সম্ভব হলে ethics exemption চিঠি |

## B2. আমি নিজে খুঁজে পাওয়া ৩টা গুরুত্বপূর্ণ ভুল (reviewer ধরবে)

**ভুল ১ — threshold আর ডাক্তারের কাজের হিসাব মেলে না।**
এখন অ্যাপে threshold 0.0691। এতে সুস্থ চোখের মাত্র ১৯% ঠিকভাবে "সুস্থ" ধরা পড়ে। ফলে মাঠে প্রায় **৮৫% চোখই "সন্দেহজনক"** দেখাবে → দিনে ~৩৪,০০০ চোখ ডাক্তারকে দেখতে হবে। অথচ paper-এ লেখা ২৫% (~১১,৫০০)। Threshold 0.5 হলে ১৯–৩১% (~৭,৫০০–১২,৪০০)।
সমাধান: আসল threshold-এ হিসাব দেখানো, কেন এই threshold তা যুক্তি দিয়ে লেখা, অথবা দুই স্তরের threshold ("জরুরি রেফার" আর "ডাক্তার দেখবেন")।

**ভুল ২ — ৫% গোপন অডিটের হিসাব বাঁকা (biased) হয়ে যায়।**
কোনো অডিট-রোগীর দৃষ্টি কম হলে ডাটাবেস তার কারণ "random_audit" থেকে বদলে "low_vision" করে দেয়। তখন আর জানা যায় না যে সে লটারিতে উঠেছিল। আর দৃষ্টি কম রোগীদের মধ্যেই AI-এর মিস করা ছানি বেশি থাকে। আমি simulation চালিয়ে দেখেছি: এতে AI-এর sensitivity আসলের চেয়ে **প্রায় ৭% বেশি** দেখায় (০.৮৫-কে ০.৯২ মনে হয়)।
সমাধান: একটা স্থায়ী `audit_sampled` চিহ্ন রাখা যা কখনো বদলাবে না, আর স্তরভিত্তিক (stratified) হিসাব। এতে ভুল ১%-এর নিচে নামে।
আরেকটা ব্যাপার: শুরুতে রোগী কম থাকবে (যেমন ২০০০ জন), তখন ৫% অডিটে হিসাব খুব অনিশ্চিত হয় (±০.০৮)। তাই পাইলটে **১০–২০% অডিট** রাখা উচিত। যুক্তরাজ্যের NHS-ও ডায়াবেটিক চোখ পরীক্ষায় অন্তত ১০% "সুস্থ" ফলাফল আবার যাচাই করে।

**ভুল ৩ — ডাক্তারের কাছে AI-এর ফল আসলে লুকানো থাকে না।**
AI স্কোরের টেবিল লুকানো ঠিকই, কিন্তু রিভিউ তালিকায় "urgent" (মানে AI বলেছে ছানি) আর "audit" (মানে AI বলেছে নেই) লেখা দেখায়, আর সেই অনুযায়ী সাজানো থাকে। ফলে ডাক্তার লেবেল দেওয়ার আগেই বুঝে যান AI কী বলেছে। তখন ডাক্তারের লেবেল দিয়ে AI-কে ন্যায্যভাবে মাপা যায় না।
সমাধান: দুটো আলাদা তালিকা — (ক) চিকিৎসার তালিকা (সব দেখা যাবে), (খ) গবেষণার লেবেল তালিকা (এলোমেলো ক্রমে, কারণ/অগ্রাধিকার লুকানো)।

## B3. সবচেয়ে বড় ৪টা সিদ্ধান্ত

1. **Paper-এর নাম ও দাবি বদলানো:** "ডিজাইন ও ক্লিনিক্যাল-পূর্ব প্রযুক্তিগত যাচাই"। যা পরীক্ষা করে দেখানো হয়নি, তা দাবি করা যাবে না।
2. **একজন চক্ষু ডাক্তার সহ-লেখক:** ছবি লেবেল করবেন, নিয়মগুলো দেখে সই করবেন। এটাই সবচেয়ে বড় বিশ্বাসযোগ্যতা। Lions হাসপাতালের সাথে যোগাযোগের সময় এটাও চাইতে পারেন।
3. **জার্নাল বদলানো:** রোগীর ডেটা ছাড়া JBHI ঠিক জায়গা নয়। ভালো বিকল্প: JMIR Formative Research, BMC Medical Informatics and Decision Making, IEEE Access, অথবা কনফারেন্স (IEEE EMBC, ACM COMPASS, MICCAI-OMIA)। পাঠানোর আগে প্রত্যেকটার scope পাতা মিলিয়ে নিতে হবে। JBHI রাখুন Paper 2-এর (আসল রোগীর ডেটা) জন্য।
4. **চাইলে:** ৫–৮ জন স্বাস্থ্যকর্মী দিয়ে ছোট ব্যবহার-পরীক্ষা (সময় মাপা + SUS প্রশ্নপত্র), ethics অনুমতি নিয়ে। এতে paper "prototype" থেকে "formative evaluation" হয়ে যায়।

## B4. নতুন যা যোগ করতে হবে (বেশিরভাগে রোগী লাগে না)

- **E1** অডিট হিসাবের simulation (শুরু করে দিয়েছি)
- **E2** ডাক্তারের কাজের চাপের হিসাব, আসল threshold-এ
- **E3** ঝুঁকি বিশ্লেষণ টেবিল: কী ভুল হতে পারে → কোন ব্যবস্থা ঠেকায় → কোন test দিয়ে প্রমাণ (ডাক্তার সই করবেন)
- **E4** ইন্টারনেট ১০০০ বার কেটে দিয়ে পরীক্ষা — কোনো স্ক্রিনিং হারায় না, দুবারও ঢোকে না
- **E5** মডেল: ৫ বার ভিন্নভাবে চালানো, CI, calibration, একবারে একটা জিনিস বদলে তুলনা
- **E6** ডাক্তার দিয়ে ২২৬টা ছবি আবার লেবেল; দুজন মানুষ দিয়ে বৃত্ত আঁকা
- **E7** অন্য উৎসের পাবলিক ছবিতে (যেমন SLID) পরীক্ষা — "চাপ পরীক্ষা" হিসেবে, নির্ভুলতার দাবি হিসেবে নয়
- **E8** ৩টা সস্তা Android ফোনে গতি, RAM, ব্যাটারি মাপা (আপনার/বন্ধুদের ফোন)
- **E9** স্কেল দিয়ে মেপে দেখা ফোনের পর্দায় E অক্ষরের মাপ ঠিক আছে কিনা; ছাপানো আইরিস দিয়ে দূরত্ব মাপা
- **E10** chatbot পরীক্ষা: ডাক্তারের লেখা ১০০টা প্রশ্ন (আপনার নয় — MediSathi-র ভুল এখানেই হয়েছিল)
- **E11** আসল সার্ভারে (আলাদা test project) লোড টেস্ট

## B5. Paper-এর কোথায় কী বদলাবে (সংক্ষেপে)

- **শিরোনাম ও সারাংশ:** "pre-clinical technical verification" যোগ; "safe" বাদ; সীমাবদ্ধতা Results-এই লেখা
- **ভূমিকা:** contribution-এর বদলে ৪টা গবেষণা প্রশ্ন (RQ1–RQ4)
- **Related work:** NHS-এর ১০% অডিট (আমাদের ধারণার পূর্বসূরি), ভারতের ২০২৫-এর multimodal ফোন-অ্যাপ গবেষণা (১১৮ রোগীতে পরীক্ষা — সরাসরি প্রতিদ্বন্দ্বী), SLID/CatScreen ডেটাসেট
- **নতুন অধ্যায় "Evaluation design":** ফল দেখানোর আগেই লিখে দেওয়া কী মাপব, কোনটা পাস/ফেল
- **ফলাফল:** নতুন ৬.৪–৬.৮ উপ-অধ্যায় (E1–E10)
- **Limitations:** "এই paper যা দেখায় না" বক্স
- **Declarations:** ডেটার উৎস ও লাইসেন্স, ethics, AI ব্যবহারের ঘোষণা (কোনো লেবেল শুধু AI দিয়ে নয়), কোড ও ডেটা GitHub + DOI

## B6. সময়সূচি (নভেম্বরে IELTS)

- **IELTS-এর আগে (আমি এখনই করতে পারি, ডেটা লাগে না):** E1, E2, E4, E5-এর বড় অংশ, E3-এর খসড়া, ভুল ১–৩ কোডে ঠিক করা (migration 0008 — লাইভ ডাটাবেস বলে আপনার অনুমতি লাগবে), paper-এর অংশগুলো নতুন করে লেখা।
- **IELTS-এর পরে:** ডাক্তার সহ-লেখক, ছবি আবার লেবেল, ফোনে মাপজোখ, chatbot পরীক্ষা, ethics চিঠি, তারপর জমা।

---
*Sources consulted: JBHI decision letter for MediSathi (26 Oct 2026); Netra AI Paper 1 draft v0.1; NHS "The Management of Grading" (diabetic eye screening QA, minimum 10% of normal grades to second-level grading); "Development and clinical validation of lightweight, multimodal machine learning models for smartphone-based cataract detection and classification", medRxiv 10.1101/2025.11.14.25340278 (not peer reviewed); SLID dataset, Front Digit Health 2025, doi 10.3389/fdgth.2025.1716501; CatScreen dataset (IIT Jodhpur, AIKosh); DECIDE-AI reporting guideline (Nat Med 2022). Simulation script: p1rev/sim.py.*
