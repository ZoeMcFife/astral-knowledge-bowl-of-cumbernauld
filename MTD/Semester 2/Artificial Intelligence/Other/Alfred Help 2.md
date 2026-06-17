[Zoe Reference](https://colab.research.google.com/drive/16K5YcRs-IKVd60L0h5HhDZ0J52rwwSKB?usp=sharing)

# Setup

as usual

```python
## this is Colab-specific; remove if you are not using Colab

from google.colab import drive

drive.mount('/content/drive')

  
## adapt this directory to your needs

base_dir = '/content/drive/MyDrive/'

notebook_dir = base_dir + 'Colab\ Notebooks/'

data_dir = base_dir + 'Colab Notebooks/Datasets/'
```

```python
!pip install git+https://github.com/UBod/pyMLaux.git
```

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.svm import SVC
from sklearn.linear_model import LinearRegression
from sklearn import datasets
from sklearn.neighbors import KNeighborsClassifier
from sklearn.model_selection import train_test_split
import seaborn as sns
from pyMLaux import plot_2d_prediction, plot_history, show_img_data, evaluate_classification_result
import tensorflow as tf
```

This time we need `TensorFlow`. It’s the library used for neural networks!

# Data Set

This time we have separate training and test datasets. We don’t have to do a manual split!

```python
digits_data_training_raw = pd.read_csv(data_dir + 'Digits_training.csv')

digits_data_test_raw = pd.read_csv(data_dir + 'Digits_test.csv')
```

## Data

We can now make the data arrays!

```python
training_data = {
    "data": np.array(digits_data_training_raw.iloc[:, :-1]),
    "target": np.array(digits_data_training_raw.iloc[:, -1]),
    "target_names": ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"],
    "feature_names": digits_data_raw.columns[:-1]
}
```

```python
test_data = {
    "data": np.array(digits_data_test_raw.iloc[:, :-1]),
    "target": np.array(digits_data_test_raw.iloc[:, -1]),
    "target_names": ["0", "1", "2", "3", "4", "5", "6", "7", "8", "9"],
    "feature_names": digits_data_raw.columns[:-1]
}
```

## Displaying the data

From the document we got as a base, we can use the `show_img_data` method to display the digits!

```python
show_img_data(training_data['data'].reshape((training_data['data'].shape[0], 6, 4, 1)), figsize=(4, 4), interpolation=None)
```

![[Pasted image 20260617104302.png|269]]


## Reshape

We reshape the target data to be a 5 x 6 grid to match the images! I don’t know why we don’t have to reshape the test targets too? 

```python
training_data['target'][range(0, 30)].reshape(5, 6)
```

# Neural Network Setup

For convinience, I made these variables to access the data more easily.

```python
X_train = digits_data['data']
y_train = digits_data['target']
X_test = digits_data['test_data']
y_test = digits_data['test_target']
```

Since the shape and and input layer are always the same, I stored them in some variables.

```python
digits_shape = (digits_data['data'].shape[1], )
input_layer = tf.keras.layers.Input(shape=digits_shape)
```

# Neural Networks

Now let’s take a look at neural networks!

A neural networks is composed out of **neurons**!

## What is a Neuron?

Neurons have many inputs and only one output.

Every input is weighted. (Multiplied by a value that gets determined in the training process).

All inputs get summed up and multiplied by a bias. This determines how much more the all the inputs matter together.

The activation function determines when the neuron should output. This can be seen as setting up a curve with a threshold to determine when the neuron should activate.

![[Pasted image 20260617104937.png]]

## So what about neural networks?

A neural network is just many neurons arranged and connected in **layers**.

**What is a layer?**

A layer is just a collection of neurons that are connected with the previous and next layer. Important to note, every neurons output is connected to every neuron in the next layer.

A neural network typically has an **input layer**, several **hidden layers** and an **output layer**.

The input layer receives the inputs. In our case its the brightness of each pixel in our 6x5 images. Every pixels brightness is an input neuron. 

Our output layer represents what we want to get out of our network. In this case, we want to recognize the digits from 0 to 9. So we have 10 output neurons, each representing the digits. This setup is highly dependent on the scenario.

The hidden layers do the magic of math behind the scenes. This is what represents the so called *black box* of neural networks and is the part that gets trained during the training process.

![[Pasted image 20260617105548.png]]

What we do is just say how many neurons there are, how many layers there are, and what the activation function is.

## Traditional vs Modern Neural Networks

Traditional neural networks usually have few neurons, at most 5 layers and use sigmoid as the activation function. Why only so few layers? Because the training process adjusts the layer in the back by just a bit, and the next layer does the same but a bit less. So the more layers there are, the less the changes matter. **(Vanishing Gradient Effect)**

Modern neural nets use fancier activation functions, have stuff like dropout, where neurons get deactivated during the training and batch normalization that does stuff (idk).

# Neural Networks with TensorFlow

To set up a neural network with TensorFlow:

```python
optimus_prime = tf.keras.models.Sequential([
              tf.keras.layers.Input(shape=(fish_data['data'].shape[1],)),
              tf.keras.layers.Dense(5, activation='sigmoid'),
              tf.keras.layers.Dense(1, activation='sigmoid')
            ])
```

It’s kinda a mouthful, I know.

With this code, we set up a neural network with the inputs using our fish data, a hidden layer with 5 neurons, and one final neuron as the output. 

Now we gotta compile the network and set its optimizer, loss function and metrics.

The optimizer determines how the neural network trains its weights. The loss function determines how wrong the network is and the metrics is used to determine how good the network is.

```python
optimus_prime.compile(
    optimizer='sgd',
    loss='binary_crossentropy',
    metrics=['accuracy']
    )
```

## Training it!

With `fit()` we can train our model.

```python
optimus_prime.fit(x=X_train, y=y_train, epochs=100, batch_size = 1, validation_data =(X_test, y_test), verbose=2, shuffle=True)
```

We give it our training data, epochs determine how often it does the training, batch size determines how parallel it can train (higher batch sizes considerably speed up training), then we give it our test data. Verbose determines how much output it spits out and shuffle shuffles the data before training.

Using `plot_history` lets us see how good the model did, visually.

```python
plot_history(optimus_prime.history)
```

![[Pasted image 20260617110917.png|420]]

![[Pasted image 20260617110922.png|451]]

## Evaluation

With `evaluate()` we can test our model!

```python
model.evaluate(X_test, y_test)
```

And that’s basically it.

# The homework

The homework is to set up 10 trad neural nets and 10 modern ones! And then compare them..

It’s as boring as it sounds, I’m sorry.

*I made a custom neural network class to speed me up a little, these are just the parameters to be used like in the example above, I just skipped the compile step and fitting step*

## Traditional

```python
model_3 = NeuralNetwork(
    name="Ironhide",
    layers=[
        input_layer,
        tf.keras.layers.Dense(20, activation='sigmoid'),
        tf.keras.layers.Dense(50, activation='sigmoid'),
        tf.keras.layers.Dense(10, activation='softmax')
    ],

    optimizer=tf.keras.optimizers.SGD(learning_rate=0.001),
    loss_function='sparse_categorical_crossentropy',
    metrics=['accuracy'],
    epochs=20,
    batch_size=50
)
```

This is an example of a traditional one.

For the digits you have to:

- use `softmax` for the output layer as the activation function
- loss function has to be `sparse_categorical_crossentropy`

Also, you should mess around with the learning rate of the optimizer! This determines how *much* the model adjusts itself. The higher, the faster the model can change, but its much easier to just miss the optimum settings and end up nowhere. A slower learning rate means higher accuracy but it’ll take the model much longer to reach its optimum state. You can also end up in local optimums, which the model cannot get out of. This is a value to mess around with to get the right results.

After every network use `summary()` and `evaluate()` and `plot_history()` to see the results and look how it performed.

## Modern

```python
model_15 = NeuralNetwork(
    name="Devastator",
    layers=[
        input_layer,
        tf.keras.layers.Dense(128, activation='relu'),
        tf.keras.layers.Dropout(0.3),
        tf.keras.layers.Dense(128, activation='relu'),
        tf.keras.layers.BatchNormalization(),
        tf.keras.layers.Dropout(0.3),
        tf.keras.layers.Dense(10, activation='softmax')
    ],

    optimizer=tf.keras.optimizers.Adam(0.001),
    loss_function='sparse_categorical_crossentropy',
    metrics=['accuracy'],
    epochs=75,
    batch_size=100
)
```

With modern networks you can go hog wild with the layers and neuron counts.

Use different activation functions for the hidden layers:

- relu
- tanh
- softmax
- leaky_relu

and more! 

See all of them here: [TensorFlow Docs](https://www.tensorflow.org/api_docs/python/tf/keras/activations)

As for optimizers:

- Adam
- RMSprop
- AdamW
- Nadam

or others: [TensorFlow Docs](https://www.tensorflow.org/api_docs/python/tf/keras/optimizers)

Mix in some batch norms and drop outs in between and you’re golden. Idk what to look out for exactly, not an AI expert myself, so I just kinda did random stuff.

You’ll notice that the modern networks outfuck the trad ones.

# what you have to talk about


Mention the Vanishing Gradient Effect

Talk about how the traditional ones kinda underfit the datasets

and overfitting, you know the terms already. In my experience, none of the models overfitted the data.

![[kit-kit-bodega.mp4]]