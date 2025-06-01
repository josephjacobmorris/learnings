# Machine Learning

## Introduction
**What is Machine Learning?**

Machine learning (ML) is a subset of artificial intelligence (AI) that enables computers to learn from data without being explicitly programmed. Unlike regular programming, where every detail must be explicitly written, ML allows the computer to discover patterns and make predictions on its own by analyzing large datasets.

**The Key Difference: Training a Model vs. Following Instructions**

In traditional programming, we dictate every step of the process, whereas in machine learning, we provide data and algorithms that enable the model to learn and improve over time. This approach makes ML particularly useful for complex problems that require significant computational resources or expertise.

**How Machine Learning Works**

Machine learning involves training a model on a dataset, where the model learns to recognize patterns and relationships between different variables. With enough practice, the model becomes more accurate at making predictions or decisions. The process is often referred to as "supervised learning," where the model is trained on labeled data (i.e., examples with correct outputs) to learn what constitutes a correct output.

**The Magic of ML in Everyday Life**

Machine learning has a significant impact on our daily lives, transforming the way we interact with technology. Here are some examples:

* **Recommendations:** Netflix, YouTube, and other services use ML to recommend content based on your viewing history.
* **Voice Assistants:** Virtual assistants like Siri, Alexa, and Google Assistant use ML to understand voice commands and respond accordingly.
* **Spam Filters:** Email clients and online shopping platforms rely on ML to filter out spam messages and prioritize genuine communications.

**Real-World Applications of Machine Learning**

Machine learning has far-reaching implications across various industries:

* **Healthcare:** Predicting health outcomes, identifying high-risk patients, and personalizing treatment plans.
* **Finance:** Analyzing market trends, predicting stock prices, and detecting financial anomalies.
* **Online Shopping:** Personalized product recommendations, optimizing search results, and improving customer experience.

## Steps in a workflow of ML
### Data Pre-processing
#### Split Data into training and test
Splitting data into a training set and a test (or validation) set is a fundamental step in machine learning, and it's essential for several reasons:

**Why split data?**

1. **Evaluate model performance**: By splitting data into two sets, you can evaluate the model's performance on unseen data, which helps to:
   * Assess generalizability
   * Identify biases or overfitting
   * Refine the model architecture or hyperparameters
2. **Monitor overfitting**: If a model is too complex and fits the training data too well, it may not generalize well to new data. Splitting data allows you to monitor overfitting during training.
3. **Compare performance metrics**: By evaluating the model on both the training and test sets, you can compare its performance on unseen data, which helps to:
   * Validate model robustness
   * Identify areas for improvement
4. **Fine-tune hyperparameters**: Splitting data enables you to tune hyperparameters (e.g., learning rate, batch size) on the test set, which helps to optimize the model's performance.
5. **Backtesting and model selection**: You can use the training set to train a model and then evaluate its performance on the test set before deploying it in production.

**Types of split techniques:**

1. **Random Split**: divides the data into two equal halves randomly
2. **Stratified Split**: maintains the same proportion of classes (or bins) in both the training and test sets as in the original dataset
3. **K-Fold Cross-Validation**: divides the data into k subsets, trains models on k-1 subsets, and evaluates their performance on the remaining subset

**When to split data:**

1. **Model selection**: Splitting data helps you evaluate the model's performance on unseen data, which is essential for selecting the best algorithm.
2. **Hyperparameter tuning**: Fine-tuning hyperparameters during training using a test set helps to optimize the model's performance.
3. **Model validation**: Evaluating the model on both the training and test sets after each iteration allows you to monitor its performance and refine it accordingly.

**Best practices:**

1. **Keep the split size reasonable**: Avoid splitting data into too few or too many subsets, as this can lead to suboptimal evaluations.
2. **Choose a suitable split technique**: Select a method that balances evaluation of overfitting and generalizability.
3. **Use a consistent split approach**: Apply the same split technique across different stages of the machine learning workflow.

By following these guidelines, you'll be able to effectively split your data into training and test sets, enabling you to evaluate and refine your machine learning model's performance more accurately.
#### Feature Scaling
Feature scaling is a process that helps reduce or normalize large or extreme features in machine learning models, especially when dealing with high-dimensional data. Let's break it down in simple terms:

**What are features?**
In machine learning, features are the characteristics or attributes of data, such as text, images, numbers, or categorical variables.

**Why scale features?**
When working with large datasets, some features might be much larger than others (e.g., a image of a person is much more massive than a text description). This can lead to several problems:

