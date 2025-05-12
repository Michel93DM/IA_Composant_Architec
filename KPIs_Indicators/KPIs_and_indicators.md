# KPIs for Renault Welding Quality Prediction using ML

In this section, i list a number of KPIs and indicators that suit the architectur i used and its necessary to assure a minimum of robustness, Xplainability and Performance of the AI Component.

Finally, i list improvements that could be applied for a perfect automated component and end-to-end procedure. 

## 1. **Explainability KPI**
   - **Description**: This KPI evaluates the interpretability and explainability of the machine learning model's decisions, particularly how easily stakeholders can understand why the model predicted a certain welding quality.
   - **Example**: The use of SHAP (Shapley Additive Explanations) values or LIME (Local Interpretable Model-agnostic Explanations) to explain why the model classifies a weld as defective.
   - **Metadata**: 
     - **Parameters**: Shapley values, LIME explanations, Feature importance scores.
     - **Dimension**: Percentage of predictions with interpretable explanations.
   - **Objective**: Ensure that the model’s decisions can be explained in a way that is understandable by engineers, quality controllers, or non-technical users.
   - **Phase**: Evaluation, Operational phase.
   - **Sample KPI Expected**: 95% of predictions have an interpretable explanation.

---

## 2. **Robustness KPI**
   - **Description**: This KPI measures the model's ability to maintain high accuracy despite variations in input data (e.g., different welding conditions, equipment, material types). A robust model should perform consistently in the face of noise or shifts in input features.
   - **Example**: The model's performance under various noisy data scenarios, such as slight variations in welding speed, voltage, or material quality.
   - **Metadata**: 
     - **Parameters**: Model accuracy under different data perturbations, sensitivity to input changes.
     - **Dimension**: Stability of model performance (e.g., percentage change in accuracy when noise is added).
   - **Objective**: Ensure the model provides consistent predictions, even when environmental or operational conditions change.
   - **Phase**: Training, Evaluation, Operational phase.
   - **Sample KPI Expected**: 95% accuracy in predictions when input data is perturbed by noise (e.g., ±5% change in parameters).

---

## 3. **Out-of-Distribution Detection (OODD) KPI**
   - **Description**: This KPI tracks how well the model can detect when new data does not match the distribution of the training data (out-of-distribution). It helps to prevent the model from making unreliable predictions on unseen or unexpected data.
   - **Example**: The model identifies and rejects inputs such as novel welding materials or extreme welding conditions that it has not encountered during training.
   - **Metadata**: 
     - **Parameters**: Confidence score, likelihood of being out-of-distribution.
     - **Dimension**: Percentage of out-of-distribution data identified.
   - **Objective**: Enable the model to correctly identify and reject out-of-distribution inputs, ensuring reliability in production.
   - **Phase**: Evaluation, Operational phase.
   - **Sample KPI Expected**: 98% of out-of-distribution samples are flagged correctly.

---

## 4. **Uncertainty Quantification (UQ) KPI**
   - **Description**: This KPI measures how well the model quantifies its uncertainty in predictions. In the context of welding quality, it would evaluate how confident the model is in its predictions, especially for edge cases or ambiguous welds.
   - **Example**: For a weld with high variability in data (e.g., material properties), the model provides an uncertainty score (e.g., 80% confidence), indicating how certain or uncertain the prediction is.
   - **Metadata**: 
     - **Parameters**: Confidence intervals, uncertainty score (e.g., variance or entropy in predictions).
     - **Dimension**: Percentage uncertainty (e.g., 95% confidence interval for predictions).
   - **Objective**: Quantify how much trust can be placed in each prediction, especially in cases of ambiguous or noisy data.
   - **Phase**: Evaluation, Operational phase.
   - **Sample KPI Expected**: 90% of predictions are made with at least 80% confidence.

---

## 5. **Recall KPI**
   - **Description**: This KPI measures the model’s ability to correctly identify defective welds (true positives) among all actual defective welds. It’s critical in ensuring that no defective welds are missed, which could lead to quality issues.
   - **Example**: The model predicts a welding defect, and recall measures how many of the actual defective welds were correctly identified.
   - **Metadata**:
     - **Parameters**: True positives, False negatives.
     - **Dimension**: Ratio (0-1).
   - **Objective**: Maximize recall to avoid missing defective welds, thereby improving quality control and reducing rework or failures.
   - **Phase**: Evaluation, Operational phase.
   - **Sample KPI Expected**: Recall = 0.92 (92% of defective welds are identified).

---

## Summary of KPIs

| KPI                             | Description                                               | Objective                                      | Sample Expected Value          | Phase(s)                    |
|----------------------------------|-----------------------------------------------------------|------------------------------------------------|---------------------------------|-----------------------------|
| **Explainability**               | Measures the model’s interpretability (e.g., SHAP, LIME).  | Make model decisions understandable.           | 95% of predictions explained    | Evaluation, Operational     |
| **Robustness**                   | Measures model stability under varying conditions.        | Ensure consistent performance despite changes.  | 95% accuracy under noise        | Training, Evaluation, Operational |
| **OODD (Out-of-Distribution Detection)** | Detects when input data is out of distribution.         | Prevent unreliable predictions on novel data.  | 98% correct OODD identification | Evaluation, Operational     |
| **Uncertainty Quantification (UQ)** | Quantifies the model's confidence in its predictions.    | Improve decision-making with uncertainty scores.| 90% predictions with 80% confidence | Evaluation, Operational     |
| **Recall**                       | Measures the model's ability to identify true positives (defects). | Maximize defect detection.                   | Recall = 0.92                   | Evaluation, Operational     |


## Future Improvements and Recommendations

To further enhance this architecture, the following features are recommended:

- **Semi-automatic Labeling**: Incorporate active learning or weak supervision to reduce labeling time.
- **Model Selection Automation**: Introduce automated model benchmarking and selection tools.
- **Drift Detection**: Add modules for automatically detecting data or concept drift over time.
- **Explainability Reports**: Generate user-friendly reports for non-technical stakeholders.
- **Visualization Dashboard**: Deploy a monitoring interface for real-time results and decision support.
- **Active Learning Integration**: Use high-uncertainty or OOD predictions to guide further training data collection.

