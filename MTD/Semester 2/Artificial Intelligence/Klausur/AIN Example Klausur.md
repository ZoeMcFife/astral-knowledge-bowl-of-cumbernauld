#artificial_intelligence 

![[Sample_exam.pdf]]

**Question 1 (20 points)** Consider the following classification results (one sample per row):

| y   | g(x) |
| --- | ---- |
| 1   | 1    |
| 1   | 1    |
| -1  | 1    |
| -1  | -1   |
| -1  | 1    |
| 1   | -1   |
| 1   | 1    |
| -1  | -1   |
| -1  | -1   |
| -1  | -1   |

Determine the confusion table and compute the following evaluation measures: accuracy (ACC), sensitivity (TPR), specificity (TNR), balanced accuracy (BACC), precision (PPV), and the Matthews Correlation Coefficient (MCC).

> [!ANSWER]
> 
> **Confusion Matrix**
> 
> ![[Pasted image 20260709141343.png]]
> 
> ACC = 0.7
> TPR = 0.75
> TNR =  0.33
> BACC = 0.54
> PPV = 0.6
> MCC = 10 / sqrt(5 \* 4 \* 6 \* 5) = 0.4

<hr>

**Question 2 (25 points)** Summarize in your own words how to deal with missing values in tabular data sets. Which strategies are available for removal or imputation?

> [!ANSWER]
> There are multiple ways to deal with missing values. 
> 
> **You can:**
> - remove samples
> - remove features
> - imputate data
> 
> *if values are missing*
> 
> This can lead to data loss or bias. 
> 
> Imputation can be done in multiple ways. You can replace missing values with fixed data, the mean or median of a numerical feature, use the most frequent category, add a missing category, or use a prediction algorithm to determine the missing value

<hr>

**Question 3 (5 points)** Suppose you have trained a classifier g and you want to compute the test error for a given test set. What condition must the test data satisfy in order to make sure that the test error is an unbiased estimate of the generalization error (exactly one answer is correct):

- [ ] the test samples must be uniformly distributed
- [ ] the test samples must have the same variance as the training samples
- [x] the test samples must have been drawn independently from the same distribution as the training samples
- [ ] the test samples must be normally distributed
- [ ] the test samples must be shuffled

<hr>

**Question 4 (15 Punkte)** Suppose you have to solve a classification task for a medium-sized tabular data set (approx. 2000 samples, approx. 100 features), where you suspect that many features are irrelevant. Suppose you need a good, robust, and easy solution, and you further need to know which features are contributing most to the classification model. Which classification method would you choose and why? (3–5 sentences)

> [!ANSWER]
> A random forest is the right choice here. It computes feature importance with Mean Gini Impurity Decrease, where it measures the effect of removing a feature on the classification performance. Using this you can easily prune unneeded features.

<hr>

**Question 5 (5 Punkte)** Which activation in the output layer of a neural networks on the left fits to which type of task on the right? Provide a 1:1 assignment.

- A: binary classification
- B: multi-class classification
- C: regression

→ ? →

- I: softmax
- II: linear function
- III: sigmoid

> [!ANSWER]
> A → III
> B → I
> C → II

<hr>

**Question 6 (30 Punkte)** Suppose you had to solve the fish classification task (salmon vs. sea bass) with the methods available today and had at least a four-digit number of labeled sample images available, with each image showing only one fish, and in a relatively large format. How would you go about it? Describe in detail how you would design an automatic classification system. While doing so, focus on the machine learning aspects such as image preprocessing & feature engineering (if relevant), training, and prediction. You need not consider the hardware environment (camera, lighting, recording trigger, transfer of the image to the computer, etc.). (approx. one A4 page)

> [!ANSWER]
> **First Step:**
> Preprocess images. Make sure the images show the fish properly and make sure the images are scaled to the same format. And that they’re properly labled!
> 
> **Second Step:**
> I wouldn’t try to engineer features here. I’d use a CNN network and have it train on the images and learn the features of the fish. This can learn more complex features, other than length or brightness as used in our uni examples. Texture amongst other features can become a factor. 
> 
> **Third Step**
> I would set up a CNN with a few convolutional layers and a some dense layers at the end for the determination of the class. Depending on the resolution of the images, I have to adjust how large the feature maps are and how many layers and neurons I use. Using too many layers and neurons for relatively small images would hurt generalization and overfit. 
> 
> **Foruth Step**
> If i train my CNN model correctly, i’d end up with a system where i can put in an image of the fish, have it undergo the same preprocessing steps as the training data and have it give me a confidence value of either ssalmon or sea bass. 
> 
> For a binary classification task I’d only need one output neuron to determine whether it is a salmon or sea bass. If i’d want to also determine if its neither, i would use two output neurons, one for the probability that its a salmon, the other for sea bass. 