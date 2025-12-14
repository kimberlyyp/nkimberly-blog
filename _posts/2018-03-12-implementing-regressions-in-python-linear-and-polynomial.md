---
layout: post
title: "Implementing Regressions in Python: Linear and Polynomial"
date: 2018-03-12 06:18:33 +0000
categories: ['Data Science', 'Python']
---

Regression is a popular technique used to model and analyze relationships among variables. There are dozens of models, but I wanted to summarize the six types I learned this past weekend.

While theory was a large component of the class, I am opting for more of a practical approach in this post. The idea is if a person can get started right away with implementing these models, researching the theory and gaining intuition will naturally follow.

Without further ado, here are the six regression implementations I'll cover,

	Simple Linear Regression
	Multiple Linear Regression (+ Backward Elimination)
	Polynomial Regression
	Support Vector Regression
	Decision Tree Regression
	Random Forest Regression

## ![29066543_1575542542562842_5003193298437799936_o](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/29066543_1575542542562842_5003193298437799936_o.png)
*This is part 1 of 2. I will cover simple linear, multiple linear, and polynomial regressions in this post. In the next post, I will cover Support Vector, Decision Tree, and Random Forest Regressions!*
## Simple Linear Regression
Simple Linear Regression is a technique used to model the relationship between a single input independant variable and an output dependant variable using a linear model. It is important to recognize that several assumptions need to be made to use this model appropriately.

	Linear relationship
	Multivariate normality
	No or little multicollinearity
	No auto-correlation
	Homoscedasticity

[Read more about these assumptions here](http://www.statisticssolutions.com/assumptions-of-linear-regression/). As mentioned, I don't want to get too theoretical here. I already had to wade through all that just to get here, so the hope is that this guide will be more of a practicum.

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 30 rows and 2 columns. The columns are titled years experience and salary. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, years of u is the independent variable, while salary is the dependent variable.

[code language="python"]
# Import the dataset
dataset = pd.read_csv('Salary_Data.csv')

# Assign years experience as independent variable
## ':-1' means all columns up to one from the last
X = dataset.iloc[:, :-1].values

# Assign salary as dependent variable
y = dataset.iloc[:, 1].values
[/code]

Then we'll want to split the dataset into training and test sets so that we can test out our regression fit later. Feature scaling is not necessary here as the LinearRegression class handles this for us.

[code language="python"]
# Splitting the dataset into the Training set and Test set
from sklearn.cross_validation import train_test_split
X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.2,random_state=0)
[/code]

** Step 2: Fitting Data **

The sklearn tool kit provides us with the LinearRegression fit method to fit our training dataset.

[code language="python"]
# Fitting Simple Linear Regression to the Training set
## Import LinearRegression
from sklearn.linear_model import LinearRegression

## Instantiate LinearRegression()
regressor = LinearRegression()

## Call fit method on training set
regressor.fit(X_train, y_train)
[/code]

** Step 3: Running Prediction(s) **

Similarly, LinearRegression provides us a predict method to create a y_pred array of predictions.

[code language="python"]
# Predicting the Test set results
## Call predict method on test array
y_pred = regressor.predict(X_test)
[/code]

** Step 4: Visualizing Data **

Because we've got enough points to train and test our dataset here, we'd also be interested in visualizing both.

[code language="python"]
# Visualising the Training set results
plt.scatter(X_train, y_train, color = 'red')
plt.plot(X_train, regressor.predict(X_train), color = 'blue')
plt.title('Salary vs Experience (Training set)')
plt.xlabel('Years of Experience')
plt.ylabel('Salary')
plt.show()

# Visualising the Test set results
plt.scatter(X_test, y_test, color = 'red')
plt.plot(X_train, regressor.predict(X_train), color = 'blue')
plt.title('Salary vs Experience (Test set)')
plt.xlabel('Years of Experience')
plt.ylabel('Salary')
plt.show()
[/code]

![simple-linear-example](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/simple-linear-example.png)

![simple-linear-example-2](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/simple-linear-example-2.png)
## Multiple Linear Regression
Multiple Linear Regression is a technique used to model the relationship between a several input independant variables and an output dependant variable using a linear model. With more than one independent variables, we should be deliberate with which variables we include in our model. In addition to regression implementation, we'll also go over an elimination process to identify which variables we ought to include in our fit.

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 50 rows and 5 columns. The columns are titled R&D Spend, Admin Spend, Marketing Spend, State, and Profit. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, R&D Spend, Admin Spend, Marketing Spend, and State are our independent variables, while Profit is our dependent variable.

