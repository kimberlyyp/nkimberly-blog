---
layout: post
title: "Implementing Regressions in Python: SVM, CART, and Random Forest"
date: 2018-03-17 22:53:58 +0000
categories: ['Data Science', 'Python']
---

Regression is a popular technique used to model and analyze relationships among variables. There are dozens of models, but I wanted to summarize the six types I learned this past weekend. While theory was a large component of the class, I am opting for more of a practical approach in this post.

Here are the six regression implementations I'll cover,

	Simple Linear Regression
	Multiple Linear Regression (+ Backward Elimination)
	Polynomial Regression
	Support Vector Regression
	Decision Tree Regression
	Random Forest Regression

## ![29314914_1581964475253982_1199160405256044544_o.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/29314914_1581964475253982_1199160405256044544_o1.png)
*This is part 2 of 2, where I will cover Support Vector, Decision Tree, and Random Forest Regressions! You can find part 1 [here](https://nkimberly.wordpress.com/2018/03/12/implementing-regressions-in-python-simple-linear-multiple-linear-and-polynomial/).*
## Support Vector Regression
Support Vector Machine (SVM) analysis is a popular machine learning tool for classification and regression. SVM regression is considered a nonparametric technique because it relies on kernel functions. SVMs are based on the idea of finding a hyperplane that best divides a dataset into two classes, as shown in the image below.

[caption id="attachment_7320" align="aligncenter" width="540"]![SVM](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/svm.png) Credit: AYLIEN[/caption]

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 10 rows and 3 columns. The columns are titled position, level, and salary. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, level is the independent variable, while salary is the dependent variable.

[code language="python"]
# Importing the dataset
dataset = pd.read_csv('Position_Salaries.csv')
X = dataset.iloc[:, 1:2].values
y = dataset.iloc[:, 2].values
[/code]

Next, we will need to scale our features. This is because we will be using the SVR library and it does not automatically handle feature scaling for us. Instantiate a two objects of StandardScaler() class and run fit_transform on our X and y inputs.

[https://pastebin.com/embed_iframe/rq9q47kd](https://pastebin.com/embed_iframe/rq9q47kd)

[code language="python"]
# Feature Scaling
from sklearn.preprocessing import StandardScaler
sc_X = StandardScaler()
sc_y = StandardScaler()
X = sc_X.fit_transform(X)
y = sc_y.fit_transform(y)
[/code]

** Step 2: Fitting Data **

The sklearn tool kit provides us with the SVR fit method to fit our training dataset. By default, the fit method uses the radial basis function kernel (Gaussian). RBF has many advantages, but the one that makes me like it the most is its 'smoothness'. RBF is able to wade through all of the noise and provide us a smooth function approximating non-linear behavior. Here are some good samples: http://mccormickml.com/2014/02/26/kernel-regression/.

[code language="python"]
# Fitting SVR to the dataset
from sklearn.svm import SVR
regressor = SVR(kernel = 'rbf')
regressor.fit(X, y)
[/code]

** Step 3: Running Prediction(s) **

Now, do remember that we conducted feature scaling here. For this reason, we do need to scale back when we run predictions. Here, if we wanted to figure out how much the salary of a level 6.5 position would be, we would need to create an array with 6.5 and plug it into our predict function. Then we'd inverse it to get the scaled back value.

[code language="python"]
# Predicting a new result
X_pred = sc_X.transform(np.array([[6.5]]))
y_scale_back = sc_y.inverse_transform(regressor.predict(X_pred))
[/code]

** Step 4: Visualizing Data **

Nothing new here- just lot observations overlaid with a prediction line :) P.S. notice how SV regression treats the last point as an outlier!

[code language="python"]
# Visualising the SVR results
plt.scatter(X, y, color = 'red')
plt.plot(X, regressor.predict(X), color = 'blue')
plt.title('Truth or Bluff (SVR)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
[/code]

![svr-example.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/svr-example.png)
## CART / Decision Tree Regression
CART stands for Classification and Regression Tree. The CART method constructions a regression tree through recusirve partitioning. To borrow a dataset from the sklearn documentation, check out for following decision tree regression overlaid blue. Notice that it is a step-wise function.

[caption id="attachment_7320" align="aligncenter" width="540"]![decisiontree](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/decision1.png) Credit: sklearn documentation[/caption]

This is what we mean by recursive partitioning: we split up the input features into several regions and then go on to assign a prediction value for that particular region. We can set the max depth parameter to control how it learns. It is important not to set it too high, as we can get a tree that learns too fine of detail and overfits the data (see green overlay).

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 10 rows and 3 columns. The columns are titled position, level, and salary. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, level is the independent variable, while salary is the dependent variable.

[code language="python"]
# Importing the dataset
dataset = pd.read_csv('Position_Salaries.csv')
X = dataset.iloc[:, 1:2].values
y = dataset.iloc[:, 2].values
[/code]

The DecisiontreeRegressor library is pretty common, so it turns out feature scaling is automatic. We do not need to pre-process the data any further.

** Step 2: Fitting Data **

The sklearn tool kit provides us with the DecisionTreeRegressor fit method to fit our training dataset. As before, we simply instantiate an object from the class and run the fit method on our X and y inputs.

[code language="python"]
# Fitting Decision Tree Regression to the dataset
from sklearn.tree import DecisionTreeRegressor
regressor = DecisionTreeRegressor()
regressor.fit(X, y)
[/code]

** Step 3: Running Prediction(s) **

Running predictions is rather straightforward here. For demonstration purposes, I've run both the desired level 6.5 prediction (150k) and a 6.51 prediction (200k). The fact that the 6.51 prediction is vastly different from the 6.5 prediction is an indication of our regression's step-wise nature.

[code language="python"]
# Predicting a new result
y_pred = regressor.predict(6.5)
y_pred_cliff = regressor.predict(6.51)
[/code]

** Step 4: Visualizing Data **

Be careful to plot with finer resolution here. As mentioned, the decision tree regression is a step-wise function, so grainy plots can be significantly more misleading than if the function were smooth.

[code language="python"]
# Visualising the Decision Tree Regression results (higher resolution)
X_grid = np.arange(min(X), max(X), 0.01)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid, regressor.predict(X_grid), color = 'blue')
plt.title('Truth or Bluff (Decision Tree Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
[/code]

![decision-example.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/decision-example1.png)
## Random Forest Regression
This one is a fun one. It takes the concept of decision trees, and makes a forest out of it :) Imagine we're at a promotional booth, where they are giving away a big prize for someone who guesses the correct number of jelly beans in a jar. If we wanted to go down the random forest road, then we'd record every guess that every contestant made, and then we'd average that out in order to make our guess.

[caption id="attachment_7320" align="aligncenter" width="540"]![randomforest](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/random-forest.png) Credit: sklearn documentation[/caption]

In a nutshell, random forest constructs a ton of decision trees and then averages the results of those trees!

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 10 rows and 3 columns. The columns are titled position, level, and salary. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, level is the independent variable, while salary is the dependent variable.

[code language="python"]
# Importing the dataset
dataset = pd.read_csv('Position_Salaries.csv')
X = dataset.iloc[:, 1:2].values
y = dataset.iloc[:, 2].values
[/code]

The RandomForestRegressor library is pretty common, so it turns out feature scaling is automatic. We do not need to pre-process the data any further.

** Step 2: Fitting Data **

The sklearn tool kit provides us with the DecisionTreeRegressor fit method to fit our training dataset. As before, we simply instantiate an object from the class and run the fit method on our X and y inputs.

[code language="python"]
# Fitting Random Forest Regression to the dataset
from sklearn.ensemble import RandomForestRegressor
regressor = RandomForestRegressor(n_estimators = 1000, random_state = 0)
regressor.fit(X, y)
[/code]

** Step 3: Running Prediction(s) **

Running predictions is rather straightforward here. For demonstration purposes, I've run both the desired level 6.5 prediction (150k) and a 6.51 prediction (200k). The fact that the 6.51 prediction is vastly different from the 6.5 prediction is an indication of our regression's step-wise nature.

[code language="python"]
# Predicting a new result
y_pred = regressor.predict(6.5)
y_pred_cliff = regressor.predict(6.51)
[/code]

** Step 4: Visualizing Data **

Be careful to plot with finer resolution here. As mentioned, the decision tree regression is a step-wise function, so grainy plots can be significantly more misleading than if the function were smooth.

[code language="python"]
# Visualising the Random Forest Regression results (higher resolution)
X_grid = np.arange(min(X), max(X), 0.01)
X_grid = X_grid.reshape((len(X_grid), 1))
plt.scatter(X, y, color = 'red')
plt.plot(X_grid, regressor.predict(X_grid), color = 'blue')
plt.title('Truth or Bluff (Random Forest Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
[/code]

![forest-example.png](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/random-forest-example.png)