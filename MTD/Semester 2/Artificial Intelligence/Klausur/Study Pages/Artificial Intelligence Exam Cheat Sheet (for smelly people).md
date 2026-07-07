# Overview

![[IMG_5393.png]]
## What is AI

- environment aware
- performs intelligent actions in a goal oriented matter

## Terminology

- Artificial Intelligence
	- Machine Learning
		- Deep Learning

**Strong AI**
- more general capabilities
- similar / superior to humans

**Weak / Narrow AI**
- made to solve a specific task

<hr>

# ML Basics

![[deer_in_space_no_top.png]]
## Explicit Models

- to know *how* and *why* things work 
- find a solution to new problems *deductively*
- for some tasks to computationally expensive or complex
## Inductive Learning in Machine Learning

- using previous data to
	- get insights
	- predict future
## Fish Detection

*since this is on the example exam, I’m going a bit deeper in the fish*

![[ironcad.png|222]]

**Goal — Sea Bass / Salmon Detection**
### *Our* Fish 

![[Pasted image 20260707161828.png|541]]

### Basic Workflow

1. Preprocessing
	- contrast, brightness correctoin
	- segmentation
	- alignment
2. Features
	- Length
	- Brightness

![[Pasted image 20260707162209.png|314]]

#### Determining a good feature

Sometimes features are just pointless…

**Using bar graphs:**

![[Pasted image 20260707162544.png]]

**Linear Separation**

![[Pasted image 20260707162609.png|224]]

**Highly Non-Linear Separation**

![[Pasted image 20260707162628.png|226]]

## Supervised vs Unsupervised ML

### Unsupervised

- identifying patterns in unlabeled data
- **Target is unknown**

**Projection Methods** 
down-projection of data to lower-dimensional space in order to concentrate on the essence of the data 

**Clustering**
grouping of similar data items 

**Biclustering** 
simultaneous grouping of samples and features 

**Generative model** 
building a model that produces data that is distributed the same as the observed data
### Supervised

- identifying relationships from input and target data
- **Targets are known**

**Classification**
target is a class label

**Regression**
target is a numeric value
## Misc. Terminology

![[ScreenShot-2024-11-15_19-21-13-A3E.jpg|418]]

**Reinforcement Learning** 
learning by feeback from the environment in an online process 

**Feature Extraction**
computation of features from data prior to machine learning (e.g. signal / image processing) 

**Feature Selection**
selection of those features that are relevant and sufficient to solve a given learning task 

**Feature construction**
construction of new features as part of the learning process

**Model** 
the specific relationship or representation we are aiming at 

**Model Class**
the class of models in which we search for the model 

**Parameters** 
representations of concrete models inside the given model class 

**Model Selection/Training** 
process of finding that model from the model class that fits/explains the observed data in the best way 

**Hyperparameters** 
parameters controlling the model complexity or the training procedure
## Data Analysis Workflow

![[Pasted image 20260707165639.png|584]]
## Model Selection

- **Model class** 
- **Objective**
- **Optimization algorithm**

<hr>

# Preprocessing
![[Mina1_glasses.png|298]]

**Categorical Features**
finite set of labels

**Numerical Features**
numerical values

## Basic Steps in Data

- filtering / removing samples
- filtering / removing features
- create new features
- transform features
- dealing with missing values
- analyze and visualize data
## Statistics

- Percentage of missing values
- **numerical:** min, max, mean, median, variance, std, quartiles
- **categorical:** num / % of categories
## Visualization of Data

**Single categorical feature:** 
bar chart (recommended) or pie chart (not recommended) 

**Single numerical feature:**
histogram 

**Numerical feature vs. categorical feature:** 
set of histograms (possibly overlayed) or box(-and-whisker) plot 

**Two categorical features:** 
heatmap or two-dimensional bar chart 

**Two numerical features:** 
scatter plot 

**Three numerical features:** 
3D scatter plot (hopefully rotatable) 

**Two numerical features vs. categorical feature:** 
color-labeled scatter plot 

**Three numerical features vs. categorical feature:** 
color-labeled 3D scatter plot (hopefully rotatable)

## Transformations

**Scaling**
\[0, 1] — \[-1, +1]

**Scale to mean 0 and variance 1**

**Removing outliers**

**Log transform**
## Feature Construction

Creating a **new feature** out of **existing** features

e.g.: *relative values of smth*
## PCA — Principal Component Analysis

- rotates normal vector data into linear uncorrelated **principal components**
- orthogonal transformation
- mutually uncorrelated, centered, ordered desc. based on variance
- also used for dimensionality reduction → *low variance components can be removed*

![[Pasted image 20260707171721.png]]
*before — after*