[code language="python"]
# Import the dataset
dataset = pd.read_csv('50_Startups.csv')

## Assign independent and dependent matrices
X = dataset.iloc[:, :-1].values
y = dataset.iloc[:, 4].values
[/code]

Before we split the dataset, we need to encode our categorical data, State. We do this by introducing n-1 dummy variables containing binary values. In other words, a single 'State' column is converted to 'New York' and 'California' columns, where indices are tied to a 0 or 1 value depending on whether the state value was NY or CA. If the value was Florida, then there would be 0 values in both the NY and CA columns. We don't add an actual 'Florida' column to avoid the dummy variable trap, where perfect multicollinearity would cause fit issues.

[code language="python"]
# Encode independent categorical data
## Import LabelEncoder and OneHotEncoder
from sklearn.preprocessing import LabelEncoder, OneHotEncoder

## instantiate labelencoder_X from LabelEncoder class
labelencoder_X = LabelEncoder()

## fit to x
labelencoder_X_fit = labelencoder_X.fit_transform(x[:,3])

## assign transformed matrix to X
X[:,3] = labelencoder_X_fit

## instantiate onehoteencoder_X from OneHotEncoder class with col 0 as category
onehotencoder_X = OneHotEncoder(categorical_features=[3])

## fit transform to X and convert to array
onehotencoder_X_fit = onehotencoder_X.fit_transform(X).toarray()

## assign transformed matrix to X
X = onehotencoder_X_fit

# Avoiding the Dummy Variable Trap
## Remove first column, one of the dummy variable columns
X = X[:, 1:]
[/code]

Now we'll want to split the dataset into training and test sets so that we can test out our regression fit later. Feature scaling is not necessary here as the LinearRegression class handles this for us.

[code language="python"]
# Splitting the dataset into the Training set and Test set (80%/20% split)
from sklearn.cross_validation import train_test_split
X_train,X_test,y_train,y_test=train_test_split(X,y,test_size=0.2,random_state=0)
[/code]

** Step 2: Fitting Data **

The sklearn tool kit provides us with the LinearRegression fit method to fit our training dataset.

[code language="python"]
## Import LinearRegression
from sklearn.linear_model import LinearRegression

## Instantiate LinearRegression()
regressor = LinearRegression()

## Call fit method on training set
regressor.fit(X_train, y_train)
[/code]

This fit automatically takes all variables and generates a model from them. We know, however, this is not an optimized model. Let's take a quick detour to examine backward elimination process. Annotation is provided in code.

[code language="python"]
# Building optimal model using Backward Elimination

## Import csv library for outputting summary tables
import csv
## Statsmodel library, does not automatically integrate b0
import statsmodels.formula.api as sm
## Add column of 1's to account for b0 the intercept
X = np.append(arr=np.ones((50, 1)).astype(int), values=X, axis=1)

## Initialize optimized matrix of features
X_opt = X[:, [0, 1, 2, 3, 4, 5]]

# Step 1: Identify significance level
alpha = 0.05

# Step 2: Fit full model with all possible predictors
## Instantiate regressor as fit of X and y
regressor_OLS = sm.OLS(endog = y, exog = X_opt).fit()
## Print summary
elim_1 = regressor_OLS.summary(alpha = alpha).as_csv
res = [elim_1]
csvfile = 'Elim_1.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    writer.writerow(res)
elim_1

# Step 3: Eliminate predictor with highest P value > 0.05
X_opt = X[:, [0, 1, 3, 4, 5]]

# Step 2: Refit model with elimination
regressor_OLS = sm.OLS(endog = y, exog = X_opt).fit()
## Print summary
elim_2 = regressor_OLS.summary(alpha = alpha).as_csv
res = [elim_2]
csvfile = 'Elim_2.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    writer.writerow(res)
elim_2    

# Step 3: Eliminate the predictor of highest P value > 0.05
## Here that would be x1: column 1
X_opt = X[:, [0, 3, 4, 5]]

# Step 2: Refit model with elimination
regressor_OLS = sm.OLS(endog = y, exog = X_opt).fit()
## Print summary
elim_3 = regressor_OLS.summary(alpha = alpha).as_csv
res = [elim_3]
csvfile = 'Elim_3.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    writer.writerow(res)
elim_3

