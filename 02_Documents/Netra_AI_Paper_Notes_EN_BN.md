# Netra AI — Notes on 5 new papers / ৫টি নতুন পেপারের নোট
Prepared 16 Oct 2026. Focus paper: Kaur et al., World J Methodol 2025 (wjm-15-4-107166).

---

## A. FOCUS — Kaur R, Morya AK, et al. "AI-based apps for screening and diagnosing diabetic retinopathy and common ocular disorders." *World J Methodol* 2025;15(4):107166. doi:10.5662/wjm.v15.i4.107166

**What it is / এটা কী:**
EN: An invited mini-review by Indian ophthalmologists. Mostly about diabetic retinopathy (DR) on fundus/OCT images, with short parts on cataract, glaucoma, keratitis, telemedicine, limitations and ethics. It reports no new experiment, so cite it for **context and limitations, not for accuracy numbers**.
বাংলা: ভারতের চক্ষু বিশেষজ্ঞদের লেখা একটি ছোট রিভিউ। বেশিরভাগ অংশ ডায়াবেটিক রেটিনোপ্যাথি (ফান্ডাস/OCT ছবি) নিয়ে; ছানি, গ্লুকোমা, টেলিমেডিসিন, সীমাবদ্ধতা ও নৈতিকতা নিয়ে ছোট অংশ আছে। নিজস্ব কোনো পরীক্ষা নেই। তাই এটা উদ্ধৃত করবেন **প্রেক্ষাপট ও সীমাবদ্ধতা বোঝাতে, accuracy-র সংখ্যার জন্য নয়**।

### What we can implement in the APP / অ্যাপে যা যোগ করা যায়
| # | Idea from the paper / পেপারের ধারণা | Netra AI feature / অ্যাপে কী হবে |
|---|---|---|
| 1 | Kanagasingam 2018 (in Table 1): specificity 92% but **PPV only 12%**, false positives blamed on **poor image quality**. Older people's photos are lower quality ("AI limitations"). / খারাপ ছবির কারণে ভুল অ্যালার্ম; বয়স্কদের ছবির মান কম। | Strong **image-quality gate** before the AI runs: blur, darkness, glare, eye not centred → "retake photo". Log how many photos were rejected. / AI চালানোর আগে ছবির মান যাচাই; খারাপ হলে "আবার তুলুন"। কতগুলো বাদ গেল তা লগ রাখা। |
| 2 | IDx-DR rule: refer anything above a threshold, **rescreen negatives yearly**. / সন্দেহজনক হলে রেফার, নেগেটিভ হলে বছরে একবার আবার পরীক্ষা। | Result screen with three outcomes: **Refer** / **Retake** / **No sign now — check again in 12 months**. / তিনটি ফল: রেফার / আবার তুলুন / এখন লক্ষণ নেই, ১২ মাস পর আবার। |
| 3 | Congenital Cataract Guardian app has three modules: **prediction, dispatching, telehealth**. / তিনটি মডিউল: পূর্বাভাস, পাঠানো, টেলিহেলথ। | Same structure: **Screen → Refer (referral slip + nearest eye hospital) → Follow-up log**. / একই কাঠামো: স্ক্রিন → রেফার → ফলো-আপ। |
| 4 | Natarajan 2019 & Upadhyaya 2025: **offline** AI on a smartphone works in rural screening. / অফলাইন AI গ্রামে কাজ করে। | Keep the TFLite model **on the phone, no internet needed**; sync later. / মডেল ফোনেই থাকবে, ইন্টারনেট লাগবে না। |
| 5 | VeriSee DR was validated **across different cameras**. / বিভিন্ন ক্যামেরায় যাচাই। | Test on at least 2–3 phone models (cheap Android + mid-range). Record phone model with every photo. / অন্তত ২–৩টি ফোন মডেলে পরীক্ষা; প্রতিটি ছবির সাথে ফোনের নাম রাখা। |
| 6 | Medico-legal risk of wrong AI diagnosis; AI must assist, keep "human touch". / ভুল রোগনির্ণয়ের আইনি ঝুঁকি। | Wording: "screening, not diagnosis"; always "see an eye doctor to confirm". Doctor has final say. / "এটা রোগনির্ণয় নয়, স্ক্রিনিং"; চূড়ান্ত সিদ্ধান্ত ডাক্তারের। |
| 7 | Data privacy; **federated learning** as a future solution. / গোপনীয়তা; ফেডারেটেড লার্নিং। | Now: consent screen, no names in images, encrypted local storage. Later (future work): federated learning across clinics. / এখন: সম্মতি, ছবিতে নাম নয়, এনক্রিপ্টেড স্টোরেজ। পরে: ফেডারেটেড লার্নিং। |

