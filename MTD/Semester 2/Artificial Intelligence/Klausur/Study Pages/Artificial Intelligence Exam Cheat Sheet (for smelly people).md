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

![[deer_in_space_no_top.png|697]]
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
> **Following can occur:**
> - data loss
> - bias
> - the scringler
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


# Neural Networks and Deep Learning

![[IMG_2155.jpg|444]]
## Feed Forward Neural Networks

- Input / Output systems
- no feedback loops
## Perceptrons

![[Pasted image 20260708114854.png|208]]

- **linear threshold unit**

![[Pasted image 20260708114939.png|437]]
### Perceptron — Logic

You can model logic gates with perceptrons. 

![[Pasted image 20260708115106.png]]
### Perceptron — Learning Algorithm

whatever the fuck this means
![[Pasted image 20260708115411.png]]
### Linear Separability

If a dataset is linearly separable, the learning algorithm terminates and gives one solution.

Non-Linearly separable problems aren’t possible with this learning algorithm.

**Example:** XOR

![[Pasted image 20260708115320.png]]
### Multi-Layer Perceptrons

![[Pasted image 20260708115949.png|426]]

Old history people thought this wasn’t possible. But it was. 
#### Continuous Activation Functions

Discontinuous threshold was replaced by a **differentiable function $\varphi$**

$$ g(\vec{x}; \vec{w}) = a = \varphi \left( w_0 + \sum_{j=1}^{d} w_j \cdot x_j \right) $$
![[Pasted image 20260708120356.png|547]]

##### Examples

- **Linear** — simple choice for regression
- **Sigmoid** — most common choice for **0 / 1** outputs
- **Hyperbolic Tangent** — for **-1 / +1** data
## Logistic Regress and Cross Entropy

Suppose we have a perceptron with sigmoid activation function $\varphi$. Then the output
$$
g(\vec{x}_i; \vec{w})
$$
can be interpreted as an estimate of the probability that $\vec{x}_i$ belongs to the positive class:
$$
p(y = 1 \mid \vec{x}_i) = a = \varphi(\text{net})
$$
and
$$
p(y = 0 \mid \vec{x}_i) = 1 - a = 1 - \varphi(\text{net})
$$
We can unify these two formulas as follows to get a single formula for the likelihood $p(y = y_i \mid \vec{x}_i; \vec{w})$:
$$
p(y = y_i \mid \vec{x}_i; \vec{w}) =
\varphi(\text{net})^{y_i}
\cdot
\left(1 - \varphi(\text{net})\right)^{(1-y_i)}
=
a^{y_i}
\cdot
(1 - a)^{(1-y_i)}
$$

**Maximizing likelihood** → **minimizing – 1** \* its **natural log**
$$
-\ln p(y = y_i \mid \vec{x}_i; \vec{w})
=
-y_i \cdot \ln(a)
-
(1 - y_i) \cdot \ln(1 - a)
$$
## Training Algorithm — Online

1. have a dataset
2. for all samples
	1. compute $net$, $a$, $\delta$ 
	2. update weights
3. go to step 2 if stopping condition isn’t
4. output vector of weights
## Training Algorithm — Batch

1. have a data set
2. set delta weights to 0?
3. for all samples
	1. compute $net$, $a$, $\delta$ 
	2. $\Delta \vec{w}:=\Delta \vec{w}-\eta \cdot \delta \cdot\begin{pmatrix}1 \\\vec{x}_i\end{pmatrix}$ ????
4. update weights + delta weights
5. go to step 2 if stopping condition isn’t
6. output vector of weights
## Delta Term

1. If we use a sigmoid activation function along with the cross entropy loss (this combination corresponds to so-called *logistic regression*), the delta term is defined as (for a given sample $(\vec{x}_i, y_i)$)
$$
\delta = a - y_i
$$
2. If we use the identity activation $\phi(x) = x$ along with the quadratic loss (this combination corresponds to classical *linear regression*), the delta term is again given as (for a given sample $(\vec{x}_i, y_i)$)
$$
\delta = a - y_i
$$
*copied from slides, dunno what any of this even means tbh*
## Training with Differentiable Activation

