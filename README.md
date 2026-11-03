# Netra AI — System Design, Research Data and Model
# নেত্র এআই — সিস্টেম ডিজাইন, গবেষণার ডেটা ও মডেল

**Author / লেখক:** Arijit Das Gupta (ORCID [0009-0003-2144-5068](https://orcid.org/0009-0003-2144-5068))
**Live research app / লাইভ অ্যাপ:** https://netra-ai-bd.lovable.app
**App source code / অ্যাপের কোড:** https://github.com/arijitdasgupta543-dev/netra-ai-bd

> Research prototype, not a medical device. The AI result is a screening aid only; every suspected case is referred to a qualified eye-care professional.
> **বাংলা:** এটা গবেষণার প্রোটোটাইপ, চিকিৎসা যন্ত্র না। AI-এর ফল শুধু স্ক্রিনিংয়ে সাহায্য করে; সন্দেহজনক প্রতিটা রোগীকে চক্ষু বিশেষজ্ঞের কাছে পাঠানো হয়।

## What Netra AI is / Netra AI কী

An offline-first smartphone web application in which health workers photograph the eye with a low-cost phone, get an on-device cataract score from a pupil crop (MobileNetV2, TensorFlow.js), run a calibrated Tumbling-E vision test, and refer patients through rules enforced in the database (two-tier triage band, random audit of negative results, blinded grading queue, privacy separation). The system was verified with 29 database behaviour checks, 57 unit tests, 35,702 injected network faults and a 200,401-patient load test.

**বাংলা:** স্বাস্থ্যকর্মী সস্তা ফোনে চোখের ছবি তোলেন। ফোনেই (ইন্টারনেট ছাড়া) মণির অংশ থেকে ছানির AI স্কোর আসে, দৃষ্টি পরীক্ষা হয়, আর ডাটাবেসের নিয়ম মেনে রোগী রেফার হয়। সিস্টেমটা ২৯টা ডাটাবেস পরীক্ষা, ৫৭টা unit test, ৩৫,৭০২টা নেটওয়ার্ক-ত্রুটি আর ২ লাখ রোগীর load test দিয়ে যাচাই করা।

## Key results (public web images, not patients) / মূল ফলাফল (ওয়েবের ছবি, রোগী না)

| Item | Result |
|---|---|
| MobileNetV2, out-of-fold AUC (212 clinician-reviewed images) | 0.976 (95% CI 0.958–0.994) |
| DenseNet121 / ResNet50 / VGG16 | 0.972 / 0.984 / 0.981 (no significant difference) |
| Locked test split (32 images) | AUC 0.996 |
| TF.js and TFLite vs reference model | 32/32 identical decisions |
| Browser inference, 6× CPU throttling (WASM) | 247 ms median |
| Design faults found and fixed by verification | 7 |

## Folder map / ফোল্ডারের তালিকা

| Folder | Contents / কী আছে |
|---|---|
| `01_Papers/PLOS_Digital_Health_submission/` | Manuscript prepared for *PLOS Digital Health* (DOCX + PDF), cover letter, Fig1–Fig7 (TIFF), S1 Table, S1 Checklist (TRIPOD+AI) / PLOS-এর জন্য তৈরি পেপার |
| `03_Research_Data_and_Code/` | Image manifest with reviewed labels, iris circles, clinician review results, training and analysis scripts, benchmarks, database checks, hazard analysis, Colab notebook / ডেটার তালিকা, লেবেল, script ও পরীক্ষা |
| `04_Figures/` | Result and architecture figures (PNG) / ফলাফল ও নকশার ছবি |
| `05_Model/` | Deployed model v3.1: TF.js graph model and TensorFlow Lite (float16) / চালু মডেল |
| `06_Dataset/` | How to obtain the public images and which ones were used / ছবির উৎস ও তালিকা |
| `07_System_Design/` | Complete system design: requirements, architecture, 18 design decisions, triage/audit/offline logic, scalability, verification, hazards, diagrams and all database migrations / পুরো সিস্টেম ডিজাইন |

## Publication / প্রকাশনা

The manuscript "Netra AI: An offline-first smartphone system for cataract screening and referral—architecture, on-device deep learning and pre-clinical technical verification" is in `01_Papers/PLOS_Digital_Health_submission/` (prepared for *PLOS Digital Health*, 2026). The published version will be linked here. A field study with patients is planned and requires ethics approval and a clinical partner.

## Reproducing the results / ফলাফল আবার তৈরি করা

See `03_Research_Data_and_Code/README.md`. In short: download the Kaggle images (see `06_Dataset/`), run `ml/train_v3.py` → `ml/make_v5.py` → `ml/train_cv.py` / `run_cv.sh` → `ml/final_clean.py` → `analysis/analyze.py`.

## Author's role and tools / লেখকের ভূমিকা ও ব্যবহৃত tool

Problem selection, system design and requirements, data curation and clinician label review, model training and evaluation, verification (database checks, fault injection, load test) and analysis: Arijit Das Gupta. The web interface was scaffolded with Lovable, and AI coding assistants (Claude, Anthropic) were used for code, analysis and writing; all generated code and text were reviewed and tested by the author, who is responsible for the content.

**বাংলা:** সমস্যা বাছাই, সিস্টেমের নকশা, ডেটা যাচাই ও ডাক্তারের লেবেল রিভিউ, মডেল train ও মূল্যায়ন, যাচাই আর বিশ্লেষণ: অরিজিৎ দাশ গুপ্ত। অ্যাপের ইন্টারফেস Lovable দিয়ে শুরু করা, আর কোড, বিশ্লেষণ ও লেখায় AI সহকারী ব্যবহার করা হয়েছে; সব কোড ও লেখা লেখক নিজে যাচাই ও পরীক্ষা করেছেন।

## Acknowledgment / কৃতজ্ঞতা

Dr. Nafis Ahmed Shahriar (Chittagong Eye Hospital, Chattogram, Bangladesh) reviewed the image labels.

## Licence / লাইসেন্স

Code: MIT. Documents, figures and the manuscript: CC BY 4.0. The eye images belong to their original sources and are not redistributed here.