### What we can use in the RESEARCH PAPER / রিসার্চ পেপারে যা ব্যবহার করা যায়
1. **Motivation (strong):** The paper says common diseases like **cataract are "not routinely imaged"**, so data is lacking. This supports Netra AI's reason to exist.
   **প্রেরণা:** পেপার বলছে ছানির মতো সাধারণ রোগের ছবি নিয়মিত তোলা হয় না, তাই ডেটার অভাব। এটা Netra AI-র প্রয়োজনীয়তা প্রমাণ করে।
2. **Bias framework (very useful):** Morya et al. list the main AI problems: **sampling bias, measurement bias from instruments, wrong algorithm choice, poor image-quality control**. Our audit found exactly these: duplicates/leakage (54 groups), shortcut AUC 0.73, and Grad-CAM showing focus on wrinkles. Use this list as the structure of your Discussion.
   **পক্ষপাতের কাঠামো:** তাদের তালিকা (স্যাম্পলিং বায়াস, যন্ত্রের কারণে বায়াস, ভুল অ্যালগরিদম, ছবির মান নিয়ন্ত্রণের অভাব) ঠিক আমাদের পাওয়া সমস্যাগুলো। Discussion অংশ এই তালিকা ধরে সাজান।
3. **"Tailor AI to regional demographics":** justification for a Bangladesh-specific dataset and pilot.
   **"অঞ্চলভেদে AI মানিয়ে নেওয়া":** বাংলাদেশি ডেটা ও পাইলটের যুক্তি।
4. **Model choice:** the review notes MobileNet is used in eye apps (keratitis) → supports MobileNetV2.
   **মডেল বাছাই:** চোখের অ্যাপে MobileNet ব্যবহার হয়, তাই MobileNetV2 যুক্তিসংগত।
5. **Reporting style to copy:** they praise studies that give **sensitivity + specificity with 95% CI** (e.g., EyeWisdom) → our notebook already does this.
   **রিপোর্টিং:** ৯৫% CI সহ sensitivity/specificity, আমাদের নোটবুক আগেই এটা করে।

**Caution / সতর্কতা:** Some numbers are old (642 million diabetics by 2040). The review is mostly DR, not anterior-segment cataract photos. Cite the original studies it mentions (Kanagasingam 2018, Natarajan 2019, Tsai 2022) when you use their numbers.
কিছু তথ্য পুরোনো, আর রিভিউটা মূলত DR নিয়ে। সংখ্যা ব্যবহার করলে মূল গবেষণাগুলো উদ্ধৃত করবেন।

---

## B. Ignatowicz AA, Marciniak T, Marciniak E. "AI-Powered Mobile App for Nuclear Cataract Detection." *Sensors* 2025;25:3954 — **most useful technically / প্রযুক্তিগতভাবে সবচেয়ে কাজের**
- EN: Android app, slit-lamp images (Nuclear Cataract Database, 1,437 images, LOCS III). **Two stages:** (1) autoencoder gate "is this a cataract-type eye image?", (2) CNN grades severity 1–6. **The pupil region is cropped** before training. MobileNetV2/EfficientNet perform about as well as VGG16/ResNet50 (91–94.5%). Pipeline PyTorch → ONNX → TensorFlow → TFLite.
- বাংলা: Android অ্যাপ, স্লিট-ল্যাম্প ছবি। দুই ধাপ: (১) ছবিটা ঠিক ধরনের কি না যাচাই, (২) ছানির মাত্রা ১–৬ নির্ধারণ। ট্রেনিংয়ের আগে **মণির অংশ কেটে নেওয়া হয়**। ছোট মডেলও বড় মডেলের মতো কাজ করে।
- **Implement / যোগ করুন:** (a) **pupil/lens crop** before the classifier, which directly fixes our Grad-CAM problem (model looking at wrinkles); (b) a gate model "is this a usable eye photo?"; (c) later, a severity step (early vs. mature cataract) useful for surgery priority.
  (ক) ক্লাসিফায়ারের আগে মণি/লেন্স ক্রপ, এতে আমাদের Grad-CAM সমস্যা সরাসরি কমবে; (খ) "ব্যবহারযোগ্য চোখের ছবি কি না" গেট; (গ) পরে ছানির মাত্রা নির্ধারণ।
