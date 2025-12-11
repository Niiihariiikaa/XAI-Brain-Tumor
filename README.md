

# 🧠 EXPLAINABLE FRAMEWORK FOR BRAIN TUMOR DETECTION


This project aims to **detect brain tumors from MRI scans** using deep learning models and provide **explainability** through Grad-CAM and SHAP.
The project focuses on *interpretability*, *clinical reliability*, and *transparent AI*.

---

# 📂 1. Dataset Description 

The dataset consists of **MRI brain images** collected from publicly available medical datasets (such as BraTS or standard Kaggle tumor datasets).
Typical dataset structure:

* **Tumor Images** – Glioma, Meningioma, Pituitary
* **No-Tumor Images** – Healthy MRI slices
* **Modalities** may include FLAIR, T1, T1CE, T2 (if multi-modal)

### 🧠 Types of MRI Modalities 

| Modality  | What It Shows                      | Why It Helps                              |
| --------- | ---------------------------------- | ----------------------------------------- |
| **T1**    | Anatomical details                 | Good for structural boundaries            |
| **T1CE**  | Tumor enhancement after contrast   | Highlights active tumor regions           |
| **T2**    | Shows water content                | Useful for edema                          |
| **FLAIR** | Suppresses CSF, highlights lesions | Best for spotting abnormal bright regions |



### Dataset Challenges

* Intensity varies between MRI machines
* Tumor shape & size highly irregular
* Low number of samples → need augmentation
* Background noise, skull regions add complexity

---

# 2. Preprocessing Pipeline

Preprocessing ensures all MRI images are consistent and usable for deep learning.

### **Step 1 — Skull Stripping**

Removes skull, leaving only brain region.

Why?

* Model focuses only on tumor areas
* Reduces irrelevant pixels

### **Step 2 — Intensity Normalization**

MRI intensities vary a LOT, so we standardize:

* **Z-score normalization:**
  [
  X' = \frac{X - \mu}{\sigma}
  ]
* Helps CNN interpret images consistently.

### **Step 3 — Resizing**

All images are resized to a fixed shape (e.g., **224×224**).

Why?

* Deep models expect fixed input
* Reduces computational cost

### **Step 4 — Augmentation**

To avoid overfitting:

* Rotation
* Horizontal/vertical flips
* Elastic deformation
* Brightness/contrast shifts

These mimic real-world MRI variations.

### **Step 5 — Train/Test Split**

Typical:

* 70% Training
* 15% Validation
* 15% Testing

---

# 🧩 3. Model Architecture 

We used two types of architectures:

---

## ⭐ A. CNN-Based Classification (ResNet / Custom CNN)

<img width="850" height="487" alt="image" src="https://github.com/user-attachments/assets/5e279c28-ac82-4d36-97ea-a25f5ff2d755" />
<img width="3003" height="1335" alt="image" src="https://github.com/user-attachments/assets/2b763ddc-00ee-482c-9c34-2b4f08512951" />



### Why CNN?

* Recognizes edges, textures, tumor boundaries
* Learns spatial patterns
* Excellent for 2D MRI slices

### Working:

1. Convolution layers extract features
2. Pooling reduces dimensions
3. Fully connected layers → classification
4. Softmax → Tumor / No Tumor

---

## ⭐ B. Vision Transformers (ViT) — Optional Enhancement

![Image](https://media.geeksforgeeks.org/wp-content/uploads/20250108160202257232/Vision-Transformer-Architecture_.webp?utm_source=chatgpt.com)
<img width="850" height="447" alt="image" src="https://github.com/user-attachments/assets/a48a9333-5f30-4058-9fa5-c5ce9742f096" />


![Image](https://www.researchgate.net/publication/387984726/figure/fig2/AS%3A11431281419262625%401746193965850/A-block-figure-of-the-Vision-Transformer-ViT-Input-images-are-divided-into-patches.tif?utm_source=chatgpt.com)

### Why Transformers for MRI?

* Treat image as sequence of patches
* Capture long-range dependencies
* Better global understanding of tumor context

### Steps:

1. Image is split into 16×16 patches
2. Patches converted into embeddings
3. Transformer encoder processes them
4. Classification head predicts tumor

Transformers often outperform CNNs on large datasets.

---

# 🔍 4. Explainable AI (XAI)

The heart of this project.
We apply **Grad-CAM, SHAP** to interpret model decisions.

---

## ** Grad-CAM**

<img width="751" height="332" alt="Screenshot 2025-11-11 230247" src="https://github.com/user-attachments/assets/78d68f0e-3b72-4c71-a319-a736915ea45b" />


**What it does:**
Creates heatmaps showing **which region influenced the prediction**.

**Why it matters:**

* Ensures model isn't focusing on background
* Helps doctors trust AI decisions

---


## ** SHAP (SHapley Additive exPlanations)**

<img width="1621" height="740" alt="Screenshot 2025-11-11 203701" src="https://github.com/user-attachments/assets/a85eefdd-df83-49ba-a5df-4d19b52ab07c" />

**What it does:**
Gives **pixel-level contribution scores** based on game theory.

Use cases:

* Dataset-level global explanations
* Detecting biases
* Understanding which regions consistently influence the model

---

# 🧠  Why Explainability Matters in Medical AI

Deep learning models are often black boxes.
XAI ensures:

* **Trustworthiness**
* **Transparency**
* **Clinical acceptance**
* **Bias detection**
* **Ethical AI deployment**

Doctors can visually confirm **why** the model said “tumor”.

---

