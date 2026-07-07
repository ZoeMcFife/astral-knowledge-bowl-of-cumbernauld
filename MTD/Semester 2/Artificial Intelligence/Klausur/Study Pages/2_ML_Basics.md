#artificial_intelligence 

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

![[Pasted image 20260707162609.png|352]]

**Highly Non-Linear Separation**

![[Pasted image 20260707162628.png|350]]

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