## Ordinal Feature as Numerical

Ordered ordinal feature *(like grades)* can be converted to numeric values.
## One-Hot Encoding

having each class be a feature that’s either 0 or 1

![[Pasted image 20260707172056.png]]

## Missing Values

- remove samples with missing values
- remove features with missing values
- imputation

> [!WARNING]
> data loss
> bias

### Imputation

**Numerical**

*replace with:*
- fixed value
- mean / median
- prediction

**Categorical**

*replace with:*
- fixed value
- most frequent
- missing category
- prediction

<hr>


# Supervised Machine Learning

![[kitcozy-g.png]]
## Inputs

X is just input
Y thingy is collection of all targets
and the individual Xs are data

![[Pasted image 20260707172820.png]]
## Classification / Regression

**Classification**
targets are categories
binary classification → positive / negative class

**Regression**
target is a number
## Generalization Performance

p(x, y)

Generalization error / risk is the **expected error on future data**
### Loss Function

maps inputs to predicted outputs
### Estimating Generalization Error

→ distribution of future data is needed

- Test sets
	- Data is split into training and test splits 
	- 20/80 method
- Cross validation
	- k folds 
### K-Fold Cross Validation

![[Pasted image 20260707173704.png]]
### Confusion Matrix

- TRUE POSITIVE
- TRUE NEGATIVE
- FALSE POSITIVE
- FALSE NEGATIVE

![[Pasted image 20260707173829.png|441]]
## Evaluation Measures *Categorical*
### Basic Metrics

- **Accuracy (ACC)**  
  Proportion of correctly classified items.
  $$
  ACC = \frac{TP + TN}{TP + FN + FP + TN}
  $$

- **True Positive Rate (TPR)** *(Recall / Sensitivity)*  
  Proportion of actual positive examples that were correctly classified.
  $$
  TPR = \frac{TP}{TP + FN}
  $$

- **False Positive Rate (FPR)**  
  Proportion of actual negative examples that were incorrectly classified as positive.
  $$
  FPR = \frac{FP}{FP + TN}
  $$

- **Positive Predictive Value (PPV)** *(Precision)*  
  Proportion of predicted positive examples that are actually positive.
  $$
  PPV = \frac{TP}{TP + FP}
  $$

- **True Negative Rate (TNR)** *(Specificity)*  
  Proportion of actual negative examples that were correctly classified.
  $$
  TNR = \frac{TN}{FP + TN}
  $$

- **False Negative Rate (FNR)**  
  Proportion of actual positive examples that were incorrectly classified as negative.
  $$
  FNR = \frac{FN}{TP + FN}
  $$
### Evaluation Measures for Unbalanced Data

- **Balanced Accuracy (BACC)**  
  Mean of the true positive rate and true negative rate.

$$
  BACC = \frac{TPR + TNR}{2}
  $$

- **Matthews Correlation Coefficient (MCC)**  
  Measures the quality of binary classifications by considering all four entries of the confusion matrix.
  $$
  MCC =
  \frac{TP \cdot TN - FP \cdot FN}
  {\sqrt{(TP + FP)(TP + FN)(TN + FP)(TN + FN)}}
  $$

- **F1 Score**  
  Harmonic mean of precision and recall.
  $$
  F_1 = \frac{2 \cdot PPV \cdot TPR}{PPV + TPR}
  $$

![[cliff.png|402]]

## Confusion Matric for Multi-Class

![[Pasted image 20260707174453.png|483]]

**Accuracy**

$$  
ACC =  
\frac{\sum_{i=1}^{k} C_{ii}}  
{\sum_{i,j=1}^{k} C_{ij}}  
=  
\frac{1}{M}  
\sum_{i=1}^{k} C_{ii}  
$$
**Balanced Accuracy**

$$  
ACC =  
\frac{1}{l}  
\sum_{i=1}^{l} TPR_i  
$$
*i don’t think we need these*
## ROC Curves

“Receiver-Operator Characteristic”

→ plotting TPR vs FPR

Area under curve used as to rank performance

![[Pasted image 20260707174958.png|319]]
![[Pasted image 20260707175023.png|313]]

*Think these are kinda useless too… they’ve been kinda confusing me since HTL lmao*
## Evaluation Measures *Regression*

**Mean Squared Error (MSE)**  
Average of the squared differences between predicted and actual values.  
  
$$  
MSE = \frac{1}{l}\sum_{i=1}^{l}\left(g(\vec{x}_i)-y_i\right)^2  
$$
  
**Root Mean Squared Error (RMSE)**  
Square root of the Mean Squared Error.  
  
$$  
RMSE = \sqrt{MSE}  
$$
  
