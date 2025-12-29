# 📁 Project Structure

Dokumentasi lengkap struktur folder dan file dalam proyek **Infidelity Risk Early Warning System**.

---

## 🗂️ Directory Tree

```
PROJEK 3/
│
├── 📂 dataset/                          # Data mentah (8 CSV files)
│   ├── kepuasan_hubungan.csv            # Kepuasan, komunikasi, konflik
│   ├── profil_psikologis.csv            # Gaya kelekatan, kontrol diri
│   ├── akses_dan_kesempatan.csv         # DM, komunikasi rahasia, LDR
│   ├── kedekatan_emosional_lain.csv     # Curhat, emotional affair
│   ├── nilai_dan_sikap.csv              # Moral, religiusitas
│   ├── komitmen_hubungan.csv            # Status, durasi, rencana
│   ├── pemicu_stres.csv                 # Stres kerja, ekonomi, alkohol
│   └── demografi.csv                    # Usia, gender, pendidikan
│
├── 📂 model/                            # Trained model
│   └── xgb_model.pkl                    # XGBoost model + feature names
│
├── 📂 notebook/                         # Jupyter notebook
│   └── app.ipynb                        # Full ML pipeline workflow
│
├── 📄 streamlit.py                       # Web application (Streamlit)
├── 📄 requirements.txt                   # Python dependencies
├── 📄 README.md                          # Project documentation (ringkas)
├── 📄 PROJECT_OVERVIEW.md                # Technical deep dive
└── 📄 PROJECT_STRUCTURE.md               # File ini - struktur proyek
```

---

## 📝 File Descriptions

### 📂 **dataset/** (3000 rows × 8 files)

| File                           | Rows | Columns | Deskripsi                                                                |
| ------------------------------ | ---- | ------- | ------------------------------------------------------------------------ |
| `kepuasan_hubungan.csv`        | 3000 | 6       | Kepuasan hubungan, kualitas komunikasi, keintiman, konflik               |
| `profil_psikologis.csv`        | 3000 | 6       | Gaya kelekatan, validasi, kontrol diri, impulsif, empati                 |
| `akses_dan_kesempatan.csv`     | 3000 | 6       | DM lawan jenis, komunikasi rahasia, lingkungan kerja, kontak mantan, LDR |
| `kedekatan_emosional_lain.csv` | 3000 | 5       | Curhat, merasa dimengerti orang lain, sembunyikan interaksi              |
| `nilai_dan_sikap.csv`          | 3000 | 5       | Pandangan selingkuh, religiusitas, toleransi, kesetiaan                  |
| `komitmen_hubungan.csv`        | 3000 | 6       | Status, lama hubungan, anak, rencana masa depan, ketergantungan          |
| `pemicu_stres.csv`             | 3000 | 5       | Stres kerja, masalah ekonomi, konflik besar, alkohol                     |
| `demografi.csv`                | 3000 | 6       | Usia, jenis kelamin, pendidikan, pekerjaan, domisili                     |

**Total Features**: 37 fitur (setelah merge pada `responden_id`)

**Format**: CSV dengan header  
**Encoding**: UTF-8  
**Missing Values**: None (clean dataset)

---

### 📂 **model/**

#### `xgb_model.pkl` (~2-5 MB)

**Format**: Python pickle (protocol 5)

**Content**:

```python
{
    'model': XGBClassifier(...),  # Trained model object
    'feature_names': [...]        # List of 37 feature names
}
```

**Loading Example**:

```python
import pickle

with open('model/xgb_model.pkl', 'rb') as f:
    model_data = pickle.load(f)
    model = model_data['model']
    features = model_data['feature_names']
```

**Model Specs**:

- **Algorithm**: XGBoost Classifier (Multi-class: 3 classes)
- **Input**: 37 features (encoded)
- **Output**: Class prediction (0/1/2) + probability distribution
- **Performance**: ~85-90% accuracy (pada test set)

---

### 📂 **notebook/**

#### `app.ipynb` (1801 lines)

**Jupyter Notebook** dengan workflow lengkap:

| Section                      | Lines     | Deskripsi                                                        |
| ---------------------------- | --------- | ---------------------------------------------------------------- |
| **Step 1: EDA**              | 1-750     | Load data, eksplorasi univariate/bivariate, visualisasi, insight |
| **Step 2: Preprocessing**    | 753-1081  | Label encoding, feature engineering, korelasi                    |
| **Step 3: Labeling**         | 1082-1185 | Rule-based labeling (3 fase risiko)                              |
| **Step 4: Train-Test Split** | 1186-1244 | Split data 80/20, stratified                                     |
| **Step 5: Model Training**   | 1245-1437 | XGBoost training + hyperparameter tuning                         |
| **Step 6: Evaluation**       | 1438-1620 | Confusion matrix, classification report, SHAP analysis           |
| **Step 7: Deployment**       | 1621-1801 | Model serialization, prediksi contoh input                       |

**Cells**: 63 cells (markdown + code)  
**Execution Time**: ~10-15 menit (full run)  
**Kernel**: Python 3

**Key Outputs**:

- 📊 15+ visualisasi (histogram, boxplot, heatmap, learning curve)
- 📈 SHAP summary plot & feature importance
- 🎯 Confusion matrix & classification report
- 💾 Model pickle file

