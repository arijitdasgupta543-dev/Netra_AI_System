# Netra AI — Ophthalmologist label check (photo check page, code NQ1)
Date received: 30 Oct 2026 · Reviewer: Dr. Nafis Ahmed Shahriar (name as entered on the page) · Reported time: 4 min · Device: Arijit's phone.
Result code: `NQ1;Dr. Nafis Ahmed Shahriar;A:nnnucccunnunnunnnnncnnuunnnnnnucnunnccnncnccuncccnnnccnnn;B:45,81,85,121;t:4`

## English
- Part A (56 images, label hidden): all answered.
  - Random sample (30): 18/24 gradable agree with the public label (75%; Cohen's kappa ≈ 0.50, moderate); 6 ungradable; disagreements: 3 labelled cataract → normal, 3 labelled normal → cataract.
  - Suspicious set (26): 10 labelled cataract judged normal; 1 labelled normal judged cataract; 3 ungradable; 12 agree.
- Part B (168 images, label shown): 4 labelled cataract disputed (ids 45, 81, 85, 121).
- Earlier code `NQ1;;A:nnnncccunn…;t:3` was Arijit's own test and must not be used as clinical labels.
- Use in paper: acknowledgement only (no co-authorship, at reviewer's request via Arijit); report agreement and label corrections; note single reviewer and short review time as limitations.

## বাংলা
- ধাপ ১-এ ৫৬টা ছবির সবগুলোর উত্তর এসেছে। এলোমেলো ছবিতে ডাক্তারের সাথে আগের লেবেলের মিল ৭৫% (মাঝারি মিল)।
- সন্দেহের ছবিগুলোর মধ্যে ১০টাতে "ছানি" লেখা ছিল, কিন্তু ডাক্তার বলেছেন ছানি নেই। এগুলো ঠিক করা হবে।
- ধাপ ২-এ আরও ৪টা ছবিতে আপত্তি এসেছে।
- আগের ৩ মিনিটের কোডটা অরিজিতের নিজের পরীক্ষা ছিল, সেটা ব্যবহার করা যাবে না।

## Update (30 Oct 2026, 12:17) — second pass / দ্বিতীয়বার
Code: `NQ1;Dr. Nafis Ahmed Shahriar;A:nnnncccnnnunnunnnnncunnnnnnnnucnunnccnncnccuncccnnncccnn;B:13,45,81,85,121,221;t:4`
- Arijit reports the reviewer was inattentive in the first pass and repeated the review carefully. Decision (Arijit): the second pass is the reference standard.
- Second pass: random-sample agreement with public labels 27/28 (96%, kappa 0.93); 9 suspicious images labelled cataract judged normal; 6 disputed in Part B (13, 45, 81, 85, 121, 221).
- Between passes 20/56 Part-A answers changed (intra-rater agreement 36/56 = 64%).
- Paper must report both passes transparently (first pass 4 min, kappa 0.50; second pass 4 min, kappa 0.93; which one is the reference and why). The 96% is label agreement, not model accuracy.
- বাংলা: অরিজিতের সিদ্ধান্তে দ্বিতীয়বারের ফল মূল হিসেবে নেওয়া হলো। paper-এ দুইবারের ফলই সৎভাবে লেখা থাকবে। ৯৬% মানে ডাক্তার আর লেবেলের মিল, AI-এর accuracy না।
