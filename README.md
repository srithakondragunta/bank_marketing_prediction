# Model Performance Comparison: Before vs After Improvements

## Getting Started

Our dataset comes from the UCI Machine Learning Repository ([link](https://archive.ics.uci.edu/ml/datasets/bank+marketing)). The data is from a Portuguese banking institution and represents the outcomes of multiple marketing campaigns. For more background and information on the features, we reference the article accompanying the dataset: [CRISP-DM-BANK.pdf](CRISP-DM-BANK.pdf).

In the Python file, we begin by calculating a **baseline model accuracy** using the majority class in the target variable (`y` = *has the client subscribed to a term deposit?*). This gives us a simple benchmark to assess model performance:

```python
baseline_accuracy = y_test.value_counts(normalize=True).max()
print(f"Baseline Accuracy (majority class): {baseline_accuracy:.4f}")
```

**Baseline Accuracy**: `0.8874`  
This means any model must exceed this threshold to provide value beyond naive majority-class guessing.

After establishing this baseline, we:
- Built and evaluated simple models on the raw data.
- Applied **feature engineering** to enhance data quality.
- Performed **hyperparameter tuning** and **grid search** to optimize model parameters.

---

## Performance Summary

| Model               | Train Time (Before) | Train Accuracy (Before) | Test Accuracy (Before) | Train Time (After) | Train Accuracy (After) | Test Accuracy (After) |
|--------------------|---------------------|--------------------------|-------------------------|--------------------|-------------------------|------------------------|
| Logistic Regression| 4.0702              | 0.9002                   | 0.9006                  | 4.6187             | 0.8996                  | 0.9018                 |
| KNN                | 0.0097              | 0.9122                   | 0.8939                  | 6.0595             | 0.9070                  | 0.9001                 |
| Decision Tree      | 0.1419              | 0.9954                   | 0.8413                  | 1.0632             | 0.8997                  | 0.9007                 |
| SVM                | 7.4160              | 0.8975                   | 0.8977                  | 42.6119            | 0.9097                  | 0.9011                 |

---

## Key Observations

- All models **outperform the baseline accuracy** of 0.8874.
- **Logistic Regression**: Stable and slightly improved test performance (+0.0012).
- **KNN**: Test accuracy improved (+0.0062), though training time rose significantly due to tuning.
- **Decision Tree**: Major reduction in overfitting; test accuracy increased by ~0.0594.
- **SVM**: Achieved strong performance but had the largest increase in training time (~6x).

---

## Conclusion

Compared to the baseline, all models provide meaningful predictive performance. After feature engineering and tuning:
- **Generalization improved**, especially for overfit-prone models like the Decision Tree.
- **Test accuracy increased** across the board.
- Trade-offs exist between **training time** and **model performance**, with Logistic Regression offering a good balance.

These improvements demonstrate the power of thoughtful preprocessing and tuning in building reliable ML models.


---

## Feature Insights

We also examined which features were most impactful for predicting subscription outcomes using both a Decision Tree and Logistic Regression model:

| Feature            | DT Importance | LR Coefficient  | Insight                                                                                                                               |
| ------------------ | ------------- | --------------- | ------------------------------------------------------------------------------------------------------------------------------------- |
| `nr.employed`      | 0.728         | +0.652          | More individuals employed → higher success rate |
| `cons.price.idx`   | —             | +1.302      | Higher consumer price index correlates with more subscriptions                          |
| `pdays`            | 0.131         | —               | Whether and when the client was previously contacted matters — recent contact boosts conversions.                                     |
| `cons.conf.idx`    | 0.069         | +0.143          | Higher consumer confidence index → higher likelihood to subscribe.                                                                    |
| `euribor3m`        | 0.021         | +0.278          | When interest rates are high, clients are more likely to lock in term deposits.                                                       |                                                                                 |
| `poutcome_success` | —             | +0.124          | Past successful campaigns lead to more conversions                                                       |