# Step 4: Remove the predictor of highest P value > 0.05
## Here that would be x2: column 2
X_opt = X[:, [0, 3, 5]]

# Step 2: Refit model with elimination
regressor_OLS = sm.OLS(endog = y, exog = X_opt).fit()
## Print summary
elim_4 = regressor_OLS.summary(alpha = alpha).as_csv
res = [elim_4]
csvfile = 'Elim_4.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    writer.writerow(res)
elim_4

# Step 3: Remove the predictor of highest P value > 0.05
## Here that would be x2: column 2
X_opt = X[:, [0, 3]]

# Step 2: Refit model with elimination
regressor_OLS = sm.OLS(endog = y, exog = X_opt).fit()
# Print summary
elim_5 = regressor_OLS.summary(alpha = alpha).as_csv
res = [elim_5]
csvfile = 'Elim_5.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    writer.writerow(res)
elim_5

# Step 3: Remove the predictor of highest P value > 0.05
## There are no more eliminations to be made

# Compile summaries used in eliminations
res = [elim_1, elim_2, elim_3]
csvfile = 'Eliminations.csv'
with open(csvfile, "w") as output:
    writer = csv.writer(output, lineterminator='\n')
    for val in res:
        writer.writerow([val])       

#======================================================#

# Fitting Simple Linear Regression to the optimized set
X_vis = X_opt[:,1:2]
## Import LinearRegression
from sklearn.linear_model import LinearRegression
## Instantiate LinearRegression()
regressor = LinearRegression()
## Call fit method on training set
regressor.fit(X_vis, y)

# Visualize final model
plt.scatter(X_vis, y, color = 'red')
plt.plot(X_vis, regressor.predict(X_vis), color = 'blue')
plt.title('Profit vs. Engineering Spend')
plt.xlabel('Engineering Spend')
plt.ylabel('Profit')
plt.show()
[/code]

If we want to automate this for the future, here are some implementation examples. The first is based only on p-value, while the second considers the R_squared to help us generate an optimized fit.

[code language="python"]
############################################################
Option A
############################################################

# Backward Elimination with p-values only
import statsmodels.formula.api as sm

## Write program
def backwardElimination(x, sl):
    numVars = len(x[0])
    for i in range(0, numVars):
        regressor_OLS = sm.OLS(y, x).fit()
        maxVar = max(regressor_OLS.pvalues).astype(float)
        if maxVar > sl:
            for j in range(0, numVars - i):
                if (regressor_OLS.pvalues[j].astype(float) == maxVar):
                    x = np.delete(x, j, 1)
    regressor_OLS.summary()
    return x

 ## Add column of 1's to account for b0 the intercept
X = np.append(arr=np.ones((50, 1)).astype(int), values=X, axis=1)

## Initialize optimized matrix of features
X_opt = X[:, [0, 1, 2, 3, 4, 5]]

## Define significance level
SL = 0.05

## Run program
X_Modeled = backwardElimination(X_opt, SL)

############################################################
Option B
############################################################

# Backward Elimination with p-values and Adjusted R Squared
import statsmodels.formula.api as sm

## Write Program
def backwardElimination(x, SL):
    numVars = len(x[0])
    temp = np.zeros((50,6)).astype(int)
    for i in range(0, numVars):
        regressor_OLS = sm.OLS(y, x).fit()
        maxVar = max(regressor_OLS.pvalues).astype(float)
        adjR_before = regressor_OLS.rsquared_adj.astype(float)
        if maxVar > SL:
            for j in range(0, numVars - i):
                if (regressor_OLS.pvalues[j].astype(float) == maxVar):
                    temp[:,j] = x[:, j]
                    x = np.delete(x, j, 1)
                    tmp_regressor = sm.OLS(y, x).fit()
                    adjR_after = tmp_regressor.rsquared_adj.astype(float)
                    if (adjR_before >= adjR_after):
                        x_rollback = np.hstack((x, temp[:,[0,j]]))
                        x_rollback = np.delete(x_rollback, j, 1)
                        print (regressor_OLS.summary())
                        return x_rollback
                    else:
                        continue
    regressor_OLS.summary()
    return x

 ## Add column of 1's to account for b0 the intercept
X = np.append(arr=np.ones((50, 1)).astype(int), values=X, axis=1)

