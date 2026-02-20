# Extended Results and Analysis

## Detailed Performance Metrics

### Isolation Forest

```
Classification Report:
              precision    recall  f1-score   support

      Normal       1.00      1.00      1.00     28443
       Fraud       0.26      0.26      0.26        38

    accuracy                           1.00     28481
   macro avg       0.63      0.63      0.63     28481
weighted avg       1.00      1.00      1.00     28481

Anomalies Detected: 77
True Positives: 10
False Positives: 67
False Negatives: 28
True Negatives: 28376
```

### Local Outlier Factor

```
Classification Report:
              precision    recall  f1-score   support

      Normal       1.00      1.00      1.00     28443
       Fraud       0.00      0.00      0.00        38

    accuracy                           1.00     28481
   macro avg       0.50      0.50      0.50     28481
weighted avg       1.00      1.00      1.00     28481

Anomalies Detected: 0
True Positives: 0
False Positives: 0
False Negatives: 38
True Negatives: 28443
```

### One-Class SVM

```
Classification Report:
              precision    recall  f1-score   support

      Normal       1.00      0.68      0.81     28443
       Fraud       0.00      0.34      0.00        38

    accuracy                           0.67     28481
   macro avg       0.50      0.51      0.40     28481
weighted avg       1.00      0.67      0.80     28481

Anomalies Detected: 9,257
True Positives: 13
False Positives: 9,244
False Negatives: 25
True Negatives: 19,199
```

## Statistical Analysis

### Transaction Amount Analysis

| Statistic | Fraud | Normal |
|-----------|-------|--------|
| Count | 492 | 284,315 |
| Mean | $122.21 | $88.29 |
| Std Dev | $256.68 | $250.11 |
| Min | $0.00 | $0.00 |
| 25% | $1.00 | $5.65 |
| 50% | $9.25 | $22.00 |
| 75% | $105.89 | $77.05 |
| Max | $2,125.87 | $25,691.16 |

**Key Observation:** Fraudulent transactions have a higher mean amount ($122 vs $88) but lower median ($9.25 vs $22), indicating a right-skewed distribution with some high-value frauds.

### Temporal Patterns

- Fraud transactions distributed across both days
- No clear temporal clustering observed
- Normal transactions show cyclical patterns (likely daily cycles)

## Feature Correlations

Top correlations with Class (fraud indicator):
- V14: -0.30 (strong negative)
- V4: -0.13
- V10: -0.10
- V12: -0.09
- V17: +0.09

Note: Due to PCA transformation, individual feature interpretability is limited.

## Computational Performance

| Algorithm | Training Time | Prediction Time | Memory Usage |
|-----------|--------------|-----------------|--------------|
| Isolation Forest | ~2s | ~0.5s | Low |
| LOF | ~5s | ~10s | Medium |
| One-Class SVM | ~30s | ~2s | High |

*Measured on sample of 28,481 transactions*

## Limitations

1. **Sample Size:** Used 10% sample due to computational constraints
2. **Temporal Split:** No temporal validation performed
3. **Feature Engineering:** Limited to provided PCA features
4. **Hyperparameter Tuning:** Used default/standard parameters
5. **Cost Matrix:** Did not incorporate business costs of false positives/negatives

## Recommendations for Production

1. **Primary Model:** Use Isolation Forest with tuned contamination parameter
2. **Threshold Tuning:** Optimize precision-recall trade-off based on business costs
3. **Ensemble:** Combine multiple algorithms for robustness
4. **Monitoring:** Implement drift detection for model retraining
5. **Human Review:** Flagged transactions require human investigation
