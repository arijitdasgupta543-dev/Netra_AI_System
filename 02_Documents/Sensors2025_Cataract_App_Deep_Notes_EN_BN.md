# Deep notes: Ignatowicz, Marciniak & Marciniak, "AI-Powered Mobile App for Nuclear Cataract Detection", *Sensors* 2025;25:3954 (doi:10.3390/s25133954)
# গভীর নোট: Sensors 2025 ছানি অ্যাপ পেপার
Prepared 16 Oct 2026 for Netra AI. Code: github.com/ignatowicz-alicja/AI-Powered-Mobile-App-for-NuclearCataract-Detection (Kotlin, open source).

---

## 1. What they did / তারা কী করেছে
| Item | Detail |
|---|---|
| Data / ডেটা | Nuclear Cataract Database: 1,437 slit-lamp images; an ophthalmologist selected **335** good images, graded NC 1–6 (LOCS III). Plus IOL (post-surgery) eyes as "non-cataract". |
| Pre-processing / প্রস্তুতি | **Pupil region (ROI) cut out manually**, background removed, resized to 224×224. |
| Stage 1 / ধাপ ১ | Convolutional **autoencoder** = "is this a cataract eye?" (anomaly detection, threshold = 1st percentile of error). 94.6% on cataract vs IOL. |
| Stage 2 / ধাপ ২ | CNN grades severity **1–6**. Six models compared: VGG16 94.4%, ResNet18 94.5%, MobileNetV3 94.2%, VGG11 91.6%, ResNet50 91.3%, EfficientNet 91.1%. |
| Phone conversion / ফোনে রূপান্তর | PyTorch → ONNX → TensorFlow → TFLite. **Accuracy fell sharply**: MobileNetV3 94% → 63%, ResNet18 → 68%, VGG11 → 85% (chosen). |
| App / অ্যাপ | Android (Kotlin). User takes/selects photo, **draws a circle around the pupil**, app crops and classifies. ~160–190 ms per image. |
| Camera finding / ক্যামেরার ফল | Live camera photos gave more false positives, mainly from **over-exposure/glare in the pupil**. Gallery images worked better. They plan a quality check in future. |

## 2. Why it matters for Netra AI / কেন গুরুত্বপূর্ণ
EN: It is the closest published work to ours: cataract + smartphone app + lightweight CNN + open code. It confirms three problems we already see: background shortcuts, image quality, and phone conversion.
বাংলা: আমাদের কাজের সবচেয়ে কাছের প্রকাশিত গবেষণা: ছানি + ফোন অ্যাপ + ছোট মডেল + খোলা কোড। আমরা যে তিনটি সমস্যা দেখছি (পটভূমির শর্টকাট, ছবির মান, ফোনে রূপান্তর), এই পেপারও সেগুলো নিশ্চিত করে।