1. **Insufficient representation**: A feature that's too large might not provide enough information for the model to learn.
2. **Overfitting**: Models might overfit to the training data, failing to generalize well to new, unseen data.
3. **Computational challenges**: Training models on very large features can be computationally expensive.

**Scaling approaches:**

1. **Standardization (Z-score scaling)**:
   This involves subtracting the mean and dividing by the standard deviation of each feature to create a new value between 0 and 1. For example, if you have a feature with values like `10`, `-5`, and `20`, the standardized version would be `(0 - (-5)) / sqrt(1 + (10^2) + (20^2))`.
2. **Normalization**:
   This method scales features to a common range, usually between 0 and 1, by subtracting the minimum value and dividing by the range of values.

**Why choose one approach?**
The choice of scaling method depends on:

* The nature of the data (e.g., categorical vs. continuous features)
* The model architecture and its requirements
* The specific problem you're trying to solve

> Note: Visualising the data help to draw inferences about the data also identify outliers etc

#### Data Pre-processing using python libraries
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
```
Read
```Python
dataset=pd.read_csv('Data.csv')
// ensure that dep col is last before using below code
feature_cols = dataset.iloc[:,:-1].values
dep_col = dataset.iloc[:,-1].values
```
> Note: In the context of Machine Learning (ML), features refer to input variables that are used as an input to your model during training or prediction phase - they're characteristics from which we learn how something else changes with certain inputs.
#### Taking care of missing data
* Delete it , if you are a large dataset and the missing data constitute very less %
* Replace it by mean, median or mode of that particular column
```Python
from sklearn.impute import SimpleImputer
imputer=SimpleImputer(missing_values=np.nan,startegy='mean')
// better include all numerical columns
imputer.fit(X[:,1:3])
X[:,1:3] = imputer.transform(X[:,1:3] )
 ```

> Note: To identify sum of missing data we can use `missing_data = df.isnull().sum()`

#### Encoding Data
**One Hot Encoding**
One-Hot Encoding (OHE) is a popular technique used in Machine Learning (ML) for categorical data. In the context of ML, OHE is used to represent a target variable as a binary vector where each dimension corresponds to a specific category or label.

**Problem it solves:**

In many real-world applications, such as classification and regression problems, you often encounter categorical variables that have multiple possible values (e.g., "yes" or "no", "male" or "female"). One-Hot Encoding helps in solving this problem by transforming these categorical variables into a numerical representation that can be easily processed by ML algorithms.

**How it works:**

One-Hot Encoding involves the following steps:

1.  **Select relevant categories:** Identify the categories that need to be encoded.
2.  **Create binary vectors:** For each category, create a binary vector where:
   *   A 1 indicates that the corresponding category is present.
   *   A 0 indicates that the category is not present.
3.  **Transform data:** Convert categorical variables into numerical representations by replacing missing values with zeros in a specific way (e.g., mean or mode).

**Types of One-Hot Encoding:**

There are two main types:

1.  **Binary One-Hot Encoding (B-One-HOT):** Used for binary classification problems where the target variable has only two possible outcomes.
2.  **Multivariate One-Hot Encoding:** Used for multi-class classification or regression problems where more than two classes exist.

**Advantages:**

One-Hot Encoding offers several benefits:

*   Simplifies data representation and processing
*   Can handle large datasets with many categories
*   Allows for easier feature engineering and selection

However, it may not be suitable for all scenarios, especially when working with high-dimensional data or categorical variables that have multiple related concepts.

**Common use cases:**

One-Hot Encoding is commonly used in:

*   Classification problems (e.g., spam vs. non-spam emails)
*   Regression problems (e.g., predicting continuous outcomes)
*   Clustering and dimensionality reduction
*   Data preprocessing for ML algorithms
```Python
# Import necessary libraries
import pandas as pd
import numpy as np
from sklearn.compose import ColumnTransformer
from sklearn.preprocessing import OneHotEncoder

# Perform One-Hot Encoding on the target variable with multiple categories
ct = ColumnTransformer(transformers=[('encoder', OneHotEncoder(),[0])], remainder='passthrough')
x = np.array(ct.fit_transform(x))
```
**Label encoding**

```Python
# Importing the necessary libraries
import pandas as pd
import numpy as np 
from sklearn.preprocessing import OneHotEncoder
from sklearn.preprocessing import LabelEncoder
from sklearn.compose import ColumnTransformer

# Load the dataset
df = pd.read_csv('titanic.csv')

# Identify the categorical data


