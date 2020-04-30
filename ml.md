# Maching Learning 
- AI algorithm learns from datasets 

- supervised learning 
  - person provides labels for data and the algorithm learns to identify things in the future by looking at what was labeled in the past 
- unsupervised learning
  - algorithm is fed datasets and detects/groups patterns by itself 

# Scikit-Learn Stuff
- linear regression
- naive bayes
- k-nearest neighbors
- k-means
- validating the model 
- training sets and test sets 

# Other Libraries 
- should get familiar with `numpy`, `pandas`, `matplotlib`

# Purpose 
- purpose of machine learning is often to model some real-life situation so that we can make good predictions for the future with different inputs 

# Linear Regression 
- basically a line of best fit
- fit the model with a straight line 
  - line is determined by $y = mx+b$ (so we're looking for the best values for m and b)

- **loss:** a measure of how bad the model's prediction was (error) 
  - the squared distance from the point to the line (squared so that both directions impacts it equally)
- the goal of the linear regression model is to minimize the loss on average across the dataset

- **gradient descent:** on the gradient of loss (graphing loss against m or against b)
  - we move in the direction that decreases our loss the most 
  - there's a formula for both b and m that I did not copy down 
- to find the optimal value, use a learning rate to change the value and then compare how well it works 
  - run it a bunch of times so that loss is lowered each time 
- learning rate needs to be chosen so that its not too big that it skips over the optimal value but not so small that it takes too long to reach the optimal value 

- **convergence:** when loss stops changing (essentially a limit)

## Multiple Linear Regression 
- has multiple independant variables 

- most ML algorithms will split dataset into a training set and a test set 
```python
from sklearn.model_selection import train_test_split

x_train, x_test, y_train, y_test = train_test_split(x, y, train_size=0.8, test_size=0.2, random_state=6)
```
- fit the linear regression model with values from `x_train` and `y_train`, then you predict for `x_test` and `y_test``
- for each independant variable, the relation to x will be different (ex. positive vs negative correlation)

### Residual Analysis 
- one method of evaluating accuracy
- essentially whether or not the model has learned the coefficients correctly 

## Steps 
1. open file and read into data frame 
2. explaratory analysis (look at columsn, data, basic stats)
  - `data.head()`: returns first 5 rows?
  - `data.columns`: returns a list of all the column titles (the different features)
  - `data.describe()`: returns a table with the count, mean, std etc of each column 

- scikit's linear regression comes with `.score()` which returns a coefficient telling you how accurate it is (max of 1.0)
