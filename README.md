# Credit Card Fraud Detection: Honours Research

[![Research](https://img.shields.io/badge/Research-Honours-blue.svg)]()
[![Topic](https://img.shields.io/badge/Topic-Anomaly%20Detection-green.svg)]()
[![Domain](https://img.shields.io/badge/Domain-Cybersecurity-red.svg)]()

> **Honours Research Project** — Comparative analysis of unsupervised anomaly detection algorithms for credit card fraud detection.

**Student:** Musa Brown (Mhlaba MB)  
**Student ID:** 202219155  
**Program:** Honours in Computer Science

---

## Research Abstract

Credit card fraud detection presents a significant challenge in machine learning due to extreme class imbalance (typically <0.2% fraud rate). This research evaluates three unsupervised anomaly detection algorithms—**Isolation Forest**, **Local Outlier Factor (LOF)**, and **One-Class SVM**—on the Kaggle Credit Card Fraud Detection dataset containing 284,807 transactions with only 492 fraudulent cases (0.172%).

### Key Findings

| Algorithm | Precision (Fraud) | Recall (Fraud) | Performance Summary |
|-----------|-------------------|----------------|---------------------|
| **Isolation Forest** | 26% | 26% | Best overall balance |
| **Local Outlier Factor** | 0% | 0% | Failed to detect fraud |
| **One-Class SVM** | 0% | 34% | High false positive rate |

**Conclusion:** Isolation Forest demonstrates the most practical balance for real-world fraud detection, while Precision-Recall AUC emerges as the optimal evaluation metric for imbalanced domains.

---

## Research Overview

### Problem Statement

Credit card fraud costs the global economy billions annually. Traditional supervised approaches face challenges:
- Extreme class imbalance (fraud cases are rare)
- Evolving fraud patterns (concept drift)
- Need for real-time detection
- High cost of false negatives

### Research Questions

1. How do tree-based, density-based, and boundary-based anomaly detection methods compare for fraud detection?
2. What is the optimal evaluation strategy for highly imbalanced fraud detection?
3. How do sampling strategies affect model performance?

### Algorithms Compared

| Algorithm | Type | Key Principle |
|-----------|------|---------------|
| **Isolation Forest** | Tree-based | Anomalies are isolated closer to the root of random trees |
| **Local Outlier Factor** | Density-based | Anomalies have lower local density than neighbors |
| **One-Class SVM** | Boundary-based | Learns decision boundary for normal class |

---

## Dataset

**Source:** [Kaggle Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

**Characteristics:**
- **Transactions:** 284,807 (September 2013, 2 days)
- **Fraud Cases:** 492 (0.172%)
- **Features:** 30 numerical (V1-V28: PCA components, Time, Amount)
- **Imbalance Ratio:** ~1:578

**Key Properties:**
- Highly imbalanced
- All features numerical (anonymized via PCA)
- No missing values
- Time and Amount are only non-PCA features

---

## Methodology

### Data Preprocessing
1. **Sampling:** 10% stratified sample (28,481 transactions) for computational efficiency
2. **Features:** All 30 features used
3. **Outlier Fraction:** 0.001336 (fraud ratio in sample)

### Model Configuration

```
Isolation Forest:
  - n_estimators: 100
  - contamination: 0.001336
  - random_state: 42

Local Outlier Factor:
  - n_neighbors: 20
  - contamination: 0.001336

One-Class SVM:
  - kernel: rbf
  - nu: 0.5
```

### Evaluation Metrics

Given extreme imbalance:
- **Primary:** Precision-Recall AUC (AUPRC)
- **Secondary:** Precision, Recall, F1-Score
- **Not Used:** Accuracy (misleading in imbalanced settings)

---

## Detailed Results

### Performance Summary

| Metric | Isolation Forest | LOF | One-Class SVM |
|--------|------------------|-----|---------------|
| **Precision (Fraud)** | 0.26 | 0.00 | 0.00 |
| **Recall (Fraud)** | 0.26 | 0.00 | 0.34 |
| **F1-Score (Fraud)** | 0.26 | 0.00 | 0.00 |
| **Accuracy** | 0.997 | 0.997 | 0.675 |
| **Anomalies Flagged** | 77 | 0 | 9,257 |
| **True Positives** | 10 | 0 | 13 |

### Analysis by Algorithm

#### 1. Isolation Forest ⭐ (Recommended)
- **Strengths:**
  - Best balance of precision and recall
  - Linear time complexity O(n log n)
  - Handles high-dimensional data well
  - No assumptions about data distribution
- **Weaknesses:**
  - Moderate false positive rate
  - Requires tuning contamination parameter

#### 2. Local Outlier Factor ❌ (Not Recommended)
- **Issues:**
  - Failed to detect any fraud cases
  - Struggles with high-dimensional PCA features
  - Density-based assumptions don't hold for this data
  - Computational complexity O(n²)

#### 3. One-Class SVM ⚠️ (Requires Tuning)
- **Issues:**
  - Extremely high false positive rate (9,257 flagged)
  - Only 13 true frauds detected out of 38
  - Computationally expensive
  - Sensitive to hyperparameters

---

## Key Insights

### Algorithm Selection
1. **Isolation Forest** is recommended for production fraud detection
2. **LOF** is unsuitable for high-dimensional, imbalanced financial data
3. **One-Class SVM** requires significant hyperparameter optimization

### Evaluation Strategy
- **Precision-Recall AUC** is essential for imbalanced domains
- Accuracy is misleading (99.8% accuracy achievable by predicting all normal)
- Cost of false negatives (missed fraud) > cost of false positives (investigation)

### Practical Considerations
- Implement human-in-the-loop for flagged transactions
- Use ensemble methods combining multiple detectors
- Consider cost-sensitive learning
- Monitor for concept drift in fraud patterns

---

## Repository Structure

```
honours-research-credit-card-fraud-detection/
├── README.md                 # This research documentation
├── docs/
│   └── research_paper.pdf    # Full research paper (when available)
├── results/
│   └── summary.md            # Extended results and analysis
└── data/
    └── README.md             # Dataset information
```

### Related Code Repository

The implementation code for this research is available at:
**[github.com/MusaBrown/Anomaly-Detection](https://github.com/MusaBrown/Anomaly-Detection)**

---

## Future Work

- [ ] Deep learning approaches (Autoencoders, Variational Autoencoders)
- [ ] Ensemble methods (XGBoost, Random Forest with SMOTE)
- [ ] Real-time streaming detection pipeline
- [ ] Cost-sensitive learning optimization
- [ ] Feature engineering on Time and Amount variables
- [ ] Temporal cross-validation strategies
- [ ] Explainable AI for fraud detection decisions

---

## References

1. Liu, F. T., Ting, K. M., & Zhou, Z. H. (2008). Isolation forest. *IEEE International Conference on Data Mining (ICDM)*.

2. Breunig, M. M., Kriegel, H. P., Ng, R. T., & Sander, J. (2000). LOF: identifying density-based local outliers. *ACM SIGMOD International Conference on Management of Data*.

3. Schölkopf, B., Platt, J. C., Shawe-Taylor, J., Smola, A. J., & Williamson, R. C. (2001). Estimating the support of a high-dimensional distribution. *Neural Computation*.

4. Dal Pozzolo, A., Boracchi, G., Caelen, O., Alippi, C., & Bontempi, G. (2017). Credit card fraud detection: a realistic modeling and a novel learning strategy. *IEEE Transactions on Neural Networks and Learning Systems*.

5. Machine Learning Group - ULB. (2016). Credit Card Fraud Detection Dataset. *Kaggle*.

---

## Author

**Musa Brown (Mhlaba MB)**  
Student ID: 202219155  
Honours Research Project — Computer Science

📧 [Your Email]  
🔗 [LinkedIn]  
🐙 [GitHub: @MusaBrown](https://github.com/MusaBrown)

---

## Acknowledgments

- **Dataset:** Machine Learning Group at ULB (Université Libre de Bruxelles) via Kaggle
- **Supervisor:** [Supervisor Name]
- **Institution:** [University Name]

---

*This research was conducted as part of the Honours program in Computer Science, contributing to the field of machine learning in cybersecurity and financial fraud detection.*
