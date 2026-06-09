# Motor Imagery Classification using CSP + Spectral/Entropy Features

Classification of **4-class motor imagery** (Left Hand, Right Hand, Foot, Tongue) using EEG data from the **BCI Competition IV-2a dataset**. This project extends traditional CSP by incorporating **alpha band power (8-13 Hz)** and **Shannon entropy** as complementary features, then compares **LDA, SVM, Random Forest, XGBoost, and MLP** classifiers.

| **Aspect** | **Details** |
|------------|-------------|
| Dataset | BCI Competition IV-2a (Subjects A01, A02, A03, A05) |
| Classes | Left hand, Right hand, Foot, Tongue (4 classes) |
| EEG Channels | 22 channels |
| Trials per subject | 288 (4 classes × 72 trials) |
| Time window | 2-6 seconds post-cue |
| Frequency band | 8-30 Hz (bandpass filtered) |

---

## Feature Engineering

| Feature Type | Count | Description |
|--------------|-------|-------------|
| **CSP components** | 16 | Discriminative spatial filters (8-30 Hz bandpassed) |
| **Alpha band power** | 22 | Mean power in 8-13 Hz per channel |
| **Shannon Entropy** | 22 | Signal complexity measure per channel |
| **Total features** | 60 | Combined feature vector per trial |

---

## Key Results

| Subject | Best Accuracy | Best Fold | Best Classifier |
|:-------:|:-------------:|:---------:|:---------------:|
| **A01T** | 60.7% | 67.2% | MLP |
| **A02T** | **74.8%** | **82.8%** | MLP |
| **A03T** | 63.4% | 75.9% | XGBoost |
| **A05T** | 60.9% | 67.2% | MLP |

> **✅ MLP is the best classifier for mixed feature types (spatial + spectral + informational), achieving 82.8% best-fold accuracy**

---

## Methodology

### 1. Data Preprocessing
- Bandpass filtering (8-30 Hz) to remove noise and artifacts
- Epoching from 2 to 6 seconds after motor imagery cue
- 22 EEG channels selected for analysis

### 2. Feature Extraction
- **CSP (Common Spatial Patterns)** : 16 discriminative spatial components
- **Alpha Band Power (8-13 Hz)** : Mean power across 22 channels
- **Shannon Entropy** : Signal complexity per channel `-Σ p(x) log p(x)`

### 3. Classification
- **LDA** - Linear Discriminant Analysis
- **SVM** - Support Vector Machine (RBF kernel)
- **Random Forest** - Ensemble of decision trees
- **XGBoost** - Gradient boosted trees
- **MLP** - Multi-Layer Perceptron (3 hidden layers)

### 4. Validation
- 10-fold ShuffleSplit cross-validation (80/20 train-test split)
- Metrics: Accuracy, Precision (weighted), Recall (weighted)

---

## Experimental Results

### Subject A01T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 53.8% | 5.1% | 60.3% |
| SVM | 37.8% | 6.5% | 50.0% |
| Random Forest | 53.1% | 4.9% | 58.6% |
| XGBoost | 54.8% | 5.5% | 65.5% |
| **MLP** | **60.7%** | 5.1% | **67.2%** |

---

### Subject A02T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 68.1% | 4.2% | 75.9% |
| SVM | 32.9% | 9.9% | 46.6% |
| Random Forest | 63.8% | 5.2% | 75.9% |
| XGBoost | 68.4% | 4.9% | 79.3% |
| **MLP** | **74.8%** | 5.9% | **82.8%** |

---

### Subject A03T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 60.2% | 6.6% | 67.2% |
| SVM | 38.8% | 7.0% | 53.4% |
| Random Forest | 57.9% | 7.8% | 70.7% |
| **XGBoost** | **63.4%** | 3.8% | **72.4%** |
| MLP | 62.8% | 7.9% | 75.9% |

---

### Subject A05T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 50.7% | 4.2% | 56.9% |
| SVM | 19.3% | 2.8% | 24.1% |
| Random Forest | 49.8% | 5.0% | 60.3% |
| XGBoost | 48.1% | 3.9% | 53.4% |
| **MLP** | **60.9%** | 4.4% | **67.2%** |

---

## Classifier Summary (All Subjects)

| Classifier | Mean Acc (4 subjects) | Best Fold Average |
|------------|----------------------|-------------------|
| **MLP** | **64.9%** | **72.4%** |
| XGBoost | 62.2% | 67.7% |
| LDA | 58.2% | 65.5% |
| Random Forest | 56.2% | 66.4% |
| SVM | 32.2% | 43.5% |

---

## Performance by Subject Difficulty

| Subject | Best Accuracy | Best Fold | Difficulty |
|:-------:|:-------------:|:---------:|:----------:|
| A02T | 74.8% | 82.8% | Easy |
| A03T | 63.4% | 75.9% | Medium |
| A05T | 60.9% | 67.2% | Hard |
| A01T | 60.7% | 67.2% | Hard |

---

## Key Findings

### 1. MLP is the Most Robust Classifier
- Wins on **3/4 subjects** (A01, A02, A05)
- Average accuracy: **64.9%** across all subjects
- Handles heterogeneous feature types (spatial + spectral + entropy) effectively

### 2. XGBoost is a Strong Contender
- Wins on **A03T** (63.4% mean, 72.4% best fold)
- Second-best overall average (62.2%)
- More interpretable than MLP

### 3. SVM Fails on This Feature Set
- Consistently poor performance (32.2% average — near chance level of 25%)
- RBF kernel cannot handle high-dimensional, heterogeneous feature space

### 4. LDA Underperforms Compared to Pure CSP
- LDA assumes Gaussianity — alpha power and entropy may violate this assumption

### 5. High Subject Variability
- Best subject (A02T): **74.8%** mean, **82.8%** best fold
- Worst subject (A05T): **60.9%** mean, **67.2%** best fold

---

## Comparison: Pure CSP vs. This Approach

| Subject | Pure CSP + LDA | CSP + Alpha + Entropy + MLP | Difference |
|:-------:|:--------------:|:---------------------------:|:----------:|
| A01 | **63.6%** | 60.7% | -2.9% |
| A02 | 74.5% | **74.8%** | +0.3% |
| **Average** | **69.1%** | 67.8% | -1.3% |

> Pure CSP + LDA remains more consistent, but MLP with spectral/entropy features **matches or exceeds** on good subjects.

---

## How to Run

```bash
# Clone repository
git clone https://github.com/yourusername/motor-imagery-csp-spectral-entropy.git
cd motor-imagery-csp-spectral-entropy

# Install dependencies
pip install -r requirements.txt

# Run main pipeline
python main.py --subject A02T
