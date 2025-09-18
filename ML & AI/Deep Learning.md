# Deep Learning

## Introduction

Deep Learning is a **subfield of Machine Learning** that focuses on using **artificial neural networks with many layers** ("deep" networks) to automatically learn patterns, features, and representations from raw data.

Instead of manually designing features (like in traditional machine learning), deep learning models **learn hierarchical feature representations** directly from data, often achieving **human-level or superhuman performance** in many domains.

### Key Ideas of Deep Learning:

1. **Neural Networks**

    * Inspired by how the human brain works.
    * Consists of layers of interconnected nodes (neurons).
    * Each neuron applies a mathematical transformation (weights + activation function).

2. **Deep Architectures**

    * "Deep" means many hidden layers (not just one or two).
    * Each layer captures higher-level abstractions:

        * Early layers → detect simple patterns (edges in an image).
        * Middle layers → detect complex patterns (shapes, textures).
        * Later layers → detect high-level concepts (faces, objects, words).

3. **Representation Learning**

    * Learns features automatically instead of requiring manual feature engineering.
    * Example: In speech recognition, the model learns to go from raw audio → phonemes → words.

4. **Massive Data & Compute**

    * Requires large datasets and high computing power (GPUs/TPUs).
    * Works best when lots of labeled or unlabeled data is available.


### Common Deep Learning Architectures:

* **Feedforward Neural Networks (FNNs)** → Basic structure.
* **Convolutional Neural Networks (CNNs)** → Excellent for images and vision tasks.
* **Recurrent Neural Networks (RNNs), LSTMs, GRUs** → Sequence tasks like speech, text, time series.
* **Transformers** → State-of-the-art in NLP (e.g., GPT, BERT) and also used in vision.
* **Autoencoders & GANs** → Unsupervised learning, data generation.

---

### Applications of Deep Learning:

* Image recognition (face ID, medical imaging).
* Natural language processing (translation, chatbots, GPT).
* Speech recognition (Siri, Alexa).
* Autonomous vehicles (self-driving cars).
* Recommender systems (Netflix, YouTube, Amazon).
* Drug discovery, healthcare, finance, robotics.

## Activation Functions
### Threshold Function

### Sigmoid Function

### Hyperbolic Function

### Rectifier Function

## Weight Adjustment


### 1. **Brute Force Method**

* **Idea**: Try out all possible weight combinations, check which one minimizes the error/loss, and pick that.
* **Example**: If you have 2 weights, you could imagine a grid of values (say from -10 to +10 in small steps), test every pair, and see which gives the lowest error.

 Pros:

* Very simple to understand.
* Guarantees finding the global optimum (if you check all possibilities with infinite precision).

 Cons:

* **Computationally impossible** for real deep learning problems.

    * A modern neural net may have **millions of weights** → the search space explodes.
* Exponential growth of possibilities (curse of dimensionality).
* Only feasible for toy problems with very few parameters.

👉 That’s why brute force is not used in practice.


### 2. **Gradient Descent Method**

Instead of trying every possible weight, gradient descent follows the **slope (gradient)** of the loss function.

* Think of the **loss function** (error) as a mountain surface.
* The goal: reach the **lowest valley (minimum error)** by adjusting weights step by step.
* At each step:

    1. Compute the gradient (partial derivatives of loss wrt each weight).
    2. Update weights in the opposite direction of the gradient:

       $$
       w_{new} = w_{old} - \eta \cdot \frac{\partial L}{\partial w}
       $$

       where $\eta$ = learning rate.

### Variants

* **Batch Gradient Descent** → use all data at once (accurate but slow).
* **Stochastic Gradient Descent (SGD)** → update per sample (fast, but noisy).
* **Mini-batch GD** → compromise between the two (most commonly used).

### Pros:

* Efficient for large-scale problems (works with millions of parameters).
* Easy to implement.
* Scales well with GPUs/parallelization.
* With optimizers like Adam, RMSProp → faster convergence.

### Cons:

* May get stuck in **local minima or saddle points** (though deep nets often still work well).
* Requires careful choice of **learning rate**:

    * Too high → overshoot, divergence.
    * Too low → very slow convergence.
* Sensitive to feature scaling.
* Non-convex landscapes (like in deep learning) → no guarantee of global optimum.

### Types of Gradient Descent

 **1. Batch Gradient Descent**

* Uses the **entire dataset** to compute the gradient before every weight update.

$$
w_{new} = w_{old} - \eta \cdot \frac{1}{N}\sum_{i=1}^N \nabla_w L(x_i, y_i)
$$

 Pros

* Stable convergence (smooth updates).
* Moves directly toward the optimum for convex problems.
* Good for small datasets (can fit into memory).

 Cons

* Very **slow** for large datasets (must process all before each update).
* Memory heavy.
* Not suitable for online learning (when new data arrives continuously).

 When to use

* Small datasets.
* When you need very stable convergence and can afford computational cost.


 **2. Stochastic Gradient Descent (SGD)**

