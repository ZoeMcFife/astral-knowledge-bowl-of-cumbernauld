[Zoe Refrence](https://colab.research.google.com/drive/1JGRKLtDFB1y1xvbcMZih0LTaORx5Oroa?usp=sharing)
# setup

- google drive mounting

- pymlaux installlieren

```python
!pip install git+https://github.com/UBod/pyMLaux.git
```

- imports von libraries

## data

- DataSet1.csv von moodle downloading 
- in drive geben

**read raw data**

``` python
data_raw = pd.read_csv(data_dir + 'DataSet1.csv')
data_raw.head()
```
### Data Set 

```python
data = {
    'data': np.array(data_raw.iloc[:, 0:2]),
    'target': np.array(data_raw.iloc[:, 2]),
    'feature_names': data_raw.columns[0:2],
    'target_names': ['0', '1']
}
```

```python
# select all columns from X to Y 

array.iloc[:, X:Y] 
 
```

#### Fish Example

```python
fish_data = {
    'data': np.array(fish_data_raw.iloc[:, :-1]),
    'target': (np.array(fish_data_raw.iloc[:, -1]) + 1) / 2, # Normalize data
    'feature_names': fish_data_raw.columns[:-1],
    'target_names': ['Salmon', 'Sea bass']
    }
```

## Train Test Split

### import train test split

```python
from sklearn.model_selection import train_test_split
```

### splitting

- X → Features
- y → Targets

```python
X_train, X_test, y_train, y_test = train_test_split(data['data'], data['target'], test_size=0.3)
```


# Next Steps

**We have to train:** 
- KNN 
- SVM
- Random Forest

We need to do hyperparameter training 

GridSearchCV!!!

## How does gridsearch work

```python

cv_model = GridSearchCV(model, parameter, cv=folds)

cv_model.fit(X_train, y_train)

# parameter

parameters = {1, 2, 3, 4, 5}

# multiple parameters like this: 

parameters = {
	'parameter_1': [1, 2, 3],
	'parameter_2': [3, 4, 5]
}

# results

cv_model.best_params_ 
cv_model.best_params_['parameter_1']
cv_model.best_score_
cv_model.best_estimator_ # best model
cv_model.best_estimator_.score(X_test, y_test) # beste model an die trainings daten scoren

results = pd.DataFrame(cv_model.cv_results_) # get results of each durchgang
```

## Flipping Labels

```python
import random as rd

rd.random()

def flip_values(array, probability):
    for (i, value) in enumerate(array):
        if rd.random() < probability:
            array[i] = 1 - value
```

```python
labels = np.array(data_raw.iloc[:, 2])

flip_values(labels, 0.2)
```

```python
data = {
    'data': np.array(data_raw.iloc[:, 0:2]),
    'target': labels,
    'feature_names': data_raw.columns[0:2],
    'target_names': ['0', '1']
}
```

## Adding 15 Noise Features

```python
data_df = data_raw.iloc[:, 0:2].copy()

for i in range(15):
    data_df[f'noise_{i}'] = np.random.uniform(size=len(data_df))
```

```python
data = {
    'data': data_df,
    'target': np.array(data_raw.iloc[:, 2]),
    'feature_names': data_df.columns,
    'target_names': ['0', '1']
}
```

![[Pasted image 20260518224226.png]]

# Models

## KNN

```python
parameters = {'n_neighbors': [1, 3, 5, 7, 9]}
```

```python
knn_model = KNeighborsClassifier()
```

## SVM

```python
parameters = {
    'C':     [0.001, 0.01, 0.1, 1, 5, 10, 20, 30, 40, 50, 100, 1000],
    'gamma': [0.001, 0.01, 0.1, 1, 5, 10, 20, 30, 40, 50, 100, 1000]
}
```

```python
svm = SVC(kernel='rbf')
```

## Random Forest

```python
parameters = {'max_depth': [1, 3, 5, 10]} # TAKES A LONG TIME 
```

```python
rf = RandomForestClassifier(n_estimators=1000)
```

# Analysis 1 


do all the steps

look at the results

think about it 

!! make it pretty

# 1
## 2

### 3


# Visualization

```python
plot_2d_prediction(data['data'], data['target'], rf_cv_model.best_estimator_.predict, xval=np.linspace(0,1,10), yval=np.linspace(0,1,10))
```

![[Pasted image 20260518225243.png]]