## 3. What we will IMPLEMENT / যা আমরা যোগ করব
| # | From the paper | In Netra AI | বাংলা |
|---|---|---|---|
| 1 | Pupil ROI crop | **Pupil/iris crop before the model.** App: user drags a circle over the pupil (simple, works offline). Training: crop training images the same way. Directly fixes our Grad-CAM problem (model looked at wrinkles). | মডেলের আগে মণি কেটে নেওয়া; অ্যাপে ব্যবহারকারী মণির ওপর বৃত্ত বসাবে। আমাদের "ভাঁজ দেখে সিদ্ধান্ত" সমস্যা সরাসরি কমবে। |
| 2 | Glare caused false positives | **Quality gate:** reject if many saturated (white) pixels inside the circle, blur, or too dark → "retake without flash, face away from light". | মণিতে ঝলক/ঝাপসা/অন্ধকার হলে "আবার তুলুন"। |
| 3 | Two-stage design | Stage 1 "usable eye photo?" → Stage 2 "cataract or not". Later Stage 3 severity (early / mature) for surgery priority. | দুই ধাপ: আগে ছবি ঠিক কি না, তারপর ছানি আছে কি না; পরে মাত্রা। |
| 4 | TFLite accuracy collapse | **Always evaluate the TFLite file itself on the test set** and report both. ✔ Done today for our model: Keras vs TFLite gave **identical decisions on 34/34 test images** (AUC 0.927 both; max probability difference 0.09). Our design (preprocessing inside the model, Keras → TFLite directly, no ONNX step) avoids their drop. | TFLite ফাইল নিজেই টেস্ট করা। আজ পরীক্ষা করেছি: ৩৪টির ৩৪টিতেই একই সিদ্ধান্ত, তাদের মতো accuracy পড়েনি। |
| 5 | Resize must match | App must resize exactly as in training (we saw AUC 0.938 vs 0.927 just from a different resize method). Write the resize rule into `model_meta.json` and the Flutter code. | অ্যাপে ছবি ছোট করার পদ্ধতি ট্রেনিংয়ের মতোই হতে হবে। |
| 6 | Speed reported | Measure and report time per image on 2–3 real phones. | ২–৩টি ফোনে প্রতি ছবিতে কত সময় লাগে মাপা। |
| 7 | Open code + image list | Publish our code, duplicate report, and clean-image list on GitHub. | কোড, কপির তালিকা ও পরিষ্কার ছবির তালিকা GitHub-এ প্রকাশ। |

## 4. Weaknesses we can point out (and do better) / তাদের দুর্বলতা (আমরা যেখানে ভালো করব)
1. **Slit-lamp images, not plain phone photos.** A slit lamp needs a clinic; Netra AI targets ordinary smartphone photos for villages. / স্লিট-ল্যাম্প ক্লিনিকে লাগে; আমরা সাধারণ ফোন-ছবি নিয়ে কাজ করি।
2. **No healthy eyes.** "Non-cataract" = post-surgery IOL eyes. Real screening must separate cataract from normal eyes. / সুস্থ চোখ নেই; অপারেশনের পরের চোখকে "স্বাভাবিক" ধরা হয়েছে।
3. **Possible leakage:** augmentation enlarged 335 → 1,272 images and 10-fold CV was run on that set; if augmented copies of one photo fell in both train and validation, scores are inflated. The paper does not say clearly. (Same kind of problem our audit found.) / অগমেন্টেশনের কপি train ও validation দুদিকে গেলে ফল বাড়তি দেখায়; পেপারে স্পষ্ট নয়।
4. **Only accuracy/F1, no sensitivity–specificity with 95% CI, no external test, no real patients, no ethics approval needed.** Our notebook already reports sensitivity, specificity, PPV, NPV with CIs. / শুধু accuracy; CI নেই, বাইরের পরীক্ষা নেই, রোগী নেই।
5. **Small:** 335 images across 6 grades (33–93 per grade). / ছোট ডেটাসেট।

## 5. Use in our research paper / আমাদের পেপারে ব্যবহার
- **Related work:** "the closest system (Ignatowicz et al., 2025) used slit-lamp images and lost up to 30 points of accuracy after TFLite conversion; we use plain phone photos and verify the converted model directly."
- **Methods:** cite them for pupil-ROI cropping and two-stage design.
- **Results:** a table "Keras vs TFLite on the same test set" (we already have it: no decision changed).
- **Discussion:** glare/over-exposure as a field risk → our quality gate; leakage risk from augmentation before splitting.

## 6. Proposed next experiment / পরের পরীক্ষা
Same 226 clean images → crop pupil/iris → retrain → compare (a) AUC, sensitivity, specificity, (b) Grad-CAM before vs after, (c) shortcut AUC. If Grad-CAM moves to the pupil, this becomes the main result of Paper 1.
একই ২২৬টি ছবিতে মণি ক্রপ → আবার ট্রেনিং → আগে-পরে তুলনা। Grad-CAM মণিতে সরলে এটাই Paper 1-এর মূল ফল।
