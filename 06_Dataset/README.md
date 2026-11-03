# Dataset / ডেটাসেট

**Source:** Kaggle "Cataract dataset" by nandanp6 — https://www.kaggle.com/datasets/nandanp6/cataract-image-dataset
(web images collected by Bing image search; folders `train/` and `test/`, each with `cataract/` and `normal/`; 612 images in the public release).

**Why the images are not in this repository:** they were collected from the web and belong to their original owners, so they are not redistributed. Download them from Kaggle and place them as `data/Netra_AI/{train,test}/{cataract,normal}/`.

**What was used:**
- The local copy contained 557 of the 612 images (normal training images 5–9 and 50–99 were missing).
- After removing exact and near duplicates and two multi-person composites: 226 images; iris circles for 224.
- After clinician review (second pass): **212 images (100 cataract, 112 normal)**.
- The full list, with original label, reviewed label, iris circle and inclusion flag, is in `../03_Research_Data_and_Code/data/image_manifest.csv`.
- The duplicate audit is in `../03_Research_Data_and_Code/data_audit_v1/`.

**বাংলা:** ছবিগুলো Kaggle-এর "Cataract dataset" থেকে নেওয়া (লিংক উপরে)। ছবিগুলো ইন্টারনেট থেকে সংগ্রহ করা, তাই কপিরাইটের কারণে এখানে রাখা হয়নি; Kaggle থেকে নামিয়ে নিতে হবে। কোন কোন ছবি ব্যবহার হয়েছে, ডাক্তারের যাচাইয়ের পর কোনটার লেবেল কী, আর মণির বৃত্ত কোথায়, সব `image_manifest.csv` ফাইলে আছে। চূড়ান্ত ডেটা: ২১২টা ছবি (১০০ ছানি, ১১২ স্বাভাবিক)।
