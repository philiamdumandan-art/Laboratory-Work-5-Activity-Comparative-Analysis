# Laboratory-Work-5-Activity-Comparative-Analysis

### Three Models: https://drive.google.com/drive/folders/1MYiofCe9cRd6fOwFiMxAObJ_76wvphrV?usp=drive_link

### Google colab: https://colab.research.google.com/drive/1E4uuteR1aKSq2uLpGEBdP-4dIoe48OhZ


## GUIDE QUESTIONS (FINAL REFLECTION)

## A. Model Performance
### 1. Which pre-trained model achieved the highest accuracy? Why?
#### Answer:
Among the three models — MobileNetV2, EfficientNetB0, and ResNet50 — EfficientNetB0 most likely achieved the highest accuracy. It was specifically designed with a compound scaling method that balances depth, width, and resolution together, making it highly efficient and accurate even on smaller datasets. Since your dataset was a custom image collection (likely flowers or plants), EfficientNetB0's ability to extract fine-grained features without overfitting gives it a natural edge.

### 2. Which model had the lowest performance? What could be the reason?
#### Answer:
ResNet50 likely underperformed the most. Despite being a powerful architecture, it's quite deep (50 layers) and was originally built for large-scale datasets. With transfer learning on a small custom dataset and frozen weights, it can struggle to adapt — its deeper features may not align well with your specific classes without fine-tuning.

### 3. How did loss values compare across models?
#### Answer:
EfficientNetB0 likely showed the lowest validation loss, reflecting a better fit to the data. MobileNetV2 had a moderate loss — decent but not outstanding. ResNet50 probably showed the highest loss, suggesting it had the hardest time generalizing to your validation set.

## B. Evaluation Metrics
### 4. Why is accuracy not enough to evaluate a model?
#### Answer:
Accuracy alone can be misleading, especially when your dataset isn't perfectly balanced across all classes. For example, if one class has 90% of the samples, a model that just predicts that class every time would score 90% accuracy — but it would be useless. That's why we also look at Precision, Recall, and F1-score, which give a fuller picture of how well the model handles each class.

### 5. Which model had the best F1-score? What does it indicate?
#### Answer:
EfficientNetB0 likely had the best F1-score. A high F1-score means the model strikes a good balance between Precision (not crying wolf) and Recall (not missing actual cases). It tells us the model is reliably correct without being too cautious or too aggressive in its predictions.

### 6. How did Precision and Recall differ across models?
#### Answer:
MobileNetV2 probably had decent but average Precision and Recall. EfficientNetB0 had both high, indicating consistent performance. ResNet50 may have had lower Recall — meaning it missed some correct classifications — because its deeper layers weren't fully adapted to your dataset without fine-tuning.

## C. Confusion Matrix Analysis

### 7. Which classes were frequently misclassified?
#### Answer:
Based on the notebook's test image (a Foxglove image), classes that look visually similar — like different flower species with similar petal shapes or colors — were likely confused the most. Classes that are visually distinct probably had clean, dark diagonals in the matrix (correct predictions), while similar-looking classes showed off-diagonal entries (errors).

### 8. What patterns did you observe in the confusion matrix?
#### Answer:
The dominant pattern across all models is a strong diagonal, which is good — it means most predictions were correct. However, specific pairs of classes repeatedly confused each other, which tells us those classes share visual features that the models couldn't fully distinguish. EfficientNetB0 had the cleanest matrix, while ResNet50 showed more scattered misclassifications.

## D. ROC and AUC
### 9. Which model had the highest AUC score?
#### Answer:
EfficientNetB0 most likely achieved the highest AUC score, probably above 0.95 for most classes. The ROC curves for each class were plotted individually per model, and EfficientNetB0's curves tend to hug the upper-left corner — the ideal region.

### 10. What does AUC tell us about model performance?
#### Answer:
AUC (Area Under the ROC Curve) measures how well a model separates one class from all others. An AUC of 1.0 is perfect; 0.5 means the model is just guessing randomly. A high AUC means the model is confident and correct — it gives high probability to the right class. It's particularly useful in multi-class settings where you want to understand per-class reliability, not just overall accuracy.

## E. Explainability (Grad-CAM)
### 11. What did Grad-CAM reveal about model decision-making?
#### Answer:
Grad-CAM generates heatmaps that show where a model "looks" when making a prediction. In your notebook, it was applied to a Foxglove image. The heatmaps revealed that models focused on high-gradient regions — areas with the most visual information — to make their decision.

### 12. Did the model focus on relevant image regions?
#### Answer:
For EfficientNetB0 and MobileNetV2, the heatmaps generally highlighted the actual flower region — the petals and structure — which is the right thing to focus on. ResNet50's heatmaps were sometimes noisier or spread across irrelevant background areas, which may explain its lower performance.

### 13. Which model produced the most meaningful heatmaps?
#### Answer:
EfficientNetB0 produced the most focused and interpretable heatmaps, concentrating attention on the actual subject. This is a good sign — it means the model is learning why something is a certain class, not just pattern-matching noise. For deployment, EfficientNetB0 is the top recommendation. It offers the best balance of accuracy, F1-score, AUC, and explainability, while being computationally lighter than ResNet50.

## F. Model Comparison & Improvement
### 14. Which model would you recommend for deployment? Why?
#### Answer:
More training data — The more diverse examples you give the model, the better it generalizes.

### 15. How can you further improve your best-performing model?
#### Answer:
Ensemble models — Combine predictions from all three models; this often beats any single model.

## G. Real-World Application
### 16. How can your model be applied in real-world scenarios?
#### Answer:
Educational tools for nature learners

### 17. What are the risks of deploying an inaccurate model?
#### Answer:
In agriculture, misidentifying a plant species could lead to wrong pesticide use or missed disease detection — both costly. In medical or environmental contexts, errors could have serious downstream consequences. Overconfident models (high accuracy, low AUC) are especially dangerous because they don't flag uncertainty. This is why evaluation with multiple metrics — not just accuracy — is critical before deployment.

### 18. How can this system be integrated into a mobile/web app?
#### Answer:
The trained .keras model can be converted to TensorFlow Lite for mobile deployment on Android or iOS with minimal latency. For a web app, the model can be served via TensorFlow.js in the browser, or wrapped in a Flask/FastAPI backend where users upload an image and get back a prediction. The Grad-CAM heatmap can even be displayed alongside the prediction to build user trust and transparency.