## Initialize optimized matrix of features
X_opt = X[:, [0, 1, 2, 3, 4, 5]]

## Define significance level
SL = 0.05

## Run program
X_Modeled = backwardElimination(X_opt, SL)
[/code]

** Step 3: Running Prediction(s) **

Take the optimized X matrix now and make the prediction with that (X_opt), as opposed to the original X matrix.

[code language="python"]
# Predicting the Test set results
## Call predict method on test array
y_pred = regressor.predict(X_opt)
[/code]

** Step 4: Visualizing Data **

If we're being strict with backward elimination at significance level of 0.05, then we're only left with Engineering Spend as an independent variable. Thus, the visualization is rather simple.

![mult-linear-regression-example](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/mult-linear-regression-example.png)

[code language="python"]
# Visualize final model
X_eng = X_Modeled[:,1:2]
X_mark = X_Modeled[:,2:3]
from sklearn.linear_model import LinearRegression

regressor_eng = LinearRegression()
regressor_eng.fit(X_eng, y)
plt.scatter(X_eng, y, color = 'red')
plt.plot(X_eng, regressor_eng.predict(X_eng), color = 'blue')
plt.title('Profit vs. Engineering Spend')
plt.xlabel('Engineering Spend')
plt.ylabel('Profit')
plt.show()
[/code]
## Polynomial Regression
Polynomial Regression is appropriate to use when modeling non-linear relationships among variables.

**Step 1: Pre-processing Data**

Let's say we've got a dataset of 10 rows and 3 columns. The columns are titled position, level, and salary. The first thing we'll want to do is import this dataset and assign our independent matrix X and dependent array y. Here, level is the independent variable, while salary is the dependent variable.

[code language="python"]
# Importing the dataset
dataset = pd.read_csv('Position_Salaries.csv')
X = dataset.iloc[:, 1:2].values
y = dataset.iloc[:, 2].values
[/code]

Since this is a pretty small dataset, we won't split into training and test sets. Also, feature scaling is not necessary here as the LinearRegression class handles this for us. We will, however, need to transform our X matrix to polynomial of desired degree. Let's say we choose a 5th degree polynomial.

[code language="python"]
# Fitting Polynomial Regression to the dataset
from sklearn.preprocessing import PolynomialFeatures
poly_reg = PolynomialFeatures(degree = 5)
X_poly = poly_reg.fit_transform(X)
poly_reg.fit(X_poly, y)
[/code]

** Step 2: Fitting Data **

Now we can fit the data. Let's do both the simple linear and polynomial, for comparison purposes. We will name them lin_reg_1 and lin_reg_2, respectively.

[code language="python"]
# Fitting Linear Regression to the dataset
from sklearn.linear_model import LinearRegression
lin_reg = LinearRegression()
lin_reg.fit(X, y)

# Fitting Polynomial Regression to the dataset
lin_reg_2 = LinearRegression()
lin_reg_2.fit(X_poly, y)
[/code]

** Step 3: Running Prediction(s) **

By making a prediction with simple linear vs. polynomial, we see how important it is to choose the right model. With simple linear, our prediction result is 330k for the salary of someone at level = 6.5. With polynomial of degree 5, we get that the salary is 174k-- much more reasonable, considering our real value is 160k.

[code language="python"]
# Predicting a new result with Linear Regression
lin_reg.predict(6.5) #330k

# Predicting a new result with Polynomial Regression
X_desired = poly_reg.fit_transform(np.array([[6.5]]))
lin_reg_2.predict(X_desired) #174k
[/code]

** Step 4: Visualizing Data **

Let's visualize both linear and polynomial for comparison sake.

[code language="python"]
# Visualising the Linear Regression results
plt.scatter(X, y, color = 'red')
plt.plot(X, lin_reg.predict(X), color = 'blue')
plt.title('Truth or Bluff (Linear Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()

# Visualising the Polynomial Regression results
plt.scatter(X, y, color = 'red')
plt.plot(X, lin_reg_2.predict(X_poly), color = 'blue')
plt.title('Truth or Bluff (Polynomial Regression)')
plt.xlabel('Position level')
plt.ylabel('Salary')
plt.show()
[/code]

![poly-example-1](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/poly-example-1.png)

![poly-example-2](https://nkimberly.wordpress.com/wp-content/uploads/2018/03/poly-example-2.png)