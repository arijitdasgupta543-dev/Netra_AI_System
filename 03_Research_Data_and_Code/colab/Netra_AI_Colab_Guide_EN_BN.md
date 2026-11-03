# Netra AI — How to run the notebook on Google Colab
# নেত্র এআই — Google Colab-এ নোটবুক চালানোর নির্দেশিকা

You need two files from your **Downloads** folder / আপনার **Downloads** ফোল্ডার থেকে দুটি ফাইল লাগবে:

| File / ফাইল | What it is / এটা কী |
|---|---|
| `Netra_AI_Colab_Training.ipynb` | The notebook (code) / নোটবুক (কোড) |
| `Netra_AI_small.zip` | Your 557 shrunken images (22 MB) / আপনার ৫৫৭টি ছোট করা ছবি (২২ MB) |

---

## Step 1 — Open Colab / ধাপ ১ — Colab খুলুন

**EN:** Open https://colab.research.google.com in Chrome and sign in with your Google account.

**বাংলা:** Chrome-এ https://colab.research.google.com খুলুন এবং আপনার Google অ্যাকাউন্ট দিয়ে সাইন ইন করুন।

## Step 2 — Upload the notebook / ধাপ ২ — নোটবুক আপলোড করুন

**EN:** In the window that opens, click the **Upload** tab, then **Choose file**, and pick `Netra_AI_Colab_Training.ipynb` from Downloads. (If no window opens: menu **File → Upload notebook**.)

**বাংলা:** যে উইন্ডো খুলবে সেখানে **Upload** ট্যাব → **Choose file** চাপুন, তারপর Downloads থেকে `Netra_AI_Colab_Training.ipynb` বেছে নিন। (উইন্ডো না এলে: মেনু **File → Upload notebook**।)

## Step 3 — Turn on the free GPU / ধাপ ৩ — ফ্রি GPU চালু করুন

**EN:** Menu **Runtime → Change runtime type** → choose **T4 GPU** → **Save**. Training on CPU works but is very slow.

**বাংলা:** মেনু **Runtime → Change runtime type** → **T4 GPU** বেছে নিন → **Save**। GPU ছাড়া চলবে, কিন্তু অনেক ধীর।

## Step 4 — Upload the zip / ধাপ ৪ — zip ফাইল আপলোড করুন

**EN:**
1. On the left edge click the **folder icon** (Files panel).
2. Click the **upload icon** (page with an up-arrow) and choose `Netra_AI_small.zip`.
3. Wait until the small progress circle at the bottom-left finishes. Do not run anything before that.
4. The file must appear as `/content/Netra_AI_small.zip` (drop it in the main list, not inside `sample_data`).

**বাংলা:**
1. বাঁ পাশের **ফোল্ডার আইকনে** (Files প্যানেল) চাপুন।
2. **আপলোড আইকনে** (উপরের তীরসহ পাতা) চাপুন এবং `Netra_AI_small.zip` বেছে নিন।
3. নিচের বাঁ কোণের ছোট প্রগ্রেস বৃত্ত শেষ না হওয়া পর্যন্ত অপেক্ষা করুন। তার আগে কিছু চালাবেন না।
4. ফাইলটা `/content/Netra_AI_small.zip` হিসেবে দেখা যেতে হবে (`sample_data`-র ভেতরে নয়, মূল তালিকায়)।

## Step 5 — Run the notebook / ধাপ ৫ — নোটবুক চালান

**EN:** Menu **Runtime → Run all**. The Settings cell is already set for the zip (`USE_DRIVE = False`), so you do not need to edit anything. If you prefer, run cell by cell with **Shift + Enter** and read each output. If Colab asks "Run anyway?", click it — the notebook is yours.