**Mean Absolute Error (MAE)**  *(Mae stands for Mae Borowski)*
Average of the absolute differences between predicted and actual values.  
  
$$  
MAE = \frac{1}{l}\sum_{i=1}^{l}\left|g(\vec{x}_i)-y_i\right|  
$$

**Mean Absolute Percentage Error (MAPE)**  
Average percentage difference between predicted and actual values.

  $$
  MAPE =
  \frac{100}{l}
  \sum_{i=1}^{l}
  \left|
  \frac{g(\vec{x}_i)-y_i}{y_i}
  \right|
  $$

**Coefficient of Determination ($R^2$)**  
Measures the proportion of the variance in the target variable that is explained by the model.
  $$
  R^2 = 1 - \frac{SS_{res}}{SS_{tot}}
  $$
  where
  $$
  SS_{res} =
  \sum_{i=1}^{l}
  \left(g(\vec{x}_i)-y_i\right)^2
  $$

  $$
  SS_{tot} =
  \sum_{i=1}^{l}
  \left(y_i-\bar{y}\right)^2
  $$
  and the mean target value is
  $$
  \bar{y} =
  \frac{1}{l}
  \sum_{i=1}^{l} y_i
  $$
## Fitting

### Underfitting

Model performs bad with training **and** test data.

→ high bias

**Solution**
- make model more complex
- better features
- stop sucking at this
### Overfitting

Model performs well on training data **but not on** test data.

→ high variance

**Solution**
- make model less complex

![[Pasted image 20260707175800.png]]
## Hyperparameter Optimization

### Evaluation

- **Three split validation**
	- training set
	- validation set
	- test set for HP
- **Nested Cross Validation**
	- inner CV loop → HP optimization
	- outer loop → generalization performance
- **Hybrid Approaches**
### Optimization

- **Grid Search**
	- test out all values in a grid like fashion
- **Random Search**
	- throw shit at the wall
- **Bayesian optimization**
- **Gradient Methods**
- **Evolution**
- **Dinosaurs** (DONT THINK ABOUT SEX)

![[unknown.png|341]]

<hr>


# Classifiers
## K-Nearest Neighbor Classifier

![[Pasted image 20260707180528.png|319]]

Basically, if K of the nearest neighbors are type A, I must be type A too.
## Linear Regression

![[Pasted image 20260707180700.png|405]]

Just like you learned in math class.
## Polynomial Regression

![[Pasted image 20260707180732.png|416]]

## Support Vector Machine

Goal → maximize margin between positive and negative samples

![[Pasted image 20260707180847.png|564]]

**Kernel**
similarity measure for inputs

- Linear
- Polynomial
- Gaussian / RBF
- Sigmoid

![[Pasted image 20260707181028.png|564]]

SVMs can only do binary classification. To use on multi-class problems, you have to convert it into multiple binary classification problems.

**Approaches**
- One vs All — SVM is trained against one class vs all other classes. Sample are assigned based off the SVM with the highest discriminant value.
- Pairwise — SVM are trained in pairs of classes. Samples are assigned via voting – like in **among us.** 

![[Pasted image 20260707181342.png|399]]
## Decision Trees

- if statements
- data partitioned into a hierarchy 
- can be used for classification and regression
### Learning

- **Splitting Criteria** — when do da if
- **Stopping Criteria** — when to stop a tree from growing
- **Pruning** — when to prune deep sub-trees

![[Pasted image 20260707181824.png|531]]
### Random Forest

- **CART Trees** (chai tea)
	- Gini impurity gain — Classification
	- Variance reduction — Regression
- **Each tree gets random samples**
- **Splits consider only a sub-sample of features**
- **No pruning**
- **Ensembles**
	- Bagging
#### Out of Bag Estimates

- generalization performance only on training data
- for each sample — error can be computed by considering trees that haven’t use a this sample in their training sub-sample
- overall OoB error is the average of all OoB errors of all samples
#### Feature Importance

**Mean Gini Impurity Decrease**
*for all features, average the Gini impurity gains of all splits in all trees that involve this feature*

- OoB error for all samples
- for every feature → random permutations and compute OoB error for the data set again, but with the permuted feature
- importance score → averages differences of before / after permutation
### Ensembles

#### Bagging

Trees get trained on random subsets of the dataset independendly.
![[Pasted image 20260707183452.png|543]]
#### Boosting

Trees get trained sequentially and improved upon.

![[Pasted image 20260707183511.png|548]]
#### Gradient Tree Boosting

math stuff

improvement comes from using gradient descent on the loss function

good for shallow trees

![[Untitled-1.png|501]]

<hr>
