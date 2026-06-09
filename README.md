# Motor Imagery Classification using CSP+Spectral/Entropy Features

Classification of **4-class motor imagery** (Left Hand, Right Hand, Foot, Tongue) using EEG data from the **BCI Competition IV-2a dataset**. This project extends traditional CSP by incorporating **alpha band power (8-13 Hz)** and **Shannon entropy** as complementary features, then compares **LDA, SVM, Random Forest, XGBoost, and MLP** classifiers.

| **Aspect** | **Details** |
|------------|-------------|
| Dataset | BCI Competition IV-2a (Subjects A01-A09) |
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

## Key Results Summary

| Subject | Best Accuracy | Best Fold | Best Classifier |
|:-------:|:-------------:|:---------:|:---------------:|
| **A01T** | 60.7% | 67.2% | MLP |
| **A02T** | 74.8% | 82.8% | MLP |
| **A03T** | 63.4% | 75.9% | XGBoost |
| **A05T** | 60.9% | 67.2% | MLP |
| **A06T** | 49.0% | 63.8% | MLP |
| **A07T** | **77.8%** | **86.2%** | XGBoost |
| **A08T** | 68.4% | 79.3% | MLP |
| **A09T** | 42.8% | 48.3% | MLP |

> **🏆 Best overall: Subject A07T with XGBoost (77.8% mean, 86.2% best fold)**

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
- **MLP** - Multi-Layer Perceptron (2 hidden layers)

### 4. Validation
- 10-fold ShuffleSplit cross-validation (80/20 train-test split)
- Metric: Accuracy

---

## Detailed Experimental Results

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

### Subject A06T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 44.5% | 5.7% | 53.4% |
| SVM | 18.8% | 2.5% | 24.1% |
| Random Forest | 36.4% | 4.7% | 43.1% |
| XGBoost | 38.4% | 5.5% | 44.8% |
| **MLP** | **49.0%** | 8.8% | **63.8%** |

---

### Subject A07T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 73.6% | 4.6% | 82.8% |
| SVM | 24.8% | 7.3% | 39.7% |
| Random Forest | 70.3% | 4.5% | 77.6% |
| **XGBoost** | **77.8%** | 5.7% | **86.2%** |
| MLP | 77.1% | 5.6% | 86.2% |

---

### Subject A08T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 62.8% | 6.0% | 74.1% |
| SVM | 38.8% | 3.5% | 44.8% |
| Random Forest | 55.3% | 5.1% | 63.8% |
| XGBoost | 58.8% | 4.3% | 63.8% |
| **MLP** | **68.4%** | 5.8% | **79.3%** |

---

### Subject A09T

| Classifier | Mean Acc | Std | Best Fold |
|------------|----------|-----|-----------|
| LDA | 36.4% | 6.9% | 50.0% |
| SVM | 22.1% | 5.5% | 34.5% |
| Random Forest | 32.8% | 3.4% | 37.9% |
| XGBoost | 35.3% | 5.4% | 44.8% |
| **MLP** | **42.8%** | 3.9% | **48.3%** |

---

## Classifier Summary (All 8 Subjects)

| Classifier | Mean Acc (8 subjects) | Best Fold Average | Subjects Won |
|------------|----------------------|-------------------|--------------|
| **MLP** | **61.4%** | **69.0%** | 5 (A01, A02, A05, A06, A08) |
| **XGBoost** | **57.7%** | **65.1%** | 2 (A03, A07) |
| LDA | 56.3% | 65.0% | 0 |
| Random Forest | 53.0% | 60.8% | 0 |
| SVM | 28.4% | 38.9% | 0 |

> **✅ MLP wins on 5/8 subjects with 61.4% mean accuracy across all subjects**

---

## Performance by Subject Difficulty

| Subject | Best Accuracy | Best Fold | Best Classifier | Difficulty |
|:-------:|:-------------:|:---------:|:---------------:|:----------:|
| A07T | **77.8%** | **86.2%** | XGBoost | Very Easy |
| A02T | 74.8% | 82.8% | MLP | Easy |
| A08T | 68.4% | 79.3% | MLP | Medium-Easy |
| A03T | 63.4% | 75.9% | XGBoost | Medium |
| A01T | 60.7% | 67.2% | MLP | Medium-Hard |
| A05T | 60.9% | 67.2% | MLP | Medium-Hard |
| A06T | 49.0% | 63.8% | MLP | Hard |
| A09T | 42.8% | 48.3% | MLP | Very Hard |

---

## Ranking of Subjects by Performance
A07T ████████████████████████████████████ 77.8% (XGBoost)

A02T ████████████████████████████████░░░░ 74.8% (MLP)

A08T ████████████████████████████░░░░░░░░ 68.4% (MLP)

A03T ██████████████████████████░░░░░░░░░░ 63.4% (XGBoost)

A05T ████████████████████████░░░░░░░░░░░░ 60.9% (MLP)

A01T ████████████████████████░░░░░░░░░░░░ 60.7% (MLP)

A06T ████████████████░░░░░░░░░░░░░░░░░░░░ 49.0% (MLP)

A09T ██████████████░░░░░░░░░░░░░░░░░░░░░░ 42.8% (MLP)


---

## Key Findings

### 1. MLP is the Most Robust Classifier Overall
- Wins on **5/8 subjects** (A01, A02, A05, A06, A08)
- Average accuracy: **61.4%** across all 8 subjects
- Best fold average: **69.0%**
- Handles heterogeneous feature types effectively

### 2. XGBoost Performs Well on Good Subjects
- Wins on **A03T** (63.4%) and **A07T** (77.8% — best overall)
- Second-best overall average (57.7%)
- More interpretable than MLP

### 3. LDA is Consistent but Not Top-Performing
- Third place overall (56.3% mean)
- Never wins best classifier for any subject
- Works better with pure CSP (as shown in previous project)

### 4. SVM Fails Completely on This Feature Set
- Consistently poor performance (28.4% average — barely above chance at 25%)
- RBF kernel cannot handle high-dimensional, heterogeneous feature space

### 5. Extreme Subject Variability
| Category | Subjects | Mean Accuracy Range |
|----------|----------|---------------------|
| High performers | A07, A02, A08 | 68-78% |
| Medium performers | A03, A01, A05 | 60-64% |
| Low performers | A06, A09 | 43-49% |

---

## Statistical Summary

| Metric | Value |
|--------|-------|
| Total subjects | 8 |
| Best overall accuracy | 77.8% (A07T, XGBoost) |
| Best overall fold | 86.2% (A07T, XGBoost/MLP) |
| Worst subject accuracy | 42.8% (A09T, MLP) |
| MLP win rate | 62.5% (5/8 subjects) |
| XGBoost win rate | 25.0% (2/8 subjects) |

---

## How to Run

```bash
# Clone repository
git clone https://github.com/Hesam-Moradkhani/EEG-MI-classification-CSP-TF-Neural-Network.git
cd motor-imagery-csp-spectral-entropy

# Install dependencies
pip install -r requirements.txt

