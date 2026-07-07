#artificial_intelligence 

![[Sample_exam.pdf]]

**Question 1 (20 points)** Consider the following classification results (one sample per row):

| y  | g(x) |
|----|------|
| 1  | 1    |
| 1  | 1    |
| -1 | 1    |
| -1 | -1   |
| -1 | 1    |
| 1  | -1   |
| 1  | 1    |
| -1 | -1   |
| -1 | -1   |
| -1 | -1   |

Determine the confusion table and compute the following evaluation measures: accuracy (ACC), sensitivity (TPR), specificity (TNR), balanced accuracy (BACC), precision (PPV), and the Matthews Correlation Coefficient (MCC).

<hr>

**Question 2 (25 points)** Summarize in your own words how to deal with missing values in tabular data sets. Which strategies are available for removal or imputation? (approx. 1/2 A4 page)

<hr>

**Question 3 (5 points)** Suppose you have trained a classifier g and you want to compute the test error for a given test set. What condition must the test data satisfy in order to make sure that the test error is an unbiased estimate of the generalization error (exactly one answer is correct):

- [ ] the test samples must be uniformly distributed
- [ ] the test samples must have the same variance as the training samples
- [ ] the test samples must have been drawn independently from the same distribution as the training samples
- [ ] the test samples must be normally distributed
- [ ] the test samples must be shuffled

<hr>

**Question 4 (15 Punkte)** Suppose you have to solve a classification task for a medium-sized tabular data set (approx. 2000 samples, approx. 100 features), where you suspect that many features are irrelevant. Suppose you need a good, robust, and easy solution, and you further need to know which features are contributing most to the classification model. Which classification method would you choose and why? (3–5 sentences)

<hr>

**Question 5 (5 Punkte)** Which activation in the output layer of a neural networks on the left fits to which type of task on the right? Provide a 1:1 assignment.

- A: binary classification
- B: multi-class classification
- C: regression

→ ? →

- I: softmax
- II: linear function
- III: sigmoid

<hr>

**Question 6 (30 Punkte)** Suppose you had to solve the fish classification task (salmon vs. sea bass) with the methods available today and had at least a four-digit number of labeled sample images available, with each image showing only one fish, and in a relatively large format. How would you go about it? Describe in detail how you would design an automatic classification system. While doing so, focus on the machine learning aspects such as image preprocessing & feature engineering (if relevant), training, and prediction. You need not consider the hardware environment (camera, lighting, recording trigger, transfer of the image to the computer, etc.). (approx. one A4 page)