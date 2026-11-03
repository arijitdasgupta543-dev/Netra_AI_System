# Netra AI — Progress Checklist / অগ্রগতির তালিকা
Last checked / শেষ যাচাই: 16 Oct 2026 (updated after first full run)

## Done / শেষ হয়েছে
- [x] Read 6 key papers (e-Paarvai Vasan 2023, NJCM 2025 field study, Eye-DR, J. Imaging 2025, Tashkandi 2025, Bangladesh paediatric eye-care) / ৬টি মূল পেপার পড়া
- [x] Collected ~557 cataract/normal phone-style images (`Downloads\Netra_AI`, `Netra_AI.rar`) / প্রায় ৫৫৭টি ছবি সংগ্রহ
- [x] Shrunk images into `Downloads\Netra_AI_small.zip` (22 MB) with `prepare_netra_upload.py` / ছবি ছোট করে zip বানানো
- [x] Data audit: 557 → 336 unique; 221 copies; 54 groups leaked across train/test; 4 conflicting labels; clean pool 226 (117 cataract, 109 normal) / ডেটা অডিট: কপি ও ফাঁস খুঁজে বের করা
- [x] Bilingual Colab notebook (cleaning → training → evaluation → Grad-CAM → TFLite) / দুই ভাষার Colab নোটবুক
- [x] Bilingual Colab guide / দুই ভাষার Colab গাইড

- [x] First full training run (done by Claude, 16 Oct): test AUC 0.938, sensitivity 1.00, specificity 0.375; Grad-CAM shows focus on skin/wrinkles → see `netra_outputs/RESULTS_SUMMARY_EN_BN.md` / প্রথম পূর্ণ ট্রেনিং রান (Claude করেছে)
- [x] Notes on 5 new papers → `Netra_AI_Paper_Notes_EN_BN.md` / ৫টি নতুন পেপারের নোট
- [x] Deep notes on Sensors 2025 cataract app; TFLite checked on full test set (34/34 same decisions) / Sensors 2025 গভীর নোট; TFLite পুরো টেস্টে যাচাই
- [x] Model v2 with automatic iris crop: no clear metric gain, Grad-CAM partly better; detector right ~2/3 → see `netra_outputs_v2/V1_vs_V2_COMPARISON_EN_BN.md` / মডেল v2 (স্বয়ংক্রিয় ক্রপ)

## Not done yet / এখনও বাকি
- [ ] Mark exact pupil circles by hand on 226 images → tight crop → retrain (v3) / ২২৬টি ছবিতে হাতে মণির বৃত্ত চিহ্নিত করে আবার ট্রেনিং (v3)
- [ ] Create GitHub repo `netra-ai` / GitHub repo খোলা
- [ ] Outreach email to eye doctors / hospital / eye camp (choose language) / ডাক্তার/হাসপাতালে ইমেইল (ভাষা ঠিক করা)
- [ ] Flutter MVP: camera → quality check → offline TFLite → result → referral log / Flutter MVP অ্যাপ
- [ ] Consent + ethics approval via partner / পার্টনারের মাধ্যমে সম্মতি ও এথিক্স অনুমোদন
- [ ] Pilot ~50 eyes with doctor labels (notebook section 12) / ডাক্তারের লেবেলসহ ~৫০ চোখের পাইলট
- [ ] Preprint (medRxiv/arXiv) + demo video / প্রিপ্রিন্ট ও ডেমো ভিডিও
- [ ] Add Netra AI to EMJM motivation letter (deadline 31 Dec 2026) / EMJM চিঠিতে Netra AI যোগ করা

## Files / ফাইল
| File | Location |
|---|---|
| `Netra_AI_Colab_Training.ipynb` | Music\Netra_AI, Downloads |
| `prepare_netra_upload.py` | Music\Netra_AI, Downloads |
| `Netra_AI_Colab_Guide_EN_BN.md` | Music\Netra_AI, Downloads |
| `netra_outputs\` + `netra_results.zip` (model, TFLite, results) | Music\Netra_AI |
| `Netra_AI_Paper_Notes_EN_BN.md` | Music\Netra_AI |
| `Sensors2025_Cataract_App_Deep_Notes_EN_BN.md` | Music\Netra_AI |
| `netra_outputs_v2\` (iris-crop model + comparison) | Music\Netra_AI |
| `Netra_AI_small.zip` (data, 22 MB) | Downloads |
| `Netra_AI.rar` + `Netra_AI\` (original images) | Downloads |
