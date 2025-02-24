# Seaborn
## Introduction to Seaborn

Seaborn is an amazing visualization library for statistical graphics plotting in Python, built on top of the Matplotlib library. It provides beautiful default styles and color palettes to make statistical plots more attractive.
Seaborn aims to make visualization the central part of exploring and understanding data. It provides dataset-oriented APIs so that we can switch between different visual representations for the same variables for a better understanding of the dataset.

**Different Categories of Plot in Seaborn**

Seaborn divides the plot into several categories:

1. **Relational plots**: These plots are used to understand the relationship between two variables.
2. **Categorical plots**: These plots deal with categorical variables and how they can be visualized.
3. **Distribution plots**: These plots are used for examining univariate and bivariate distributions.
4. **Regression plots**: The regression plots in Seaborn are primarily intended to add a visual guide that helps to emphasize patterns in a dataset during exploratory data analyses.
5. **Matrix plots**: A matrix plot is an array of scatterplots.
6. **Multi-plot grids**: It is a useful approach to draw multiple instances of the same plot on different subsets of the dataset.

## Installation of Seaborn Library

For Python environment:
```bash
pip install seaborn
```

## Example Plots using Seaborn

**Basic Plots using Seaborn**

Here are some basic plots that can be used with Seaborn:

### Histogram
```python
import numpy as np
import seaborn as sns
sns.set(style="white")
rs = np.random.RandomState(10)
d = rs.normal(size=100)
sns.histplot(d, kde=True, bins=10, rug=True, hist_kws={"alpha": 0.3, "color": "m"})
```
### Distribution plot
```python
import numpy as np
import seaborn as sns
sns.set(style="white")
rs = np.random.RandomState(10)
d = rs.normal(size=100)
sns.distplot(d, kde=True, hist_kws={"alpha": 0.3, "color": "m"})
```
### Line plot
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set(style="dark")
fmri = sns.load_dataset("fmri")
plt.plot(fmri.timepoint, fmri.signal)
plt.title("Response for different events and regions")
plt.xlabel("Timepoint")
plt.ylabel("Signal")
```
### Linear regression plot
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.set(style="ticks")
df = sns.load_dataset("anscombe")
plt.scatter(df.x, df.y)
plt.plot(df.x, df.y, "r--")
plt.title("Linear Regression Model")
plt.xlabel("X")
plt.ylabel("Y")
```

## References
* https://seaborn.pydata.org/tutorial/function_overview.html
* https://www.thenerdnook.io/p/stop-struggling-with-charts?utm_source=share&utm_medium=android&r=40ln3j
* https://www.geeksforgeeks.org/introduction-to-seaborn-python/