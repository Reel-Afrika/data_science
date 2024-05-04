# Store Sales Prediction
Sales depend on the day of the month, time of the day, promotion, offers, seasonality, etc. Sales prediction will help management plan for when to source for stock, and in what quantities before they run out. It will also help determine when to run seasonal or day-wise offers to attract more people to the store.

## Problem Definigion
[TBA]

[Sample Dataset](https://www.kaggle.com/c/rossmann-store-sales/data)

## Possible Solution - Regression Analysis
**Regression Analysis** is a set of statistical processes for estimating the relationships between a dependent variable / label and one _or more_ independent variables (predictors, covariates, features).

Regression analysis is primarily used for two conceptually distinct purposes.

First, it is widely used for prediction and forecasting, where its use has substantial overlap with the field of machine learning.

Second, in some situations regression analysis can be used to infer causal relationships between the independent and dependent variables. Importantly, regressions by themselves only reveal relationships between a dependent variable and a collection of independent variables in a fixed dataset. To use regressions for prediction or to infer causal relationships, respectively, a researcher must carefully justify why existing relationships have predictive power for a new context or why a relationship between two variables has a causal interpretation. The latter is especially important when researchers hope to estimate causal relationships using observational data.

### Linear Regression
In linear regression, we try to find the line that most closely fits the data according to a specific mathematical criterion. foo

### Logistical Regression

## Analysis

This table has 9 columns and 1,017,209 rows.

## Column Definitions

| Columns                              | Descriptions                                              |
| :----------------------------------- | :-------------------------------------------------------- |
| Id | An Id that represents a row, starting from 0 |
| Store | A unique Id for each store. The range of values is from 1 to 1115 |
| DayOfWeek | Numerical representation of the day of the week, between 1 and 7 |
| Date | YYYY-MM-DD formatted date |
| Sales | The turnover on a given day. From the table, the mean is **5,773.82**, the minimum **0.00** and the maximum **41,551.00**. This is potentially a value that we need to predict. |
| Customers | The number of customers on a given day. From the table, the mean is **633.15**, the minimum **0.00** and the maximum **7,388.00**. Because the data in this column represent individual persons, we should consider truncating the decimal values when sanitizing the data or performing any computations on the data in this column. |
| Open | open: 0 = the store is closed , 1 = the store is open |
| Promo | Promo (1) or not (0) on that day |
| StateHoliday | Indicates a state holiday.  |
| SchoolHoliday | Store on this Date was affected or not by the closure of public schools |

## Additional Notes

### Store
The data under the `Store` column has no unique values. The mean is **558.43**, the standard deviation **321.91**, the smallest value **1**, the largest value **1,115**. The values at the 25th percentile is **280**, at the 50th percentile **558** and at the 75th percentile **838**. By conclusion the value at the 100th percentile is **1,115**. A closer inspection shows that the values under the `Store` column form a series of unique values from 1 - 1,115, and are repeated multiple time through the 1,017,209 rows of data.

**Qn: How many times are these?**

This leads us to believe that:

- the values under the `Store` column are unique ids representing all the 1,115 stores under the Rossmann enterprise.
- data for a specific date is collected across all the 1,115 stores. We can observe this by examining the `Date` column. The frequency is **1,115**, meaning that we are repeating a single date 1,115 times.
- we have *roughly* 942 data points for each of the 1,115 stores under the Rossmann enterprise. We can observe this by examining the `Date` column. There are 942 unique date values. However, because _942 x 1115 = 1,050,330_, which is more than _1,017,029_ records in the data table, we can conclude that there are some stores that have missing data on some dates.
- the mean does not provide us a meaningful value because we cannot compute fractionals on entity ids

### DayOfWeek
The data under the `DayOfWeek` column has no unique values. The mean is 4, the standard deviation 2, the smallest value 1, the largest value 7. The value at the 25th percentile is 2, at the 50th percentile 4 and at the 75th percentile 6.

A closer inspection shows that the values under the `DayOfWeek` column form a series of values from 1 - 7, and are repeated multiple times through the 1,017,209 rows of data.

Looking at the above example, we notice that `DayOfWeek => 5` falls on _July 31st, 2015_ for all the 1,114 records. Looking at the calendar we see that _July 31st, 2015_ falls on a Friday. `DayOfWeek => 4` falls on _July 30th, 2015_ for all the 1,114 records. Looking at the calendar we see that _July 30th, 2015_ falls on a Thursday.

Assuming that Day 1 falls on a Monday, and Day 4 on a Thursday, and Day 5 on a Friday, we can conclude that the values under the `DayOfWeek` column represent the days of a 7-day week. This information could potentially help us determine whether the `DayOfWeek` had any impact on the numbers of customers that visited the Rossmann stores, and, by extension, the sales that the Rossmann stores made on any particular day

### Date

`Date` seems to go hand-in-hand with `DayOfWeek`. So, when analysing this data to predict sales we can leverage the `DayOfWeek` value to determine what day of the week that was.

It would be beneficial to convert the `DayOfWeek` values into actual days namings, such as:
- 1: Monday
- 2: Tuesday
- 3: Wednesday
- 4: Thursday
- 5: Friday
- 6: Saturday
- 7: Sunday

Without any other sources of information (besides whether it was a school holiday or a state holiday) that could lend more context on the `Date` or `DayOfWeek` values, it is hard to assess what kind of influence these values could have on the prediction of the values of sales on any particular day. So, one consideration when cleaning this data is to drop the `Date` and `DayOfWeek` columns.

### Sales
The `Sales` column has unpredictable values just by looking at the mean, min, std, max, and percentile information. This is one of those data points we may want to predict based on the rest of the information in the sales table, such as:
- the day of the week,
- whether or not it was a school holiday or a state holiday,
- whether or not there was a promotion,
- whether or not a store was opened, and
- whether the number of customers on any given day at a particular store had any impact on sales

### Customers
The `Customers` colum has a mean of **633.15**. Since we cannot split a person into fractions when doing any computations on customers we should consider truncating
any decimal values to whole values. For example **633.15** should be _~633_.

Optionally, when sanitizing the data, we should do the truncation at that point.

### Open & Promo & SchoolHoliday

From the code snippets on `Promo`, `Open` &

### StateHoliday
We are not sure what the distinct values under this column represent. One of the ways we can determine what the actual values represent is to see on what dates the values land on and guess based on what we know. For example

- New Years lands on 1/1/YYYY
- Chrismas lands on 12/25/YYYY
- It is not straightforward to determine when Easter falls because the dates move, so, probably worth checking when Easter landed during this time period
- For any other holiday that is not considered an internationally recognized holiday such as Easter, Christmas and New Years, we could conclude that a value other that "0" that lands on any of the rows for the "StateHoliday" could be a public holiday
- This is because, we cannot presume to know which country this sales data comes from.


**Qn: What are the ranges of values of 'StateHoliday'?**
