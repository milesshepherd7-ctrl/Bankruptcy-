# Bankruptcy-
Results
7 classifiers + 1 tuned Random Forest variant. F1 score is the primary metric — accuracy is misleading here because predicting "Healthy" for everyone gives 96.8% accuracy.

Model	Accuracy	Precision	Recall	F1 Score
Random Forest (Tuned, balanced)	0.9633	0.4559	0.7045	0.5536
Decision Tree	0.9626	0.4103	0.3636	0.3855
Gradient Boosting	0.9692	0.5417	0.2955	0.3824
Logistic Regression	0.9677	0.5000	0.2727	0.3529
KNN (k=3)	0.9685	0.5238	0.2500	0.3385
Random Forest	0.9721	0.7500	0.2045	0.3214
Naive Bayes	0.8724	0.1782	0.8182	0.2927
SVM (linear, balanced)	0.9076	0.1940	0.5909	0.2921
Best GridSearchCV parameters: {class_weight: balanced, max_depth: 10, min_samples_leaf: 4, n_estimators: 200} (CV F1 = 0.413).

Key Findings
Class imbalance is the central challenge (~30:1 healthy-to-bankrupt). Accuracy ≥ 96% on every model is meaningless; recall on the bankrupt class is what matters.
class_weight='balanced' is the single most impactful change — it pushed Random Forest's recall from 0.20 to 0.70 at the cost of dropping precision from 0.75 to 0.46. Net F1 nearly doubled (0.32 → 0.55).
Most predictive features: profitability ratios (ROA, Net Income / Total Assets), leverage indicators (Debt ratio %, Liability / Equity), and liquidity (Net Worth Turnover Rate). The aggregated AvgProfitability and AvgLeverage ranked among the top 20 importances.
Outlier capping at 0.5%/99.5% quantiles improves all models — a few firms have extreme financial-ratio values that destabilise tree splits and SVMs.
Naive Bayes traded precision for the highest raw recall (0.82) — useful if the cost of a missed bankruptcy is far higher than a false alarm (e.g. a credit officer triaging firms for closer review).
Linear models (Logistic Regression, linear SVM) struggle here — financial-ratio interactions are non-linear, and tree ensembles dominate.
