# Netra AI — System Design
# নেত্র এআই — সিস্টেম ডিজাইন

This folder collects everything about how the Netra AI system is designed: requirements, architecture, data model, decision records, triage and audit logic, offline synchronization, scalability, verification and hazards.

**বাংলা:** Netra AI সিস্টেম কীভাবে বানানো, এক জায়গায় সব। এখানে আছে: চাহিদা, কাঠামো, ডাটাবেস, বড় সিদ্ধান্তগুলো, রেফার আর অডিটের নিয়ম, অফলাইন সিঙ্ক, বড় হওয়ার সক্ষমতা, যাচাই আর ঝুঁকির বিশ্লেষণ।

## Read in this order / এই ক্রমে পড়ুন

| File | What it covers / কী আছে |
|---|---|
| `01_System_Design_Document_EN_BN.md` | First full design document (v0.1, Oct 2026): users, requirements, capacity, architecture, flows, data model, security, ML lifecycle, roadmap, risks. Some "to do" items in it are now done; the files below show the current design / প্রথম পূর্ণ নকশা; এর কিছু "বাকি" কাজ এখন শেষ, নিচের ফাইলগুলো এখনকার অবস্থা |
| `02_Architecture_Decision_Records.md` | 18 design decisions with reasons and alternatives / ১৮টা বড় সিদ্ধান্ত ও তার কারণ |
| `03_Architecture_and_Requirements.md` | Prioritised requirements, four-part architecture, table-level access rules / চাহিদা, কাঠামো, টেবিলের নিয়ম |
| `04_Triage_Audit_Offline_Logic.md` | Two-tier band, random audit and estimator, blinding, guard, privacy, offline sync / রেফারের নিয়ম, অডিট, অফলাইন |
| `05_Scalability_Quality_Attributes_Tradeoffs.md` | Load test, doctor workload, quality attributes, growth stages, trade-offs / লোড, ডাক্তারের সময়, বৃদ্ধি, আপস |
| `06_Verification_and_Design_Faults.md` | How the system was verified; seven faults found and fixed; database checks / যাচাই আর ৭টা ভুল |
| `07_Hazard_Analysis.md` | ISO 14971-style hazard table / ঝুঁকির তালিকা |
| `diagrams/` | Architecture, triage/audit/grading flow, growth stages (paper figures); Mermaid diagrams (architecture overview, offline sync sequence, early data model) with source in `diagrams/mermaid/` / নকশার ছবি |
| `database/migrations/` | All SQL migrations 0000–0009 (the actual database design) / ডাটাবেসের আসল SQL |
| `database/tests/` | Database behaviour tests, authentication shim, benchmark results / ডাটাবেস পরীক্ষা |

## Design in one paragraph / এক অনুচ্ছেদে নকশা

Health workers use an installable web app that works offline: it guides the camera, finds the iris, crops the pupil, scores it with an on-device MobileNetV2 (WASM back end), runs a calibrated vision test and queues everything locally with phone-generated identifiers. When the network returns, idempotent writes and deterministic photo names make every retry safe. In PostgreSQL, a trigger turns each score into a band (urgent refer, routine review, negative), draws a permanent random audit of negatives, and row-level security decides who sees what; doctors grade in a blinded research queue. The database is not the bottleneck even at national scale (12× margin); storage and ophthalmologist time are.

**বাংলা:** স্বাস্থ্যকর্মীর অ্যাপ ইন্টারনেট ছাড়াই চলে। এটা ক্যামেরা চালাতে সাহায্য করে, মণি খুঁজে কেটে নেয়, ফোনেই AI স্কোর দেয়, আর দৃষ্টি পরীক্ষা করে। সব তথ্য ফোনে জমা থাকে, আর নেট ফিরলে কোনো ডুপ্লিকেট ছাড়া পাঠায়। ডাটাবেস নিজেই ঠিক করে কোন রোগী জরুরি রেফার, কোনটা ডাক্তার দেখবেন, আর কোনটা নেগেটিভ। নেগেটিভ রোগীদের থেকে এলোমেলোভাবে কিছু অডিটে যায়। কে কী দেখতে পারবে, সেটাও ডাটাবেস নিয়ন্ত্রণ করে। দেশজুড়ে চালালেও ডাটাবেস সমস্যা না; আসল সীমা স্টোরেজ আর ডাক্তারের সময়।