* Updates weights **after each training example** (one sample at a time).

$$
w_{new} = w_{old} - \eta \cdot \nabla_w L(x_i, y_i)
$$

 Pros

* Very fast updates (good for very large datasets).
* Works well with streaming/online learning.
* Helps escape local minima due to noise (the "jitter" acts as random exploration).

 Cons

* Very noisy updates (loss curve jumps around instead of smoothly decreasing).
* Harder to converge to exact minima (may keep oscillating).

 When to use

* Very large datasets.
* Online/real-time learning scenarios.
* When exploration (escaping local minima) is desired.

---

 **3. Mini-Batch Gradient Descent**

* Compromise: Split dataset into **small batches** (like 32, 64, 128 samples).
* Compute gradient for each batch and update.

$$
w_{new} = w_{old} - \eta \cdot \frac{1}{m}\sum_{i=1}^m \nabla_w L(x_i, y_i)
$$
 
Pros

* **Best of both worlds**: efficient like SGD, stable like batch GD.
* Works well with GPUs (vectorized matrix ops).
* Reduces noise while keeping updates fast.
* Allows use of optimizers (Adam, RMSProp).

 Cons

* Still some noise (less than SGD).
* Choosing batch size affects performance (too small = noisy, too large = slow).

 When to use

* **Almost always** → this is the standard in deep learning.
* Large datasets that cannot fit into memory.
* Training with GPUs/TPUs.


 ✅ Quick Comparison Table

| Method                             | Update Frequency        | Pros                                                 | Cons                     | Best Use                      |
| ---------------------------------- | ----------------------- | ---------------------------------------------------- | ------------------------ | ----------------------------- |
| **Batch GD**                       | Once per dataset        | Stable, exact gradient                               | Very slow, memory heavy  | Small datasets                |
| **SGD**                            | Once per sample         | Fast, good for online learning, escapes local minima | Noisy, oscillations      | Huge datasets, streaming data |
| **Mini-Batch GD**                  | Once per batch (32–512) | Efficient, stable, GPU-friendly                      | Batch size tuning needed | Default for deep learning     |


👉 In practice:

* **Batch GD** → rarely used (too slow).
* **SGD** → research/online learning.
* **Mini-batch GD with Adam** → almost always used in deep learning frameworks (TensorFlow, PyTorch).


## Sample Artifical Neural Network python code
Got it 👍
I’ll walk you through **a full Python example** of building an **Artificial Neural Network (ANN)** using TensorFlow/Keras. I’ll include **data preprocessing**, model building, training, and explanations of the important arguments.

---

### 🧠 Steps:

1. **Import libraries**
2. **Load and preprocess data** (Scaling, train-test split)
3. **Build ANN model** (using `tf.keras.Sequential`)
4. **Compile the model** (optimizer, loss, metrics)
5. **Train the model** (`fit`)
6. **Evaluate and predict**

---

```python
# Step 1: Import necessary libraries
import numpy as np
import pandas as pd
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import StandardScaler
import tensorflow as tf
from tensorflow.keras.models import Sequential
from tensorflow.keras.layers import Dense, Dropout
```

```python
# Step 2: Load & preprocess data
# Example: Using sklearn's breast cancer dataset
from sklearn.datasets import load_breast_cancer
data = load_breast_cancer()

X = data.data        # Features
y = data.target      # Labels (0 = malignant, 1 = benign)

# Train-test split
X_train, X_test, y_train, y_test = train_test_split(X, y, 
                                                    test_size=0.2, 
                                                    random_state=42)

# Standardization (important for neural networks to converge faster)
scaler = StandardScaler()
X_train = scaler.fit_transform(X_train)
X_test = scaler.transform(X_test)
```

```python
# Step 3: Build ANN model
model = Sequential([
    # Input layer: input_dim = number of features in dataset
    Dense(units=16, activation='relu', input_dim=X_train.shape[1]),
    
    # Hidden layer with Dropout (to prevent overfitting)
    Dense(units=8, activation='relu'),
    Dropout(0.2),
    
    # Output layer (1 neuron, sigmoid for binary classification)
    Dense(units=1, activation='sigmoid')
])
```

```python
# Step 4: Compile the model
model.compile(optimizer='adam', 
              loss='binary_crossentropy', 
              metrics=['accuracy'])
```

```python
# Step 5: Train the model
history = model.fit(X_train, y_train, 
                    validation_split=0.2,  # 20% of training data for validation
                    epochs=50,             # number of iterations over dataset
                    batch_size=32,         # samples processed before weight update
                    verbose=1)             # 1 = show progress bar
```

```python
# Step 6: Evaluate the model
loss, acc = model.evaluate(X_test, y_test, verbose=0)
print(f"Test Accuracy: {acc:.4f}")

# Make predictions
y_pred = (model.predict(X_test) > 0.5).astype("int32")
print("Sample Predictions:", y_pred[:10].ravel())
```

---

### 🔑 Explanation of Important TensorFlow/Keras Arguments

#### `Dense(units, activation, input_dim)`

