


# 💳 PROJECT: High-Recall Credit Card Fraud Detection
## 1. 🎯 Core Problem and Business Goal
Credit card fraud inflicts substantial damage across the financial ecosystem, impacting institutions, merchants (via chargebacks ), and customers (through inconvenience and lost trust ).


The project's overarching objective is a delicate balance : to aggressively minimize financial losses (high Recall) while ensuring maximal customer satisfaction by preventing unnecessary declines of legitimate transactions (minimizing False Positives ).


Crucial Insight: Due to the extreme class imbalance (only ≈0.17% fraud rate ), traditional Accuracy is a useless metric. Model evaluation must prioritize Recall and Precision.


## 2. 📊 Exploratory Data Analysis (EDA) & Data Challenge

Extreme Imbalance: The dataset confirms a severe class imbalance, with only about 492 fraudulent transactions out of ≈285,000 total transactions. This necessitated a specific modeling approach.



Feature Scaling: The non-PCA features, 'Time' and 'Amount', were scaled using StandardScaler to be compatible with distance-based algorithms.


Time & Amount Insights: Initial analysis showed fraudulent transactions tend to cluster around smaller transaction amounts and are more uniformly spread across time, suggesting fraudsters are not constrained by typical business hours.


## 3. ✨ Data Preprocessing and Leakage Guardrail
This section outlines the critical steps taken to prepare the data while preventing a common modeling error: Data Leakage.


Split First: Data was split into 80% Training and 20% Testing sets before any sampling. The split was stratified to maintain the original fraud proportion in both sets.




Imbalance Handling (SMOTE): The Synthetic Minority Over-sampling Technique (SMOTE) was applied exclusively to the training data (X_train). This provided the model with enough synthetic examples of fraud to learn patterns, preventing leakage into the final test set.






Baseline Model: An Isolation Forest model was established as an unsupervised anomaly detection baseline for comparison against supervised models.

## 4. 🤖 Model Training, Selection, & Refinement
Models were trained on the SMOTE-balanced data and evaluated solely on the untouched, original Test Set (X_test, y_test).

Model	Type	Training Data	Evaluation Metric Focus
Isolation Forest	Unsupervised (Anomaly)	Original Training Data	
Baseline for Recall/Precision 

Random Forest	Supervised (Ensemble)	
SMOTE-Balanced Training Data 

Robust performance comparison
XGBoost	Supervised (Boosting)	
SMOTE-Balanced Training Data 

Highest performance potential

Final Selection Rationale: XGBoost was selected as the final model due to its superior performance on structured data. It was then fine-tuned using RandomizedSearchCV with the primary scoring metric set to Recall.





## 5. 💡 Feature Interpretation & Future Work

Feature Importance: Analysis revealed that several anonymized V-features (e.g., V14,V17,V10) were the most critical statistical predictors of fraud, driving the model's decision-making process.


Final Key Result: The Tuned XGBoost Model achieved a Recall of 83.1%. 

Future Work:


Deployment: Model could be deployed via Flask or FastAPI to provide real-time predictions to a payment gateway with low latency.



Concept Drift: A strategy for continuous monitoring and retraining is essential to adapt to the natural evolution of fraud tactics.



Cost-Sensitive Learning: Implement a loss function that explicitly weights False Negatives (missed fraud) more heavily to directly minimize the financial cost.