# Implement an instance of the ColumnTransformer class
ct = ColumnTransformer([('one_hot_encoder', OneHotEncoder(), ['Sex','Embarked','Pclass'])], remainder='passthrough')

# Apply the fit_transform method on the instance of ColumnTransformer


# Convert the output into a NumPy array
X=np.array(ct.fit_transform(df))

# Use LabelEncoder to encode binary categorical data
le= LabelEncoder()
y=le.fit_transform(df['Survived'])
# Print the updated matrix of features and the dependent variable vector
print(X)
print(y)


```
#### Splitting data into training set and testing set

```Python
from sklearn.model_selection import train_test_split
X_train,X_test,y_train,y_test = train_test_split(X,y,test_size=0.2, random_state=1)
```
> Note: Feature scaling should be done after splitting data as otherwise it will lead to 


##  Types of Machine Learning
### Supervised Machine Learning: Task Driven (Preduct next value)
**Supervised Learning Algorithms**

Supervised learning is a type of machine learning where the model learns from labeled data, meaning the inputs and their correct outputs are already known. This approach enables the model to learn how to map inputs to outputs, enabling it to make predictions on new, unseen data.

**A Simple Analogy: Flashcards for Learning**

To illustrate this concept, think of a flashcard system where you have pre-written answers (inputs) and corresponding responses (outputs). The goal is to teach the student to recall the correct answer by studying the cards. In supervised learning, the model is like a student who receives labeled data through training, where they learn to connect inputs with outputs.

**Common Supervised Learning Algorithms**

1. **Linear Regression**: Used for predicting continuous values, such as house prices.
2. **Logistic Regression**: Suitable for problems with two outcomes (e.g., yes/no decisions).
3. **Support Vector Machines (SVM)**: Effective for classification tasks, where the model can learn to separate different classes or labels.
4. **Decision Trees and Random Forests**: Can be used for both classification and regression problems, making them versatile and powerful tools.

### Un-Supervised Machine Learning: Data Driven (Identify Cluser)
**Unsupervised Learning: Exploring Unlabeled Data**

Unsupervised learning is a type of machine learning that operates without labeled data. Instead, the model discovers patterns and relationships within the unlabeled data on its own, often by grouping similar data points together.

**Like Exploring a New City Without a Map**

To illustrate this concept, think of trying to explore a new city without a map. You wouldn't be given specific locations or directions; instead, you'd observe the streets, landmarks, and surroundings to figure out how to navigate and find your way around.

**Key Techniques in Unsupervised Learning**

1. **Clustering**: This technique groups similar data points together based on their features or characteristics. A popular example is K-Means clustering, which divides data into distinct clusters by minimizing the difference between them.
2. **Dimensionality Reduction**: This technique simplifies a dataset by reducing the number of features while preserving the essential information. Principal Component Analysis (PCA) is a common method for dimensionality reduction, which helps reduce the complexity of the data and make it easier to analyze.

**Popular Unsupervised Learning Algorithms**

1. **K-Means Clustering**: A popular algorithm for grouping similar data points into clusters.
2. **Principal Component Analysis (PCA)**: A technique for reducing the number of features in a dataset while retaining most of the important information.
3. **Hierarchical Clustering**: An extension of K-Means clustering that builds a hierarchy of clusters, allowing the model to identify subgroups within each cluster.

**Real-World Applications of Unsupervised Learning**
Reinforcement Learning
Finally this type is different from both the previous two. In this type of learning, an agent learns how to make decisions by interacting with an environment. The goal is to train the model to achieve the best outcome by taking actions and receiving feedback—rewards for good actions and penalties for bad ones.

Over time, the model learns to maximize rewards and avoid penalties. It’s a lot like teaching a dog to fetch. At first, the dog might not know what you want it to do. But every time it successfully brings the ball back, it gets a treat (reward). That positive reinforcement teaches the dog to repeat the behavior.

Some common uses for reinforcement learning include:

Robotics: Training robots to move around and complete tasks.

Gaming: Developing AI that can play video games as well as or even better than humans (like AlphaGo).

Self-Driving Cars: Helping cars learn how to navigate roads by simulating real-world driving situations.


1. **Market Research**: As mentioned earlier, unsupervised learning can help businesses group customers based on their shopping habits and create targeted marketing campaigns.
2. **Recommendation Systems**: Unsupervised learning algorithms can be used to recommend products or services based on user behavior and preferences.
3. **Image Recognition**: Unsupervised learning techniques are often used in image recognition systems, where the model learns to identify patterns in images without labeled data.

### Reinforcement Learning (Learn from mistakes)
**Reinforcement Learning: Making Decisions in the Real World**

Reinforcement learning is a type of machine learning that involves an agent interacting with an environment, taking actions, and receiving feedback in the form of rewards or penalties. The goal is to train the model to make decisions that maximize rewards while minimizing penalties.

**Like Teaching a Dog to Fetch**

To illustrate this concept, think of teaching a dog to fetch a ball. At first, the dog might not know what you want it to do. However, every time it successfully brings the ball back to you and receives a treat (reward), it learns that the behavior is desirable.

**Common Applications of Reinforcement Learning**

1. **Robotics**: Reinforcement learning is used in robotics to train robots to navigate complex environments and perform tasks such as:
    * Autonomous driving
    * Manipulation of objects
    * Assembly and disassembly of products
2. **Gaming**: Reinforcement learning is used in gaming to develop AI that can play games effectively, such as:
    * AlphaGo, a deep learning-based artificial intelligence that defeated a human world champion in Go
3. **Self-Driving Cars**: Reinforcement learning is used to help self-driving cars learn how to navigate roads and make decisions in real-time, by simulating real-world driving scenarios.

**Key Components of Reinforcement Learning**

1. **Reward Signal**: A signal that indicates the reward or penalty received for an action.
2. **Environment Model**: A model that represents the environment and describes its dynamics and state.
3. **Policy**: The decision-making function that takes in the current state and generates a set of actions to take.
4. **Value Function**: A representation of the expected return or utility of each state.

**How Reinforcement Learning Works**

1. The agent interacts with the environment, taking actions based on the policy.
2. The agent receives a reward signal for each action taken.
3. The agent updates its value function using the rewards received and the current policy.
4. The process is repeated iteratively until convergence or termination.

## Regression

### Simple Linear Regression
```Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.model_selection import train_test_split
from sklearn.linear_model import LinearRegression