* **units** → Number of neurons in the layer.
* **activation** → Non-linear function applied (`relu`, `sigmoid`, `softmax`).
* **input\_dim** → Size of input features (only for first layer).

#### `model.compile(optimizer, loss, metrics)`

* **optimizer** → How weights update. Common:

    * `"adam"` → Adaptive optimizer (fast, good default).
    * `"sgd"` → Stochastic gradient descent.
* **loss** → What the model tries to minimize.

    * `"binary_crossentropy"` for binary classification.
    * `"categorical_crossentropy"` for multi-class.
    * `"mse"` for regression.
* **metrics** → How to evaluate model (e.g., `"accuracy"`, `"mae"`).

#### `model.fit(X_train, y_train, validation_split, epochs, batch_size, verbose)`

* **validation\_split** → Fraction of training data used as validation set.
* **epochs** → How many times the model sees the entire dataset.
* **batch\_size** → How many samples per gradient update.
* **verbose** → 0 = silent, 1 = progress bar, 2 = one line per epoch.

#### `Dropout(rate)`

* **rate** → Fraction of neurons to randomly drop during training (e.g., `0.2 = 20%`).
* Helps prevent overfitting.

Great question 👍
Let’s go step by step to understand **Convolutional Neural Networks (CNNs)**:

---

## 🧠 What is a CNN?

A **Convolutional Neural Network (CNN)** is a type of **deep learning model** designed specifically for data with a **grid-like topology** (e.g., images, which are 2D pixel grids).

Unlike a traditional Artificial Neural Network (ANN) that treats input as a flat vector, CNNs **preserve spatial structure** (height × width) of data.
They automatically learn **features** (edges, textures, shapes) from raw input using **convolution operations**.

---

## 🔍 CNN Use Cases

CNNs are widely used in **computer vision** and beyond:

1. **Image Classification** – Identify the object in an image (e.g., cat vs dog).
2. **Object Detection** – Locate and classify objects in an image (e.g., self-driving cars).
3. **Image Segmentation** – Classify each pixel (e.g., medical imaging).
4. **Face Recognition** – Authentication in phones, security systems.
5. **Optical Character Recognition (OCR)** – Read text from images.
6. **Medical Diagnosis** – Detect tumors, pneumonia, retinal diseases.
7. **Autonomous Vehicles** – Lane detection, traffic sign recognition.
8. **Video Processing** – Action recognition, surveillance.
9. **Recommendation Systems** – Learning from visual features (e.g., fashion e-commerce).
10. **Audio Classification** – Speech recognition (using spectrograms as images).

---

## 🏗️ Layers in a CNN

A CNN is usually made up of several types of layers stacked together:

### 1. **Convolutional Layer**

* Core building block of CNN.
* Uses **filters/kernels** (small matrices, e.g., 3×3, 5×5) that slide across the image.
* Captures local patterns like edges, textures, corners.
* Hyperparameters:

    * **filters** → number of feature maps to learn.
    * **kernel\_size** → size of filter (e.g., `(3,3)`).
    * **stride** → how much filter moves each step.
    * **padding** → `"same"` (keep size) or `"valid"` (shrink size).

---

### 2. **Activation Layer**

* Adds **non-linearity** (otherwise CNN is just linear filtering).
* Most common: **ReLU** (`max(0, x)`) – fast and prevents vanishing gradient.

---

### 3. **Pooling Layer**

* Downsamples feature maps → reduces computation + overfitting.
* Types:

    * **Max Pooling** – takes maximum value in a region.
    * **Average Pooling** – takes average value in a region.
* Example: 2×2 max pooling reduces image size by half.

---

### 4. **Batch Normalization Layer**

* Normalizes activations to speed up training and stabilize learning.

---

### 5. **Dropout Layer**

* Randomly “drops” neurons during training (prevents overfitting).

---

### 6. **Flatten Layer**

* Converts 2D feature maps into 1D vector (so we can feed into dense layers).

---

### 7. **Fully Connected (Dense) Layer**

* Classic ANN layer, connects every neuron to next layer.
* Usually used at the end for **classification/regression**.
* Example: If 10 classes, final dense layer has `units=10`, `activation='softmax'`.

---

### 8. **Output Layer**

* Depends on task:

    * **Binary classification** → 1 neuron, `sigmoid`.
    * **Multi-class classification** → n neurons, `softmax`.
    * **Regression** → 1 neuron, linear activation.

---

## ⚡ CNN Example Architecture (Image Classification)

```
Input Image (64x64x3)
 → Convolution (32 filters, 3x3) + ReLU
 → MaxPooling (2x2)
 → Convolution (64 filters, 3x3) + ReLU
 → MaxPooling (2x2)
 → Flatten
 → Dense (128 neurons, ReLU)
 → Dense (10 neurons, Softmax)   ← 10 classes
```

---

✅ **Key Idea:**
CNNs **learn features automatically** (edges → shapes → objects) without manual feature engineering.

## References
* Chatgpt
* 