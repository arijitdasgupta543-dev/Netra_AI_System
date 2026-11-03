# Netra AI — Model v3 (pupil crop) results / মডেল v3 (মণি ক্রপ) ফলাফল
Run 25 Oct 2026 by Claude (CPU, seed 42). Same de-duplicated split as v1 (train 156 / val 34 / test 34; 2 multi-person composite images removed from train).

## What changed / কী বদলেছে
- EN: Every image got an iris circle. Automatic detection (MediaPipe face landmarks found only 3 eyes in these close-ups; Hough circles the rest) was checked on gridded contact sheets: 36 automatic circles were accepted, 188 were re-marked by visual annotation (done by Claude, **to be re-checked by a human** with `Netra_ROI_Annotator.html`). The model sees only the crop the app makes (`cropPupil`: square 2.5 × radius, 224 px).
- বাংলা: প্রতিটি ছবিতে আইরিসের বৃত্ত দেওয়া হয়েছে। স্বয়ংক্রিয় বৃত্ত গ্রিড-শিটে দেখে যাচাই করা হয়েছে: ৩৬টি ঠিক ছিল, ১৮৮টি দেখে নতুন করে চিহ্নিত (Claude করেছে, **মানুষের আবার যাচাই করা দরকার**)। মডেল শুধু অ্যাপের মতো একই ক্রপ দেখে।
- EN: Fair comparison: a full-image model (v1r) trained with the **same recipe and the same initial weights**.
- বাংলা: ন্যায্য তুলনার জন্য একই রেসিপি ও একই শুরু-ওজনে পুরো-ছবির মডেল (v1r)ও ট্রেন করা হয়েছে।

## Results on the same 34 test images / একই ৩৪টি টেস্ট ছবিতে
| Model | Test AUC | Val AUC | Sens @ val-rule* | Spec @ val-rule* | Sens / Spec @ 0.5 |
|---|---|---|---|---|---|
| v1 original (full image) | 0.927 | 0.931 | 18/18 | 3/16 | – |
| v1r full image, same recipe | 0.924 | 0.924 | 18/18 | 3/16 | 14/18 / 15/16 |
| **v3 pupil crop** | **0.958** | 0.896 | 18/18 | 3/16 | 16/18 / 15/16 |

*Threshold chosen on validation so that sensitivity ≥ 0.95 (screening rule). With only 17 validation cataracts this gives a very low threshold and low specificity.

- AUC difference v3 − v1r = +0.034, bootstrap 95% CI **−0.032 to +0.126** → **not statistically significant** (34 test images). / পার্থক্য পরিসংখ্যানগতভাবে নিশ্চিত নয়।
- Shortcut check (simple colour/size statistics → label): full image **0.670**, pupil crop **0.575** (closer to 0.5 = fewer shortcuts). / শর্টকাট অনেক কমেছে।
- Grad-CAM share of heat inside the iris ≈ the iris' share of the picture for both models (v1r 13.8% vs 14.3%; v3 51.1% vs 50.3%). So the crop removes the face, wrinkles and makeup from view, but **inside the crop the heat is not specially focused on the lens**. In cataract images the heat often sits on the white lens; in normal images it is on eyelids/lashes at the crop edge.
  বাংলা: ক্রপ মুখ, ভাঁজ ও মেকআপ সরিয়ে দেয়; কিন্তু ক্রপের ভেতরে হিট বিশেষভাবে লেন্সে কেন্দ্রীভূত নয়।
- Robustness (simulated capture problems, severity 0→1): AUC stays ≥ 0.93 for blur, glare and brightness; falls to **0.91 (dark), 0.90 (noise), 0.875 (strong JPEG)**. Sensitivity at the threshold stays 1.00 except one miss at mild blur. → the app's quality gate should reject very dark, noisy or heavily compressed photos.
  বাংলা: ঝাপসা, ঝলক ও উজ্জ্বলতায় মডেল মোটামুটি স্থির; খুব অন্ধকার, নয়েজ আর বেশি কমপ্রেস করা ছবিতে AUC কমে। তাই অ্যাপের মান-যাচাই এগুলো বাতিল করবে।
- Phone formats: TFLite (float16, 4.5 MB) — 34/34 same decisions, max diff 0.013; **TF.js graph model (used by the web app) — 34/34 same decisions, max diff 0.000002**.

## Honest conclusion / সৎ সিদ্ধান্ত
EN: Cropping removes most of the dataset shortcut and does not hurt accuracy (AUC 0.96 vs 0.92, not significant). It is the right design for the app, but **these numbers are from web images and say nothing about real patients**. Normal images are mostly young, made-up stock photos; some labels look wrong. The next real step is doctor-labelled phone photos.
বাংলা: ক্রপ করলে ডেটাসেটের শর্টকাট অনেক কমে আর accuracy কমে না। অ্যাপের জন্য এটাই সঠিক ডিজাইন। কিন্তু **এই সংখ্যাগুলো ওয়েবের ছবি থেকে, আসল রোগীর ওপর কিছু বলে না**। পরের আসল ধাপ ডাক্তারের লেবেলসহ ফোনের ছবি।

## Files / ফাইল
`models/tfjs_v3/` (already bundled in the app as `/model/model.json`), `models/netra_v3_crop.tflite`, `models/netra_v3_crop.keras`, `model_meta_v3.json`, `results_v3.json`, `circle_annotations_v3.json`, `figures/` (ROC, Grad-CAM, robustness), `scripts/`.