**বাংলা:** মেনু **Runtime → Run all**। Settings সেলে zip-এর জন্য আগে থেকেই সব ঠিক করা আছে (`USE_DRIVE = False`), কিছু বদলাতে হবে না। চাইলে **Shift + Enter** দিয়ে এক সেল এক সেল করে চালিয়ে আউটপুট পড়তে পারেন। Colab "Run anyway?" জিজ্ঞেস করলে চাপুন — নোটবুকটা আপনারই।

## Step 6 — What to watch / ধাপ ৬ — কী কী খেয়াল করবেন

| Section / অংশ | Look for / কী দেখবেন |
|---|---|
| 1 Inventory | ~557 images found / প্রায় ৫৫৭টি ছবি পাওয়া গেছে |
| 2 Duplicates | how many copies removed (expect ~221) / কতগুলো কপি বাদ গেল (প্রায় ২২১ আশা করুন) |
| 5 Shortcut check | AUC near 0.5 is good; high = image "shortcuts" / ০.৫-এর কাছে ভালো; বেশি হলে ছবিতে "শর্টকাট" আছে |
| 8 Evaluation | sensitivity, specificity with 95% CI / সেন্সিটিভিটি, স্পেসিফিসিটি ও ৯৫% CI |
| 9 Grad-CAM | heat-maps: does it look at the lens/pupil? / হিটম্যাপ: মণি/লেন্সের দিকে তাকাচ্ছে কি? |

## Step 7 — Get your results / ধাপ ৭ — ফলাফল নামান

**EN:** The last code cell zips everything and downloads `netra_outputs.zip` (model, TFLite file, tables, Grad-CAM images). Allow the browser download. If it does not start, open the Files panel, right-click the file → **Download**.

**বাংলা:** শেষের কোড সেল সবকিছু zip করে `netra_outputs.zip` নামিয়ে দেবে (মডেল, TFLite ফাইল, টেবিল, Grad-CAM ছবি)। ব্রাউজারকে ডাউনলোড করতে দিন। না নামলে Files প্যানেলে ফাইলে রাইট-ক্লিক → **Download**।

---

## Important notes / গুরুত্বপূর্ণ কথা

- **EN:** Uploaded files disappear when the Colab session ends (idle ~90 min or closed). Download results before leaving; next time, upload the zip again.
  **বাংলা:** Colab সেশন শেষ হলে (প্রায় ৯০ মিনিট নিষ্ক্রিয় থাকলে বা বন্ধ করলে) আপলোড করা ফাইল মুছে যায়। যাওয়ার আগে ফলাফল নামিয়ে নিন; পরের বার zip আবার আপলোড করতে হবে।
- **EN:** Keep the browser tab open while training runs.
  **বাংলা:** ট্রেনিং চলার সময় ব্রাউজার ট্যাব খোলা রাখুন।
- **EN:** If you see an error, copy the red error text and send it to me. Do not skip cells.
  **বাংলা:** লাল রঙের এরর দেখলে টেক্সটটা কপি করে আমাকে পাঠান। কোনো সেল বাদ দেবেন না।
- **EN:** These results are only a pipeline test. The web images have a "young/clean = normal, old = cataract" bias. Do not quote the accuracy in a paper or essay. The real test is your own doctor-labelled phone photos (notebook section 12).
  **বাংলা:** এই ফলাফল শুধু পাইপলাইন পরীক্ষা। ওয়েবের ছবিতে "কম বয়সী/পরিষ্কার = normal, বয়স্ক = cataract" পক্ষপাত আছে। কোনো পেপার বা এসেতে এই accuracy লিখবেন না। আসল পরীক্ষা হবে ডাক্তারের লেবেল করা নিজের তোলা ফোন-ছবি (নোটবুকের সেকশন ১২)।
- **EN:** Then send me: images removed, shortcut AUC, test table, and 3–4 Grad-CAM images.
  **বাংলা:** তারপর আমাকে পাঠান: কত ছবি বাদ গেল, shortcut AUC, টেস্ট টেবিল এবং ৩–৪টি Grad-CAM ছবি।
