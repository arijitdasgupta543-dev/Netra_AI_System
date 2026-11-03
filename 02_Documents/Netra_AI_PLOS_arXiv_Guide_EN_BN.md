# Netra AI — arXiv preprint + PLOS Digital Health (backup: PLOS ONE)
*30 Oct 2026 · English for professors, বাংলা for Arijit*

## English

**What is ready**

| Folder | Files | Purpose |
|---|---|---|
| `arXiv/` | `Netra_AI_arXiv_source.zip`, `netra_ai_arxiv.pdf`, `arXiv_metadata.txt`, `arXiv_endorsement_request_email.txt` | Preprint: upload the zip as the source; copy title/abstract/categories from the metadata file |
| `PLOS/` | `Netra_AI_PLOS_manuscript.docx`, `Cover_letter.docx`, `Fig1.tif`–`Fig7.tif`, `S1_Table.docx`, `S1_Checklist.docx`, `PLOS_submission_form_answers.docx` | Journal submission in PLOS format: double spacing, continuous line numbers, page numbers, sentence-case headings, Vancouver references with DOIs, author summary, ethics statement, generative-AI disclosure, TRIPOD+AI checklist, TIFF figures (300 dpi, LZW) |
| `Alternative_Independent_Researcher/` | same manuscript, cover letter and arXiv files with the affiliation "Independent Researcher, Khagrachari, Bangladesh" | Use only if the affiliation is changed (see the fee note below) |
| `Netra_github_research_folder.zip` | `research/` folder | Data and code required by the PLOS data policy: image manifest with reviewed labels, iris circles, clinician review results, training and analysis scripts, benchmarks, database checks |

**Placeholders the author must fill before submission:** Kaggle dataset URL (manuscript Methods "Dataset", arXiv text, form answers); Dr. Nafis Ahmed Shahriar's affiliation (Acknowledgments) and his consent to be named; Zenodo DOI (form answers); arXiv ID (form answers, after posting); date in the cover letter.

**Fee note.** PLOS publishes free for authors at institutions in Research4Life Group A countries who have no external funding. Bangladesh is in Group A; India is not on the Research4Life list. With the Kurukshetra University affiliation, the only route to a waiver is PLOS Publication Fee Assistance, which is decided case by case and requires proof of need. Listing a university as affiliation also requires a current link to it or the department's agreement.

**Order of work.** (1) Upload `research/` to GitHub and create a Zenodo DOI. (2) Fill the placeholders. (3) Post the arXiv preprint (endorsement needed). (4) Submit to PLOS Digital Health with the arXiv ID. (5) If rejected, re-submit the same files to PLOS ONE with a new cover letter.

## বাংলা

### আমি যা তৈরি করেছি
- **arXiv-এর জন্য:** paper-এর LaTeX zip, PDF (২১ পৃষ্ঠা), ফর্মে কপি করার মতো শিরোনাম-abstract-বিভাগ, আর অনুমোদনকারীকে (endorser) পাঠানোর ইমেইল।
- **PLOS-এর জন্য:** PLOS-এর নিয়ম মেনে Word ফাইল। লাইন নম্বর, দুই লাইনের ফাঁক, পৃষ্ঠা নম্বর, সাধারণ মানুষের জন্য সারাংশ (Author summary), নৈতিকতা আর AI ব্যবহারের ঘোষণা সব আছে। সাথে ৭টা ছবির TIFF ফাইল, cover letter, দুটো সহায়ক ফাইল (S1 Table, S1 Checklist), আর জমার ফর্মে কোন ঘরে কী লিখবেন তার উত্তর।
- **GitHub-এর জন্য `research` ফোল্ডার:** PLOS-এর নিয়মে ডেটা আর কোড সবার জন্য খোলা থাকতে হয়। তাই কোন ছবি ব্যবহার হয়েছে, ডাক্তারের যাচাই, মণির বৃত্ত, আর সব script এক জায়গায় রেখেছি। ছবিগুলো নিজে দেওয়া হয়নি (কপিরাইটের কারণে), শুধু তালিকা আছে।

### আপনার কাছ থেকে যা লাগবে (৫টা জিনিস)
1. **Kaggle-এর লিংক:** যে Kaggle পেজ থেকে ছবিগুলো নামিয়েছিলেন তার লিংক পাঠান। paper-এ উৎস হিসেবে লাগবে।
2. **ডাক্তার নাফিস:** কোথায় কাজ করেন (হাসপাতাল বা কলেজের নাম), আর নাম ছাপানোর জন্য তাঁর অনুমতি। PLOS অনুমতি ছাড়া কারো নাম ছাপাতে দেয় না। WhatsApp-এ একটা "হ্যাঁ, নাম দিতে পারেন" লিখে নিলেই হবে।
3. **GitHub আর Zenodo:**
   - `Netra_github_research_folder.zip` খুলে `research` ফোল্ডারটা GitHub repo-তে drag করুন। আগের `Netra_github_upload_v3_1.zip`-ও যদি এখনো upload না হয়ে থাকে, সেটাও দিন।
   - তারপর zenodo.org-এ GitHub দিয়ে লগইন করুন, repo-টা চালু (ON) করুন, আর GitHub-এ একটা "Release" বানান। Zenodo নিজে থেকেই একটা DOI দেবে। সেটা আমাকে পাঠান।