- **Limitation to mention / উল্লেখ করবেন:** slit-lamp, not plain phone photos; no healthy eyes (they used post-surgery IOL eyes as "non-cataract"). Netra AI targets plain phone photos, which is harder and different.
  স্লিট-ল্যাম্প ছবি, সাধারণ ফোন-ছবি নয়; সুস্থ চোখের ছবি নেই। Netra AI সাধারণ ফোন-ছবি নিয়ে কাজ করে, যা আলাদা ও কঠিন।

## C. Grzybowski A, Jin K. "AI-Based Medical Devices for DR Screening in the European Union." *Ophthalmol Ther* 2026;15:569–589
- EN: Scoping review of 13 CE-certified DR AI systems in the EU. Most include **automatic image-quality assessment**; reporting is inconsistent; evidence on real workflow and post-market monitoring under the **EU MDR** is limited.
- বাংলা: ইউরোপের ১৩টি অনুমোদিত AI ডিভাইসের রিভিউ। বেশিরভাগে স্বয়ংক্রিয় ছবির মান যাচাই আছে; রিপোর্টিং অসম; বাস্তব ব্যবহারের প্রমাণ কম।
- **Use / ব্যবহার:** Europe plan + EMJM essay: "to reach Europe, Netra AI must follow EU MDR, built-in quality check, standard reporting (STARD-AI)". Also supports the quality-gate design.
  ইউরোপ পরিকল্পনা ও EMJM চিঠিতে কাজে লাগবে।

## D. Cardona M, et al. (Fred Hollows Foundation). "Eye care interventions that reduce access inequities for women, rural residents and older people in LMICs." *Front Public Health* 2025;13:1578848
- EN: 39 studies, 16 countries. Outreach and teleophthalmology increased screening and referral for rural people and women. Digital tools helped rural uptake but **not significantly for women**. Few studies showed benefit for older people.
- বাংলা: আউটরিচ ও টেলি-চক্ষু সেবায় গ্রামের মানুষ ও নারীদের স্ক্রিনিং ও রেফারাল বেড়েছে; কিন্তু ডিজিটাল টুলে নারীদের সুবিধা স্পষ্ট নয়।
- **Implement / যোগ করুন:** record **sex, age, rural/urban** for each screening; measure **referral completion** (did the person actually reach the hospital?); consider female health workers. This is the core of your "field equity" research part.
  প্রতিটি স্ক্রিনিংয়ে লিঙ্গ, বয়স, গ্রাম/শহর রাখুন; রোগী আসলে হাসপাতালে গেল কি না মাপুন। এটাই "ফিল্ড ইকুইটি" গবেষণার মূল।

## E. Burmeister J, et al. "Diabetic retinopathy in rural communities: barriers and solutions." *Ann Med* 2025;57:2522323
- EN: US-focused narrative review. Barriers: distance/travel, cost, specialist shortage. Solutions: teleophthalmology, AI screening, rural workforce.
- বাংলা: যুক্তরাষ্ট্রকেন্দ্রিক রিভিউ। বাধা: দূরত্ব, খরচ, বিশেষজ্ঞের অভাব।
- **Use / ব্যবহার:** one or two lines in the Introduction about rural barriers. Low priority. / ভূমিকায় এক-দুই লাইন; কম গুরুত্বপূর্ণ।

---

## Paper plan that these notes support / এই নোট থেকে পেপারের পরিকল্পনা
**Paper 1 (possible now, no patient data needed / এখনই সম্ভব):**
"Hidden duplicates and shortcut learning in a public smartphone cataract dataset: an audit and a lens-cropped baseline for Netra AI."
- Audit: 557 → 226 clean; 218 duplicates; 54 leaked groups; shortcut AUC 0.73; Grad-CAM on wrinkles.
- Fix: pupil/lens crop (idea from Sensors 2025) → re-train → compare Grad-CAM and AUC.
- Discussion structured on Morya's bias list (WJM 2025).
**Paper 2 (after ethics + pilot / অনুমোদন ও পাইলটের পর):** field study with doctor labels, quality-reject rate, referral completion, by sex and age (Front Public Health 2025).
