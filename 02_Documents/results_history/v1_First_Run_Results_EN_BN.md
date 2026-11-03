# Netra AI — First full run results / প্রথম পূর্ণ রানের ফলাফল
Run date / রানের তারিখ: 16 Oct 2026 · Run by Claude in its own workspace (CPU) using the same notebook, MobileNetV2 with ImageNet weights, seed 42.
Claude নিজের ওয়ার্কস্পেসে (CPU) একই নোটবুক দিয়ে চালিয়েছে; MobileNetV2 + ImageNet weights, seed 42।

## 1. Data cleaning / ডেটা পরিষ্কার
| Step | Count |
|---|---|
| Images found / পাওয়া ছবি | 557 (306 cataract, 251 normal) |
| Exact + near duplicates removed / কপি বাদ | 218 (111 duplicate groups, 28 near-duplicate groups) |
| Duplicate groups leaking across original train/test / train-test ফাঁস | 54 |
| Conflicting labels dropped / উল্টো লেবেল বাদ | 4 |
| Smaller than 200 px / ২০০ পিক্সেলের ছোট | 106 |
| Composite wide images / জোড়া লাগানো চওড়া ছবি | 3 |
| **Clean pool / পরিষ্কার ছবি** | **226 (117 cataract, 109 normal)** |
| New split train/val/test | 158 / 34 / 34 |

## 2. Shortcut check / শর্টকাট পরীক্ষা
Image size only AUC 0.594 · colour only 0.633 · both **0.730**.
EN: Even without looking at the eye, simple image statistics separate the classes fairly well, so the dataset contains shortcuts.
বাংলা: চোখ না দেখেই শুধু ছবির মাপ আর রং দিয়ে ভালোভাবে আলাদা করা যাচ্ছে, অর্থাৎ ডেটাসেটে শর্টকাট আছে।

## 3. Test results (34 images, threshold chosen on validation for sensitivity ≥ 0.95)
| Metric | Value | 95% CI |
|---|---|---|
| Sensitivity | 1.000 (18/18) | 0.824–1.000 |
| Specificity | 0.375 (6/16) | 0.185–0.614 |
| PPV | 0.643 | 0.458–0.793 |
| NPV | 1.000 | 0.610–1.000 |
| Accuracy | 0.706 | 0.538–0.832 |
| ROC AUC | 0.938 | — |

TFLite model: 2.52 MB (quantized), max difference vs Keras 0.026.
TFLite মডেল: ২.৫২ MB, Keras-এর সাথে সর্বোচ্চ পার্থক্য ০.০২৬।

## 4. Grad-CAM finding (most important) / Grad-CAM (সবচেয়ে গুরুত্বপূর্ণ)
EN: In most examples the heat-map is on the **skin, wrinkles, eyelids and image edges**, not on the pupil/lens. The model is partly learning "older-looking face = cataract". One "cataract" image with a clear young-looking eye was predicted 0.02, which may be a wrong label in the web dataset.
বাংলা: বেশিরভাগ ছবিতে হিটম্যাপ **চামড়া, ভাঁজ, চোখের পাতা ও ছবির কিনারায়**, মণি বা লেন্সে নয়। মডেল আংশিকভাবে শিখছে "বয়স্ক চেহারা = ছানি"। একটি "cataract" ছবিতে পরিষ্কার কম বয়সী চোখ, মডেল দিয়েছে ০.০২; সম্ভবত ওয়েব ডেটাসেটের ভুল লেবেল।

## 5. What this means / এর মানে
- EN: The pipeline works end to end (cleaning → training → evaluation → Grad-CAM → TFLite). The numbers are **not** valid clinical performance.
  বাংলা: পুরো পাইপলাইন কাজ করছে। কিন্তু এই সংখ্যাগুলো ক্লিনিক্যাল কার্যকারিতা **নয়**।
- EN: Next fix: crop the pupil/lens region before classification (as in Ignatowicz et al., Sensors 2025), so skin and wrinkles cannot be used.
  বাংলা: পরের সমাধান: শ্রেণিবিন্যাসের আগে মণি/লেন্সের অংশ কেটে নেওয়া, যাতে চামড়া বা ভাঁজ দেখে মডেল সিদ্ধান্ত নিতে না পারে।
- EN: The real test remains doctor-labelled phone photos (notebook section 12).
  বাংলা: আসল পরীক্ষা এখনও ডাক্তারের লেবেল করা ফোন-ছবি (নোটবুকের সেকশন ১২)।

## Files / ফাইল
`netra_cataract.tflite` (for the app), `netra_mobilenetv2.keras`, `model_meta.json` (threshold, input format), `duplicates_report.csv`, `clean_manifest.csv`, `figures/`, `run_log.txt`.
