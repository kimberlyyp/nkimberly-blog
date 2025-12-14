---
layout: post
title: "QA in Business"
date: 2017-12-22 04:54:43 +0000
categories: ['Business Administration']
---

*Yet it should be noted that no matter how mathematically precise the tools of quantitative analysis may appear, they are no substitute for an MBA's own best judgment.*

# Quantitative Analysis

	Decision Tree Analysis
	Cash Flow Analysis
	Net Present Value
	Probability Theory
	Regression Analysis and Forecasting

### I. Decision Tree Analysis
Decision theory teaches us how to break complex problems into manageable parts. For example, quantitative analysis can be used to help a company decide whether to drill for oil. The inherent risks of oil exploration cannot be eliminated, but they may be simply managed responsibly and reasonably.

A **decision tree** can organize a problem's alternatives, risks, and uncertainty. To prepare such a diagram:

	Determine all possible alternatives and risks associated with the situation.
	Calculate the monetary consequences of each of the alternatives.
	Determine the uncertainty associated with each alternative.
	Convert the first three steps into a tree diagram.
	Determine the best alternative and do consider the nonmonetary aspects of the problem.

Decision tree diagrams include activity forks and event forks where alternatives are possible. Activity forks are where decisions can be made, while event forks are determined by the probability of an event.

One of the best ways to get some practice creating these diagrams is to create one ourselves. I created the following diagram through [SilverDecisions](http://silverdecisions.pl/SilverDecisions.html?lang=en) with the following scenario,

	Sam paid $20k for the drilling option.
	Sam could lower his risks if he hired a geologist to perform seismic testing ($50k). That would give him a better indication of success and lower his risk of wasting drilling costs.
	Should he roll the dice and incur $200k in drilling costs without a seismic evaluation to guide him?
	Sam consulted with oil experts. They believe Sam's parcel has a 60% chance of having oil without the benefit of any tests.
	It has also been the experts' experience that if seismic tests are positive for the oil, there is a 90% chance there is "some" oil. And conversely, there is a 10% chance of failure.
	If those seismic tests are negative, Sam could still drill but with a 10% chance of success and a 90% chance of failure.
	Sam could decide not to drill at all.

# ![decision-tree.png](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/decision-tree-e1513917465785.png)
As it turns out, the expected monetary value of going ahead without tests is 400k, while drilling with testing is only $370k. The decision dictated by the tree is to forgo the tests.

While this is impressive, I can already imagine a multitude of situations where such a method simply would not work. In an ideal world, we would have all the information. In the real world, rarely is that the case. The probability of many events is difficult to quantify. Worse yet, I sense the potential for instability in this sort of model. If an estimate in one area of the tree is slightly off, it is possible that a minor correction of the estimate can cause wild fluctuations throughout the tree.

Despite these possible limitations, I have used the decision tree successfully to identify the best possible outcomes of a negotiation. As with most tools, its effectiveness lies in its situational appropriateness. Simply preparing a decision tree also helps with clarifying the problem.
### II. Cash Flow Analysis
A commonly used representation of the timing of cash flows is a bar graph. Each period, cumulative cash flow is reflected either below the line for cash investments or above the line for returns.
# ![25637278_1495579197225844_366237284_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/25637278_1495579197225844_366237284_o-e1513917694572.jpg?w=1024)
In the first depiction, the company receives an almost immediate return on investment of 163k after year 1.

![25637278_1495579197225844_366237284_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/25637278_1495579197225844_366237284_o1-e1513917759121.jpg?w=1024)

In the second depiction, the company must work through 2 years before it receives the 163k return at the end of year 3.

In the first scenario, the company will have a higher accumulated value than that in the second scenario. That's because, in the first scenario, the company can reinvest the returns into opportunities that yield, say, 10%. The company in the second scenario cannot. Thus by the end of year 3, the company in the first scenario will have 163k + earned interest of 34k equalling 197k.

![25637278_1495579197225844_366237284_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/25637278_1495579197225844_366237284_o2-e1513917812216.jpg?w=1024)

This last depiction is more common. We see that an investment of 102k can return about 51k the following two years and 61k the year after. This scenario allows the company to at least reinvest 51k after year 1, so on and so forth. Understanding this can help to put into perspective why we sometimes feel the need to rush development time order to generate revenue earlier on.
### III. Net Present Value
NPV tries to take into account inflation and timing of cash flow in order to make good decisions today. In general, a dollar received today is worth more than a dollar received in the future. Using this simple logic, NPV analysis takes future cash flows and discounts them to their present-day value. The discount rate used here is 10% arbitrarily. In practical applications, they would need to be obtained through an analysis of inflation and investment opportunities.
```
  NPV = ($ in future) x (1 + discount rate) ^ (# of periods)

```
Thus, a dollar received a year from now would be worth,
```
  $1 x 1/(1+10)^-1 = $1 / 1.1 = 91 cents

```
If we take a look at our previous example again, we can use NPV to calculate how much the project would be worth, when the timing is accounted for.

Year
Future Cash
Discount Factor
NPV today

0
-102k
1
-102k

1
51k
0.90
46k

2
51k
0.83
42k

3
61k
.75
46k

Without NPV, the project value would have been inaccurately estimated to be,
```
  -102k + 51k + 51k + 61k = 61k

```
With NPV, we see that the project would more accurately be valued at,
```
  -102k + 46k + 42k + 46k = 32k

```
### IV. Probability Theory
In situations where multiple outcomes are possible, the result is a distribution of outcomes. As with the event fork, all possible outcomes of any event must add up to 100%. When there are only two outcomes (win/lose, yes/no, success/fail), the distribution is more specifically referred to as a **binomial distribution**.

The binomial distribution has practical applications for assessing probabilities for portfolio managers, sales directors, and research analysts. Take the case of AT&T share prices from 1957 to 1977. Each month, share prices were examined to determine the rate of positive returns. It was found that 56.7 percent of the time, there was a success. Using this probability of success, we can assemble a binomial table to predict the number of months of positive returns during a given quarter.

# of Successes
Frequency Expected

0
0.082

1
0.318

2
0.416

3
0.184

Compared to what actually occurred, the theory matches fairly well.

# of Successes
Frequency Occurred

0
0.088

1
0.325

2
0.387

3
0.200

Then there's the popular **normal distribution**. When the distribution of data represents a normal curve, the mean and standard deviation are powerful key concepts that allow us to better understand the data.

Let's say a shoe store owner purchases research data on women's shoe sizes, plot it, and finds that the distribution is normal. From this simple bell curve, the owner can already make some key assumptions about how many of each shoe size he should carry at his store. For example, based on the following plot, the owner would know to stock sizes 3-11 if he wanted to cover 95% of women's feet sizes. This is because 3 and 11 are each two standard deviations away from the mean, covering a range of 4 standard deviations, which by definition provides coverage of 47.7% * 2 = 95.4% of the studied population.
# ![25855954_1495624347221329_1765794240_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/25855954_1495624347221329_1765794240_o.jpg?w=1024)
### V. Regression Analysis and Forecasting
This section is relatively straightforward. Just remember our standard slope equation,
```
  Y = mx + b

```
Take data, find the best fit, calculate R-squared, and evaluate numerics to figure out how to interpret the data. Then remember, of course, correlation is not causation ;)