- Training passes are called **epochs**
- **Online Learning**
	- weights are updated for each sample individually
	- one gradient descent step per sample
	- random order → avoiding bias
- **Batch Learning**
	- updates are summed up for all samples before weights get updated
- **Mini Batches**
	- common to use batch train on sampled batches
	- Stochastic Gradient Descent (SGD)

## Multi-Layer Perceptron 2 — Electric Boogaloo

![[Pasted image 20260708123542.png|481]]

idk lots of math crap
## Forward Propagation

Inputs gets propagated through the network. 

![[Pasted image 20260708124048.png|659]]
## Backpropagation

![[Pasted image 20260708124453.png|657]]
## Delta Term

- The deltas of the N − 1-st layer are given as the derivative of the activation function times a weighted sum of deltas of the N-th layer, i.e. the deltas are propagated back through the network.
- In the same way, the deltas are propagated back to the N − 2-nd layer and so forth.
## Backpropagation — Summary

### Forward Pass

Set activation of input layer to input vector. For each layer (from first hidden layer to output layer), compute net input as product of activation with weight matrix and apply activation function to net input for each unit.
### Backward Pass

Perform forward pass and compute deltas for output layer. For each layer (from last hidden layer to first hidden layer), compute deltas as elementwise products of the activations’ derivatives and the product of the transpose of the weight matrix times the deltas of the next layer (the one ‘‘to the right’’). The weight updates are outer products of the deltas of the ‘‘right layer’’ and the activations of the ‘‘left layer’’.
## Multi-Layer Perceptron — Classification

**Binary Classification** – 1 output neuron
**Multi-Class** — 1 neuron per class

**Softmax** activation is commonly used for multi-class problems.
## Multi-Layer Perceptron — Regression

1. **Scaling** — scale output vectors to \[0, 1]
2. **Linear Neurons in Output**

**Quadratic Loss** function is common for loss.
## Autoencoders

- No targets
- trained to produce input as output
- inputs can be noisy → learning to recover original from noisy data. *(denoising autoencoder)*
## Practical Considerations

- **Input Scaling**
	- inputs standardized to \[-1, 1]
	- mean 0, variance 1
- **Initial Weights**
	- small random uniformly distributed values \[–0.1 - 0.1]
- **Number of hidden layers**
	- usually 2 are sufficient
	- more for difficult tasks
- **Number of hidden units**
	- too few → underfitting
	- too many → overfitting
- **Learning Rates**
	- low for online
	- higher for batch
	- adaptive
- **Online vs Batch**
	- online converges faster if learning rate is good
	- mini batches is good in between
- **Momentum**
	- augment updates with previous update to avoid oscillations
- **Stopping Criteria**
	- epochs reached
	- error below threshold
	- improvement under threshold
	- maximum weight change below threshold
## Regularization

Neural networks have no built measures against overfitting. That’s why this stuff exists:

- **Early Stopping**
	- quit learning when right model complexity is reached
- **Training with Noise**
	- add noise to inputs
- **Weight Decay**
	- pushes weight matrix towards 0 

A moment of silence for Zoe’s sex life. 
## The Vanishing Gradient Problem

Magnitude of deltas decrease exponentially layer by layer. Backpropagation is not usable for deep networks.

![[Pasted image 20260708131517.png]]
## Deep Learning

Deep learning rests on three pillars:

- **New Architectures** and methods
- **Big Data Sets**
- **Many Resources**

Two step procedure:

1. **Pre-training** — representations are learned layer by layer
2. **Fine-tuning** — makes predictions from the last layer of a pre-trained network. Usually only one extra layer at the end
### Pre-Training Methods

- **Restricted Boltzmann Machine** — stochastic neural network with one input / output layer with a hidden layer with symmetric weights.
- **Autoencoders** — an autoencoder is trained on each step. Output layer is discarded and only hidden layer remains
- **Supervised pre-training** — training with one hidden layer , output layer discarded, and new a layer is trained with the last hidden layer as outputs
## Representations

