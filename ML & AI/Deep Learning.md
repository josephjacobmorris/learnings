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


## References
* Chatgpt
* 