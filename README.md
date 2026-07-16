# BBBP-prediction-pipeline
Here is a complete, professional `README.md` file customized for your project. It incorporates the biological context and experimental design we discussed, making it an excellent showcase for your portfolio.

---

# Blood-Brain Barrier (BBB) Permeability Predictor

## 📌 Project Overview

This project develops a machine learning pipeline to predict the blood-brain barrier (BBB) permeability of chemical compounds. By comparing different chemical feature representations (Morgan Fingerprints vs. MACCS keys) and classification algorithms, this project aims to accurately classify molecules as permeable or non-permeable while documenting the biological limitations of 2D structural models.

## 🧪 Dataset & Exploratory Data Analysis

* **Data Source:** DeepChem BBBP Dataset.


* **Data Cleaning:** Parsed SMILES strings using RDKit, removing 11 invalid molecular structures to result in a clean dataset of 2039 molecules.


* **Class Imbalance:** The dataset is heavily imbalanced, with 76.4% of the molecules being permeable (1567 permeable vs. 483 non-permeable).


* **Chemical Benchmarking:** Evaluated the dataset against Lipinski's Rule of 5 and the industry-standard **CNS MPO score**. Permeable molecules averaged a CNS MPO score of 3.82, while non-permeable molecules averaged 2.41, establishing a strong biological baseline for the ML task.



## ⚙️ Methodology & Pipeline Architecture

1. **Feature Engineering:** Converted chemical structures into machine-readable numerical vectors using two methods:
* **Morgan Fingerprints:** 2048-bit vectors (radius=2) to capture circular topological environments.


* **MACCS Keys:** 167-bit vectors encoding predefined structural fragments.




2. **Experimental Design:** To prevent data leakage and ensure robust generalization, the data was subjected to a rigorous 3-way split: 60% Training, 20% Validation, and 20% Test.


3. **Model Benchmarking:** Trained and evaluated three distinct algorithms (Support Vector Classifier, Random Forest, Logistic Regression) across both feature sets. All models utilized `class_weight='balanced'` to mathematically penalize errors on the minority class.


4. **Threshold Optimization:** Conducted a probability threshold sweep on the validation set, adjusting the classification cutoff from the default 0.5 to **0.67** to optimize the precision-recall trade-off.



## 🏆 Results & Champion Model

The **Linear Support Vector Classifier (SVC)** trained on **Morgan Fingerprints** emerged as the champion model.

* Unlike tree-based models, the Linear SVC provided superior distance-based probability calibration and allowed for direct feature interpretation via coefficient magnitudes (`coef_`).


* The final model achieved a **Validation ROC-AUC of 0.897** and a final **Test ROC-AUC of 0.923**.



## 🔬 Error Analysis, Biological Context & Limitations

Real world chemical testing and misclassification analysis revealed important biological insights regarding the model's limitations:

* **Passive vs. Active Transport:** The model predicts strictly based on passive lipid-bilayer diffusion. It misclassified biological sugars (like Glucose) as False Positives. In reality, Glucose requires active biological transport (GLUT1 proteins) to cross the BBB, a biological mechanism the model cannot learn from 2D structural drawings.
* **Blindness to Physical Bulk:** The model was trained exclusively on topological fragments. Because it lacks global physical descriptors, it generated false positives for massive, highly polar molecules (like Sucrose) that cannot physically diffuse across the BBB.

## 🚀 Future Scope

To resolve the false positives associated with massive, polar molecules, future iterations of this pipeline will implement a **Hybrid Feature Matrix**. By concatenating the 2048-bit Morgan Fingerprint array with 1D physical descriptors (Molecular Weight, TPSA, LogP) generated during the EDA phase, the model will be able to mathematically penalize molecules that exceed passive diffusion size and polarity thresholds.

## 🛠️ Tech Stack

* **Chemistry/Cheminformatics:** RDKit (`Chem`, `Draw`, `Descriptors`, `MACCSkeys`, `AllChem`)


* **Machine Learning:** Scikit-Learn (`SVC`, `RandomForestClassifier`, `LogisticRegression`)


* **Data Manipulation:** Pandas, NumPy


* **Visualization:** Matplotlib, Seaborn