dataset=pd.read_csv('salary.csv')
X=dataset.iloc[:,:-1].values
y=dataset.iloc[:,-1].values

X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.2,random_state=1)
lr= LinearRegression()
lr.fit(X_train,y_train)

y_pred=lr.predict(X_test)

plt.scatter(X_train,y_train, color = 'red')
plt.plot(X_train,r.predict(X_train),color ='blue')
plt.title('Salary vs Exp')
plt.xlabel('YOE')
plt.ylabel('Salary')
plt.show()

plt.scatter(X_test,y_test, color = 'red')
plt.plot(X_train,r.predict(X_train),color ='blue')
plt.title('Salary vs Exp')
plt.xlabel('YOE')
plt.ylabel('Salary')
plt.show()
```


### Multiple Linear Regression 

#### Assumptions of Linear Regression

* Linearity
* Homoscedasticity - Equal variance
* Multivariate Normality
* Independence
* Lack of multicollinearity
* Outlier check

> Note: Lookout for the dummy variable trap and collinear variance
#### Sample Code
Same as Simple Linear Regression
### Polynomial Regression
### Sample Code
```python
import pandas as pd
import matplotlib.pylot as plt
import numpy as np
from sklearn.preprocessing import PolynomialFeatures
from sklearn.linear_model import LinearRegression
df=pd.read_csv('salary.csv')
X = df.iloc[:,1:2].values
y = df.iloc[:, -1].values

poly = PolynomialFeatures(degree=4)
X_poly = poly.fit_transform(X)
poly_lin_reg = LinearRegression()
poly_lin_reg.fit(X_poly, y)

 # Plotting Results
 plt.scatter(X, y, color='blue')
 
plt.plot(X, lin2.predict(poly_lin_reg.fit_transform(X)),
         color='red')
plt.title('Polynomial Regression')
plt.xlabel('XLabel')
plt.ylabel('YLabel')
 
plt.show()
```
> Note: Beware of overfitting when choosing polynomial degree as it may lead to overfitting
#### Advantages
* Higher approxination between features and dependant variable
#### Disadvantages
* Very sensitive to outliers
#### Real World use cases
* growth rate of tissues
* disease epidemics progression

### Support Vector Regression (SVR)
> Note: Feature scaling should be applied since there are no coefficient

#### Sample Code
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
from sklearn.preprocessing import StandardScaler

df = pd.read_csv('Position_Salaries.csv')
X=df.iloc[:,1:2].values
y=df.iloc[:,-1].values

# Feature Scaling on features and dependant variables
y=y.reshape(len(y),1)
scy = StandardScaler()
scX = StandardScaler()
y=scy.fit_transform(y)
X=scX.fit_transform(X)


# SCVR
from sklearn.svm import SVR
regressor = SVR(kernel = 'rbf')
regressor.fit(X,y)
scy.inverse_transform(regressor.predict(scX.transform([[6.5]])).reshape(-1,1))
```