4. **arXiv-এর অনুমোদন (endorsement):**
   - arxiv.org-এ অ্যাকাউন্ট খুলুন (প্রতিষ্ঠানের নামের জায়গায় paper-এ যা থাকবে সেটাই লিখবেন)।
   - নতুন submission শুরু করে বিভাগ হিসেবে cs.CV বাছুন। arXiv তখন একটা endorsement code দেবে।
   - এমন একজন গবেষক খুঁজুন, যিনি আগে arXiv-এর cs বিভাগে paper দিয়েছেন। যেমন আপনার পুরনো কোনো শিক্ষক, বা ছানি/AI নিয়ে লেখা কোনো গবেষক। তাঁকে আমার লেখা ইমেইলটা কোড বসিয়ে পাঠান।
   - কাউকে না পেলে আমাকে জানাবেন। তখন medRxiv-এ দেওয়ার ব্যবস্থা করব, সেখানে অনুমোদন লাগে না।
5. **PLOS অ্যাকাউন্ট:** PLOS Digital Health-এর Editorial Manager-এ অ্যাকাউন্ট খুলুন। প্রোফাইলে ORCID যুক্ত করুন, আর ORCID প্রোফাইল Public করুন।

### খুব জরুরি: প্রতিষ্ঠান আর খরচ
- আপনি প্রতিষ্ঠান হিসেবে Kurukshetra University বেছেছেন, তাই মূল ফাইলগুলো সেভাবেই বানিয়েছি।
- কিন্তু PLOS-এ ফ্রি প্রকাশের নিয়ম প্রতিষ্ঠানের দেশ দেখে ঠিক হয়। বাংলাদেশ Research4Life Group A-তে আছে, তাই সেখানে ফ্রি। ভারত Research4Life-এর তালিকাতেই নেই।
- তাই Kurukshetra লিখলে PLOS Digital Health-এর ফি ৩,১৬৫ ডলার আপনাআপনি মাফ হবে না।
- তখন একমাত্র উপায় হলো PLOS-এর "Publication Fee Assistance"-এর জন্য আবেদন করা। জমার সময় দেখাতে হবে যে আপনার টাকা নেই। পুরো ফি মাফ হবে কিনা নিশ্চিত না।
- এছাড়া বিশ্ববিদ্যালয়ের নাম দিতে হলে বিভাগের অনুমতি লাগবে, কারণ পাসের পরের কাজে আপনি এখন আর সেখানে যুক্ত নেই।
- **তাই আমি দুই সেট ফাইল বানিয়েছি।** `Alternative_Independent_Researcher` ফোল্ডারে আছে "Independent Researcher, Khagrachari, Bangladesh" লেখা সংস্করণ। এটা দিলে PLOS-এ ফ্রি হওয়ার সম্ভাবনা সবচেয়ে বেশি। Kurukshetra University-র নাম তখনও cover letter-এ আপনার শিক্ষার পরিচয় হিসেবে থাকবে।
- arXiv আর PLOS-এ একই প্রতিষ্ঠান দিতে হবে, তাই arXiv-এ জমার আগেই সিদ্ধান্ত নিন।

### কাজের ক্রম
1. GitHub-এ `research` ফোল্ডার তুলুন, তারপর Zenodo DOI নিন।
2. লিংক, ডাক্তারের তথ্য আর DOI আমাকে দিন। আমি সব ফাঁকা জায়গা (placeholder) ভরে ফাইলগুলো আবার বানিয়ে দেব।
3. arXiv-এ preprint দিন। ২–৩ দিনে প্রকাশ হয়ে একটা arXiv নম্বর পাবেন।
4. সেই নম্বর দিয়ে PLOS Digital Health-এ জমা দিন। কোন ফাইল কোন ঘরে যাবে তা `PLOS_submission_form_answers.docx`-এ লেখা আছে।
5. PLOS Digital Health reject করলে একই ফাইল PLOS ONE-এ দিন, শুধু cover letter-এ জার্নালের নাম বদলাবেন (আমি করে দেব)।

## Decision (30 Oct 2026) / সিদ্ধান্ত
- PLOS (Digital Health / ONE): **Independent Researcher, Khagrachari, Bangladesh** → choose Research4Life in the fee section. / ফি অংশে Research4Life বাছবেন।
- arXiv / TechRxiv preprint and IEEE Access version: **Dept. of CSE, Kurukshetra University**.
- Kaggle source: https://www.kaggle.com/datasets/nandanp6/cataract-image-dataset (local copy 557 of 612 images). Reviewer: Dr. Nafis Ahmed Shahriar, Chittagong Eye Hospital.
- The `PLOS/` folder now holds the Independent Researcher version; the `Alternative_Independent_Researcher/` folder is no longer needed. / `PLOS` ফোল্ডারেই এখন Independent Researcher সংস্করণ; Alternative ফোল্ডার আর লাগবে না।