**Meaningful representation:**
- hidden units correspond to specific patterns
- hidden layers correspond to level of abstractions
- different units, different patterns — disentangling
### Sparse Representations

- **Dropout** — Activations randomly set to 0
- **Rectified linear units (ReLU)** — gives 0 below a certain threshold
## Applications of Deep Learning

- Computer Vision
- Language Processing
- Generation of Data
- other stuff

![[zoe.png|262]]

<hr>


# Convolutional Neural Nets

![[VRChat_2024-10-03_21-23-02.998_3840x2160.png|369]]

used for larger complex images
## Architecture

- **Convolutional Layers**
	- consist of units that operate on small image patches
	- units correspond to one simple feature of a patch
	- referred to as **Filters**
	- all units run over all patches → creates a **feature map**
	- stackable
	
![[Pasted image 20260708135520.png|446]]

- **Batch Normalization**
	- normalizes outputs of the previous layer
- **Pooling**
	- down-samples feature maps by local max pooling
### Types

- **Standard**
	- last convolutional layer gets flattened and connected to dense layers
	- useful for categorical or numerical outputs
- **Fully Convolutional**
	- used when you need an image as output
## Training

- Dense layers are trained like usual
- feature maps only have one set of weight → shared by other feature maps
	- weight sharing
- max pooling layer → Error propagated to the input of the maximal activation

**Convolutional layer outputs**

![[Pasted image 20260709120411.png]]
## Data Augmentation

- translating, rotating, etc. images makes model generalize better
	- → learns to recognize images even if they’re imperfect

look at those kitlers! 

![[Pasted image 20260709120724.png|472]]
## ImageNET

- large annotated image database
## Using pre-existing Models

- common approach: 
	- use an existing model trained on vast amounts of data
	- chop off output layer and add new layers onto that
	- train with new specific data (fine tuning)
## Recognition Based CNNs

- **Regions of Interest** (ROI)
	- bounding box
	- ![[Pasted image 20260709121403.png|548]]
- R-CNN using SVM are slow

- **Fast R-CNN** — use CNN to extract RoIs from feature maps
- **Faster R-CNN** — uses integrated CNN for RoIs
## YOLO 

- Current best model tbh
- assigns class probability to image patches
	- ![[Pasted image 20260709121823.png|532]]

<hr>


# Further Topics

![[f2h.png|563]]
## Time Series Analysis with ANNs

Feedforward neural networks need vectorial inputs → cannot be applied to time series or sequence data.

**Options:**
- Sliding windows
	- Problem → no learning across windows
## Recurrent Neural Nets

- Network has connection cycles
- activation of previous windows are used as inputs for the next time step
- backpropagation can be used

RNNs are prone to **vanishing gradient problem**. Only short times between input and output can be learned.
## Long Short-Term Memory

**LSTM Memory cells**
- linear self connected memory unit → linear activation avoids vanishing gradients
- multiplicative input gate → prevents irrelevant inputs
- multiplicative output → protects outputs from irrelevant memory

![[Pasted image 20260709133021.png|426]]

- Forget gates exist too
## LSTM Networks

1. emit output for each time step or with delay *(for forecasting)*
2. emit after sequence *(classification)*
3. Combination
	1. Encoder — emits outputs after processing a sequence
	2. Decoder — takes output of encoder and turns into a sequence

![[Pasted image 20260709133511.png|509]]
## Using LSTM for Words

- Token-wise
### Word Embeddings

**Maps a word to a vector. Existing Algorithms:**
- Global Vectors for Word Representation
- Word2Vec
- Bidirectional Encoder Representations from Transformers (BERT)
## GANs — General Adversarial Networks 

- **Discriminator**
	- distinguishes real and artificial data
	- binary output
- **Generator**
	- tries to recreate data
- These two fight each other.
### Mode Collapse

Happens when the generator only creates samples of a certain sub-group.
### Applications

- Image Generators
- Audio
- Text


<hr>


# IM FREE

![[MinaKit.png|408]]
