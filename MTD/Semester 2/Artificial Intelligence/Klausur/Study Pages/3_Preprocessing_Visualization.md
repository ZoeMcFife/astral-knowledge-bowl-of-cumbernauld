#artificial_intelligence 

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