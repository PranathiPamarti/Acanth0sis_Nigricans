# Acanthosis Nigricans Grading (Baseline Deep Learning Model)

This repository contains a baseline deep learning model for grading 
Acanthosis Nigricans (AN) severity using only RGB clinical photographs.  
The project uses **EfficientNet-B0** with transfer learning and is trained 
on a small dataset of 35 patients (99 total images).  

The aim is to build a reproducible foundation for future clinical modeling 
and research, including neck-region cropping, multi-view fusion, and 
regression-based grading.


## 📂 Project Structure

```

Acanthosis_Nigricans/
│
├── data/                     # CSV metadata files
│   ├── dataset.csv
│   ├── dataset_splits.csv
│
├── models/                   # Trained model weights
│   └── best_model.pth
│
├── processed_images/         # Preprocessed images
│
├── src/                      # Code and notebooks
│   └── main.ipynb
│
├── requirements.txt
├── README.md
└── .gitignore

```

---

## 🧪 Dataset Summary

- Total images: **99**
- Total patients: **35**
- Views: *back*, *side*, *front*
- Grades: **1 – 4** (Burke scale)
- Grade distribution:
  - Grade 4 → 39 images  
  - Grade 3 → 36 images  
  - Grade 2 → 12 images  
  - Grade 1 → 12 images  

Each patient contains 1–4 images.

---

## 🧵 Data Preprocessing

1. HEIC → JPG conversion  
2. Image resizing + normalization  
3. Filename standardization  
4. Metadata extraction:
   - patient ID  
   - grade  
   - view (back/side/front)
5. Created `dataset.csv`
6. Patient-wise split into:
   - Train → 80%  
   - Validation → 10%  
   - Test → 10%

**Why patient-wise split?**  
To avoid leakage — multiple images of the same patient should not appear in different splits.

---

## 🤖 Model Architecture

- **Backbone:** EfficientNet-B0 (ImageNet pretrained)
- **Classifier:** 4 output neurons (Grades 1–4 → encoded as 0–3)
- **Loss:** Weighted CrossEntropy (handles imbalance)
- **Optimizer:** Adam (lr = 1e-4)
- **Transforms:**
  - Random flip  
  - Random rotation  
  - Color jitter  
  - Resize 224×224  
  - ImageNet normalization  

---

## 📊 Baseline Results

Test set: **10 images**  
Validation set: **8 images**

**Test Accuracy:** 50%

- Strong performance for **Grade 4**
- Moderate performance for **Grade 3**
- No Grade 1/2 seen in test (due to small dataset)
- Confusion mainly between Grade 2/3/4 (clinically expected)

This baseline is appropriate for proceeding to next improvements.

---

## 🚀 How to Run

1. Install dependencies:
```

pip install -r requirements.txt

```

2. Open the main notebook:
```

src/main.ipynb

```

3. Run preprocessing → training → evaluation.

---

## 🔮 Future Improvements (Planned)

- 🔲 Automatic neck region cropping (YOLO / face alignment)
- 🔲 Multi-view fusion (back + side images)
- 🔲 Oversampling / WeightedRandomSampler
- 🔲 Stronger augmentation (Cutout, Mixup, etc.)
- 🔲 Focal Loss for heavy imbalance
- 🔲 Regression-based grading (ordinal modeling)
- 🔲 Grad-CAM interpretability
- 🔲 Hyperparameter tuning


---