### Decision Tree Regression

### Random Forest Regression

## Classification
### Logistic Regression

### KNN

### Support Vector Machine

### Kernel SVM

For non-linearly separable dataset mapping them to higher dimension where they are linearly separable
Good explaination at : https://medium.com/@abhishekjainindore24/svm-kernels-and-its-type-dfc3d5f2dcd8

#### Sample Code
```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

df = pd.read_csv('Social_Network_Ads.csv')
X = df.iloc[:,:-1].values
y = df.iloc[:,-1].values


from sklearn.model_selection import train_test_split

# Do feature scaling if required

X_train, X_test, y_train, y_test = train_test_split(X,y, test_size=0.2)
from sklearn.svm import SVC
md = SVC(kernel='rbf')
md.fit(X_train,y_train)

y_pred = md.predict(X_test)

from sklearn.metrics import accuracy_score
accuracy = accuracy_score(y_test,y_pred)
from sklearn.metrics import confusion_matrix
cm = confusion_matrix(y_test, y_pred_test) 
```

Types of kernel function:
#### Rbf (Radial Basis Function)
**Advantages**
* can handle very comples and non -linear relationships

**Disadvantages**
* computational intensitive
* requires careful tuning of the `σ` parameter

#### Sigmaoid

**Advantages**
* can be used to model relationships simalar to found in neaural networks
* simple to implement
**Disadvantages**


#### Polynomial
**Advantages**
* can model feature interaction
**Disadvantages**
* risk of over-fitting
* computationally expensive

### Naive Bayes

### Decision Tree Classification

## Clustering

### K Means Clustering

### Hierarchical Clustering

## Association Learning
📘 What is **Association Rule Learning**?

**Association Rule Learning** is a **rule-based machine learning technique** for discovering interesting relationships, patterns, or associations among variables in large datasets.

It is most commonly used in:

* **Market basket analysis**
* **Recommendation engines**
* **Web usage mining**
* **Medical diagnosis**

**Common Algorithms in Association Rule Learning**

Here are the most well-known algorithms:

| Algorithm                               | Description                                                                                                             |
| --------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
| **Apriori**                             | Generates frequent itemsets by iteratively expanding them and pruning non-frequent subsets. Uses the Apriori property.  |
| **Eclat**                               | Uses a vertical data format and intersection-based approach for better efficiency on dense datasets.                    |
| **FP-Growth (Frequent Pattern Growth)** | Builds a compact **prefix tree (FP-tree)** and avoids candidate generation. Much faster than Apriori on large datasets. |
| **AIS**                                 | Early algorithm that generated candidate itemsets during the scan. Less efficient than Apriori.                         |
| **SETM (Set-Oriented Mining)**          | Generates itemsets using SQL-like set operations. Focuses on performance in relational databases.                       |
| **CHARM**                               | Used for mining **closed frequent itemsets** (i.e., maximal sets with no supersets of same support).                    |
| **SPADE / PrefixSpan**                  | Algorithms used for **sequential pattern mining**, a related field where order of itemsets matters.                     |

---

**⚖️ Comparison (Apriori vs FP-Growth vs Eclat)**

| Feature                    | Apriori                           | FP-Growth            | Eclat                           |
| -------------------------- | --------------------------------- | -------------------- | ------------------------------- |
| Uses candidate generation? | ✅ Yes                             | ❌ No                 | ❌ No                            |
| Memory usage               | Medium                            | Low                  | High (on dense data)            |
| Speed                      | Slower (due to multiple DB scans) | Faster (fewer scans) | Faster (if data fits in memory) |
| Suitable for               | Small to medium datasets          | Large datasets       | Dense datasets                  |

### Apriori
The **Apriori algorithm** is a classic algorithm used in **association rule mining** and **frequent itemset mining**. It is particularly popular in **market basket analysis**, where the goal is to find relationships between items purchased together.

---

#### 🔍 What is the Apriori Algorithm?

The **Apriori algorithm** is used to identify **frequent itemsets** (sets of items that appear together frequently in a dataset) and derive **association rules**. It operates on the principle that:

> **"If an itemset is frequent, then all of its subsets must also be frequent."**
> This is known as the **Apriori property** (or anti-monotonicity property).

---

#### ⚙️ Working of the Apriori Algorithm

1. **Set a minimum support threshold**.
2. **Generate frequent itemsets**:

   * Start with individual items (1-itemsets).
   * Calculate their **support** (frequency in transactions).
   * Prune items that do not meet the minimum support.
   * Join remaining items to form 2-itemsets, then 3-itemsets, etc.
   * Repeat until no more itemsets can be formed.
3. **Generate association rules**:

   * For each frequent itemset `L`, generate rules of the form `A → B`, where `A ⊂ L` and `B = L − A`.
   * Calculate metrics like **confidence**, **lift**, etc.
   * Retain rules that meet the **minimum confidence**.

---

#### 📘 Key Terms

| Term                 | Description                                                                                                                               |
| -------------------- | ----------------------------------------------------------------------------------------------------------------------------------------- |
| **Itemset**          | A collection of one or more items.                                                                                                        |
| **Support**          | The proportion of transactions in the dataset that contain the itemset.                                                                   |
| **Confidence**       | Measures how often items in `B` appear in transactions that contain `A`. `conf(A → B) = support(A ∪ B) / support(A)`                      |
| **Lift**             | Measures how much more likely `B` is given `A` compared to the baseline likelihood of `B`. `lift(A → B) = confidence(A → B) / support(B)` |
| **Frequent Itemset** | Itemsets that meet or exceed the minimum support threshold.                                                                               |

---

#### 🐍 Sample Python Implementation (Using `mlxtend`)

```python
from mlxtend.frequent_patterns import apriori, association_rules
import pandas as pd

# Sample transaction data
dataset = [
    ['milk', 'bread', 'nuts', 'apple'],
    ['milk', 'bread', 'nuts'],
    ['milk', 'bread'],
    ['milk', 'bread', 'apple'],
    ['milk', 'bread', 'apple']
]

# Convert to one-hot encoding DataFrame
from mlxtend.preprocessing import TransactionEncoder
te = TransactionEncoder()
te_ary = te.fit(dataset).transform(dataset)
df = pd.DataFrame(te_ary, columns=te.columns_)

# Step 1: Get frequent itemsets
frequent_itemsets = apriori(df, min_support=0.6, use_colnames=True)

# Step 2: Generate rules
rules = association_rules(frequent_itemsets, metric="confidence", min_threshold=0.7)

print("Frequent Itemsets:")
print(frequent_itemsets)
print("\nAssociation Rules:")
print(rules[['antecedents', 'consequents', 'support', 'confidence', 'lift']])
```


#### 💡 Popular Use Cases

1. **Market Basket Analysis**:

   * Discover which items are frequently bought together.
   * Optimize product placement in stores.

2. **Recommendation Systems**:

   * Suggest products that are frequently purchased together.

3. **Inventory Management**:

   * Group items that are often co-purchased to improve stock efficiency.

4. **Web Usage Mining**:

   * Discover common navigation patterns on websites.

5. **Medical Diagnosis**:

   * Find relationships between symptoms and diseases.

---

#### ⚠️ Limitations

* **Computational Complexity**: Can be slow on large datasets due to repeated scanning of the database.
* **Large Rule Sets**: May generate many rules, requiring further filtering.

### Eclat

## Reinforcement Learning

### Upper Confidence Bound (UCB)

### Thompson Sampling

## References
* LLMs
* https://www.thenerdnook.io/p/machine-learning-explained?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* https://www.thenerdnook.io/p/create-a-foolproof-ml-workflow?utm_source=share&utm_medium=android&r=40ln3j&triedRedirect=true
* https://udemy.com/course/machinelearning/learn/lecture/19019942#learning-tools 
* https://www.geeksforgeeks.org/assumptions-of-linear-regression
* https://www.geeksforgeeks.org/python-implementation-of-polynomial-regression/
* https://www.geeksforgeeks.org/support-vector-regression-svr-using-linear-and-non-linear-kernels-in-scikit-learn/
* https://medium.com/@abhishekjainindore24/svm-kernels-and-its-type-dfc3d5f2dcd8
* https://www.kaggle.com/code/prashant111/svm-classifier-tutorial
* [Apriori Algorithm](https://www.geeksforgeeks.org/apriori-algorithm/)
* [Implementing Apriori Algorithm In Python](https://www.geeksforgeeks.org/implementing-apriori-algorithm-in-python/)