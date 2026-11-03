# Netra AI — Model v1 (full image) vs v2 (automatic iris crop)
# মডেল v1 (পুরো ছবি) বনাম v2 (স্বয়ংক্রিয় মণি/আইরিস ক্রপ)
Run 16 Oct 2026 by Claude (CPU, same 226 clean images, same train/val/test split, same settings, seed 42).

## What changed / কী বদলানো হয়েছে
EN: Before training, each image was cropped around the iris using an automatic circle detector (OpenCV Hough circles + edge check, `irisCrop.py`), with 25% margin. Idea from Ignatowicz et al., Sensors 2025.
বাংলা: ট্রেনিংয়ের আগে প্রতিটি ছবি আইরিসের চারপাশে কেটে নেওয়া হয়েছে (স্বয়ংক্রিয় বৃত্ত খোঁজার কোড), ২৫% বাড়তি জায়গাসহ।

## Detector quality (visual check of 40 random images) / ডিটেক্টরের মান
| Result | Count |
|---|---|
| Circle correctly on the iris / ঠিক জায়গায় | ~27/40 (≈ 2/3) |
| Partly on iris, too big or off-centre / আংশিক | ~10/40 |
| Wrong place / ভুল জায়গায় | ~3/40 |
See `figures/iris_detector_check_40_images.jpg`.

## Results / ফলাফল (test set, 34 images)
| Metric | v1 full image | v2 iris crop |
|---|---|---|
| Test ROC AUC | 0.938 | 0.927 |
| Validation ROC AUC | 0.927 | **0.972** |
| Sensitivity | 18/18 | 18/18 |
| Specificity | 6/16 (0.375) | 3/16 (0.188) |
| Shortcut AUC (colour + size, no deep model) | 0.730 | 0.674 |
| TFLite size | 2.52 MB | 2.52 MB |

EN: With only 34 test images, all differences are within noise (95% CIs overlap widely). **No clear gain in numbers.**
বাংলা: মাত্র ৩৪টি টেস্ট ছবি, তাই পার্থক্যগুলো এলোমেলো ওঠানামার মধ্যেই। **সংখ্যায় স্পষ্ট উন্নতি নেই।**

## Grad-CAM / কোথায় তাকাচ্ছে
EN: Partly better. In some cataract images the heat now sits on the pupil/lens (e.g., first and seventh examples). But many crops still contain eyelids and wrinkles, and on those the model still looks at skin. The shortcut is **reduced, not removed**.
বাংলা: আংশিক উন্নতি। কিছু ছানির ছবিতে এখন হিটম্যাপ মণি/লেন্সে। কিন্তু অনেক ক্রপে এখনো চোখের পাতা ও ভাঁজ থেকে যাচ্ছে, সেখানে মডেল এখনো চামড়া দেখছে। শর্টকাট **কমেছে, পুরো যায়নি**।

## Conclusion / সিদ্ধান্ত
1. EN: Automatic circle detection on mixed web photos is accurate only ~2/3 of the time, so the crop is too loose. বাংলা: ওয়েবের মিশ্র ছবিতে স্বয়ংক্রিয় বৃত্ত খোঁজা প্রায় ২/৩ বার ঠিক হয়, তাই ক্রপ ঢিলা।
2. EN: Next: **exact pupil circles marked by a person** (same as the Sensors app, where the user draws the circle), then a tight crop. A simple click-and-drag marking page for all 226 images takes ~20–30 minutes. বাংলা: পরের ধাপ: **মানুষের হাতে মণির বৃত্ত চিহ্নিত করা** (Sensors অ্যাপের মতো), তারপর টাইট ক্রপ। ২২৬টি ছবিতে ক্লিক-ড্র্যাগ করে চিহ্ন দিতে ২০–৩০ মিনিট লাগবে।
3. EN: For the paper this is still a useful, honest result: "loose automatic cropping reduced but did not remove shortcut attention; precise ROI is needed." বাংলা: পেপারের জন্য এটাও সৎ ও কাজের ফল।
4. EN: The app should use the **user-drawn pupil circle** (more reliable than automatic detection). বাংলা: অ্যাপে **ব্যবহারকারীর আঁকা বৃত্ত** ব্যবহার করা উচিত।