I won't entertain this section too much, as it's just your basic stats class. In case a refresher is needed, T-stat above 2 shows a significant correlation between two variables and a high R-squared tells us that the variation in the data is mostly explained by the regression equation.

One cool thing I learned in this section that isn't readily taught in stats class is the use of a **dummy variable** in regression analysis. Dummy variables can be used to represent conditions that are not or are difficult to be measured numerically. For example, if we simply wanted to track whether there was any relationship between certain offerings and sales, we can use binary 9 & 1 dummy variables. In the following example, a 0 indicates a "hot" toy at a toy store being out of stock while a 1 indicates the toy is in stock.
# ![25673172_1495624343887996_1102954840_o](https://nkimberly.wordpress.com/wp-content/uploads/2017/12/25673172_1495624343887996_1102954840_o.jpg?w=1024)
Evaluating these 7 observations leads to an R-squared of 1, a standard error of 0, and a T-stat of 65535. This T-stat is enormous first of all, and the R-squared is 100%, indicating that the in-stock condition is very much correlated with $200k sales while out of stock condition correlates to $100k sales.

## From the author,
*After I earned my MBA, I had a chance to reflect on the two most exhausting and fulfilling years of my life. As I reviewed my course notes, I realized that the basics of an MBA education were quite simple and could easily be understood by a wider audience...*

![ten-day-DSC_1655-1038x576](https://nkimberly.wordpress.com/wp-content/uploads/2017/10/ten-day-dsc_1655-1038x576.jpg?w=300)

*The basics of MBA knowledge fall into nine disciplines. Some schools have carefully crafted their own exalted names for each subject, but their unglorified names are:*

	*Marketing*
	*Ethics*
	*Accounting*
	*Organizational Behavior*
	*Quantitative Analysis*
	*Finance*
	*Operations*
	*Economics*
	*Strategy*

This particular post covers Day 5: Quantitative Analysis.