---

### 📄 **streamlit.py** (326 lines)

**Streamlit Web Application** untuk deployment interaktif.

#### Structure:

```python
# CONFIG (lines 1-17)
- Page configuration
- Theme settings

# LOAD MODEL (lines 19-26)
- Cached model loading

# MAPPING (lines 28-68)
- Ordinal/nominal mappings untuk input form

# HEADER (lines 70-76)
- Title & description

# INPUT FORM (lines 78-161)
- 3 columns layout
- 37 input fields (radio buttons, number input)

# PREDICTION (lines 163-326)
- Data preparation
- Model prediction
- SHAP analysis
- Results visualization
```

#### Features:

- ✅ **Interactive Form**: User input 37 fitur
- ✅ **Real-time Prediction**: Instant risk assessment
- ✅ **SHAP Explainability**: Top 3 faktor meningkatkan/menurunkan risiko
- ✅ **Probability Distribution**: Gauge chart 3 fase
- ✅ **Responsive Layout**: 3-column grid

#### Tech Stack:

- `streamlit`: Web framework
- `plotly`: Interactive charts
- `matplotlib`: Static plots
- `shap`: Model explainability

#### Run Command:

```bash
streamlit run streamlit.py
```

---

### 📄 **requirements.txt**

**Python Dependencies** (tanpa versi untuk compatibility):

```
pandas          # Data manipulation
numpy           # Numerical operations
matplotlib      # Static visualization
seaborn         # Statistical plotting
scikit-learn    # ML utilities (train_test_split, metrics)
xgboost         # XGBoost algorithm
shap            # Model explainability
streamlit       # Web framework
plotly          # Interactive visualization
pickle5         # Model serialization
```

**Installation**:

```bash
pip install -r requirements.txt
```

**Python Version**: 3.8+

---

### 📄 **README.md**

**Main Documentation** (ringkas) - mencakup:

- Project overview singkat
- Quick start guide
- Struktur folder
- Tech stack
- Author info & social links

**Target Audience**: GitHub visitors, recruiters, portfolio reviewers

---

### 📄 **PROJECT_OVERVIEW.md**

**Technical Deep Dive** - mencakup:

- Problem statement & research questions
- Dataset detail (37 features, 8 dimensi)
- Methodology lengkap (EDA → Deployment)
- Key findings dari SHAP analysis
- Future improvements
- References

**Target Audience**: Technical reviewers, data scientists, ML engineers

---

### 📄 **PROJECT_STRUCTURE.md**

**File ini** - dokumentasi struktur proyek untuk navigasi mudah.

---

## 🔄 Workflow Overview

```
1. 📂 dataset/              ──→  2. 📓 notebook/app.ipynb
   (8 CSV files)                   ├─ EDA
   3000 responden                  ├─ Preprocessing
   37 features                    ├─ Feature Engineering
                                   ├─ Labeling (rule-based)
                                   ├─ Training (XGBoost)
                                   ├─ Evaluation (SHAP)
                                   └─ Save model
                                          │
                                          ↓
                                   3. 💾 model/xgb_model.pkl
                                          │
                                          ↓
                                   4. 🌐 streamlit.py
                                      (Web Deployment)
                                          │
                                          ↓
                                   5. 👤 User Input (37 fitur)
                                          │
                                          ↓
                                   6. 🎯 Prediction + SHAP
                                      (Fase 0/1/2 + Insights)
```

---

## 📊 Data Flow

```
Raw Data (CSV)
    ↓
[Merge pada responden_id]
    ↓
Master DataFrame (3000 × 37)
    ↓
[Preprocessing]
    ├─ Label Encoding
    └─ Feature Engineering (+7 fitur)
    ↓
Processed DataFrame (3000 × 43)
    ↓
[Labeling]
    └─ Rule-based target: risiko_selingkuh (0/1/2)
    ↓
[Split] Train (2400) / Test (600)
    ↓
[Training] XGBoost Classifier
    ↓
[Save] model/xgb_model.pkl
    ↓
[Load] streamlit.py
    ↓
[Predict] User Input → Fase Risiko + SHAP
```

---

## 🎯 Entry Points

| Task              | File                  | Command                      |
| ----------------- | --------------------- | ---------------------------- |
| **Full Analysis** | `notebook/app.ipynb`  | Open in Jupyter/VS Code      |
| **Train Model**   | `notebook/app.ipynb`  | Run all cells                |
| **Run Web App**   | `streamlit.py`        | `streamlit run streamlit.py` |
| **Load Model**    | `model/xgb_model.pkl` | `pickle.load(...)`           |

---

## 📌 Notes

- **Dataset**: Dummy data (ChatGPT generated) untuk pembelajaran
- **Model**: Sudah trained, siap pakai untuk prediksi
- **Reproducibility**: Random state fixed di notebook untuk konsistensi
- **Scalability**: Struktur modular, mudah di-extend untuk fitur baru

---

## 👤 Maintainer

**Damelia**  
📧 Contact via:

- [GitHub](https://github.com/cerdasbersamadamelia)
- [LinkedIn](https://www.linkedin.com/in/dameliaa/)
- [YouTube](https://www.youtube.com/@CERDASBersamaDamelia)

---

**Last Updated**: December 29, 2025
