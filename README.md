## Propane Prices
I've been observing the flunctuations in gas prices for some years and I do wonder if consumers have ever had the oppotunity to enjoy a low price after a hike. The [National Propane Gas Association](https://www.npga.org/news-resources/2020-residential-energy-consumption-survey-recs/) noted that 9% of U.S. households utilize propane for at least one residential application excluding grilling; I'm just curious to see if these consumers have had any chance to enjoy low prices in the past or if there could be one in the future. In this analysis, I looked at the price of propane over some years and also create a model to see if there is any relieve for consumers in 2025. 
I used the [New York Average Propane Price Data](https://catalog.data.gov/dataset/average-residential-retail-propane-prices-by-region-beginning-1997-4888e) for this analysis.

## Data Analysis
Data is any record that might be text (structured or unstructured), vedio, audio, graph etc that could be used for reference purposes. Data analysis is the process of extracting facts from data for human or business decision. 
To analyze data in Python, we must first import the necessary ***libraries***. See my write up on [Python Libraries](https://github.com/dataglyder/Basic-Python-Libraries.io) for more details.

## Open Data File with Pandas
First import Pandas
``` Python
import pandas as pd        
data = pd.read_csv("file name or file path")
print(data.head())           # Use display/print to view the whole table or display/print(data.head()) to view first 5 rows.
```
![first five rows](https://github.com/dataglyder/Propane-Prices.io/blob/main/head.png)

The data is from 1997 to 2024; each month within each each year has price imput of at least 2 i.e., at the beggining and at the end of the month. I will not fill-in each month with its average price in other to have a full month because:
**1** There is no indication of missing values
**2** This will not change the average price of each month.

```Python
print(data.isnull().sum())     # To confirm that no value was missing.
```
![confirm empty data](https://github.com/dataglyder/Propane-Prices.io/blob/main/isnul.png)

## Select Needed Columns From Data in Python
Often times, not all the columns in the dataframe are usefull for ones analysis; so, for easier handling of the data table, one can select only necessary columns. ***".loc"*** could be used as in the code below or the [double square bracket method](https://pandas.pydata.org/docs/user_guide/indexing.html) can also be used. 
``` Python
needed_cols = data.loc[:["Data", "New York Statewide Average ($/gal)"]] # ":" selected all the rows and "Data" and
                                                                          # "New York Statewide Average ($/gal)"   
                                                                             # were the columns selected.
```
## Change Column Name in Python
This is usually done to rename the columns. In this case, I will rename the columns to change all the characters to lower case and also to shorten the names.
``` Python
needed_cols = data.rename(column = {"old_name1":"new_name1", "old_name2":"new_name2"}]
needed_cols = data.rename(column={"Data":"data", "New York Statewide Average ($/gal)":"ny_state_avg_price"})
```
## Working with Date in Python
Let's split the date column into Month and year using Pandas [Pandas.to_datetime](https://pandas.pydata.org/docs/reference/api/pandas.to_datetime.html) additonal information is available on  [Python datetime](https://docs.python.org/3/library/datetime.html#module-datetime). Although some IDE might work with datetime without first importing datetime, it is better to import datetime to be on the saver side.
I first converted the date to Pandas datetime and then create new columns for the months and the years.
``` Python
from datetime import datetime
needed_cols.date = pd.to_datetime(needed_cols.date)
needed_cols["month"], needed_cols["year"] = needed_cols.date.dt.month, needed_cols.date.dt.month
```
Here is what the new table look  like:

![Cleaned tabled](https://github.com/dataglyder/Propane-Prices.io/blob/main/cleaned_table.png)

Now that the data is ready, let's start the analysis.
## Histogram
We'll view the shape of the data but befote then, let's import another library for visualization. 
``` Python
import matplotlib.pyplot as m
import seaborn as s
hist_chart=p.figure(figsize=(6,5), layout="constrained")
s.histplot(needed_cols, x="ny_state_avg_price", bins=20, element="step")
m.ylabel("frequncy")
m.show()
```
![Histogram of Average Price Between 1997 to 2024](https://github.com/dataglyder/Propane-Prices.io/blob/main/Hist_of_data.png)

The chart shows the price on the x-axis and its frequencies on the y-axis. It is right  skewed i.e. price mostly move towards right side of the chart and it looks like it's bi-modal i.e., has two peak periods. Let's lay a density curve on the chart to ascertain the bi-modal shape.

## Density Curve
Density curve summarises the approximate shape and the pattern a data distribution.
``` Python
s.histplot(needed_cols, x="ny_state_avg_price", bins=20, element="step", kde=True)
m.show()
```
![Bi_modal Density Chart of Price](https://github.com/dataglyder/Propane-Prices.io/blob/main/density_c_wit_chart.png)
## Density Curve without bars or Elements
For better clarity, I will view the modal density shape without the bars or elements.
``` Python
s.displot(needed_cols, x="ny_state_avg_price", kind = "kde"
m.show()
```
![Bi_modal Curve](https://github.com/dataglyder/Propane-Prices.io/blob/main/density_c_witht_chart.png)

The distribution appears to be bi-modal, one peak at around price $1.5 and another peak period around price $3.0

## Box Plot
Let's examine the average price within each year with box plot.

```Python
fig, ax=m.subplots(figsize=(10,8))
s.boxplot(x=needed_cols["year"], y=["ny_state_avg_price"]
ax.ticks_params(rotation=90)
m.show()
```
![boxplot](https://github.com/dataglyder/Propane-Prices.io/blob/main/boxplot.png)
The boxplot summarises the price for each year.The "T" extensions show the minimum (bottom) and maximumm (top) price for the year. The horizontal line that cut through the box indicate the median for the year. The spade shape around the box indicate [outliers](). There seems to have been some relieve in the past: price drop in 2002 after the increase in 2001,it also drop in 2009 after a preceeding consistent hike in price and the most recent in hike was in 2022 before a bit of relief in 2023 and 2024. 

## Statistical Analysis
Let's take some sample and carryout statistical analysis on the sample. Year 2003 and 2004 are very recent; are there any sigificant difference between their price? Let's check with a T- statistics. You can check my write up on [traditional way of conductiong statistical analysis](). Let's quickly run it with Python here.

**Let's state our hypothesis**

**Null hypothsis $`H_{0}`$**: There is no significant difference between the price of propane in 2023 and 2024; $`\mu_{2023} = \mu_{2024}`$

**Alternamte hypothesis $`H_{1}`$** There is significance difference between the price of propane in 2023 and 2024 $`\mu_{2023}\neq\mu_{20}`$

Now, let's filter the needed years before we import the necesarry library.
## Filter Columns in python
```Python
filtered_column = data[data["column"].isin([value/"sting" to be filtered])
yr_2023_sample = needed_cols[needed_cols["year"].isin([2023]).sample(n=25, random_satate=42)
yr_2024_sample = needed_cols[needed_cols["year"].isin([2024]).sample(n=25, random_state=42)
```
We just filtered year 2023 and 2024 from the dataframe and took 25 random samples from each year. This sample is sensitive to randomness hence the use of random.state to fodster reproduibility The use of another value for random.state might might alter the result and not using random_state at all might even generate opposite result to mine.
```Python
import scipy.stats
sig_test = scipy.stats.ttest_ind(yr_2023_sample["ny_state_avg_price"], yr_2024_sample["ny_state_avg_price"])
print(sig_test)
```
![image](https://github.com/dataglyder/Propane-Prices.io/blob/main/sig_test_with_rand_ts.png)

Let's assume we are using confidence level of 95%, our level of significance alpha $`\alpha`$ will be 0.05. Since our calculated p-value (0.2635  is greater than our chosen $`\alpha`$ (0.05) we will fail to reject the null hypothesis. Therefore, there is no significant difference between the price of propane in 2023 and 2024.

## Regression Analysis
Regression analysis is used to predict how a change in variable x might cause a change in variable y. I will be using linear regression and it could be expressed  mathematically as $`y=m+cx.
where:

y = dependent variable in this analysis, the "price"

x = independent variable in this analysis, the "day"

c= coefficient of x or the slope or change in x

m= is the intercept i.e point at which the regression line meets the y-axis

Before we move forward, it is important to establish that the fact that y change (**correlate**) when there is change in x does not means that x is the root cause of change in y; hence the popular saying ***correlation does not equal causation***.

Linear Regression in simple graphical representation is just scatter plot combine with line graph. We'll use regression to see how change in years might have influnce change in price between 2000 to 2024.
## Scatter Plot
```Python
from_2000 = pd.DataFrame(needed_cols[needed_cols["year"]>1999])    #filter the needed years
from_2000["day"] = [daz for daz in range(len(from_2000))           # Days in those years that are selected
fig, ax=m.subplots(figsize=(6,8))
m.scatter(from_2000["day"],from_2000["ny_state_avg_price"])
m.xlabel("day")
m.ylabel("price")
                            

```
![scatter_plot](![Uploading image.png…](https://github.com/dataglyder/Propane-Prices.io/blob/main/scatterplot.png)


Each point on the scatter plot represent price from from its corresponding day. Now, let's fit the regression line to the scatter plot to visualize how many points will fall exactly on the line or be closser to the line.

## Scatter Plot + Line Plot = Linear Regression Chart
```Python
import sklearn
from sklearn.linear_model, import LinearRegression
# Reshape necessary columns to 2-D to facilitate analysis
x=from_2000.day.values.reshape(-1,1)
y=from_2000.ny_state_avg_price.values.reshape(-1,1)
regression = LinearRegression()
regression.fit(x,y)
predict_y=reg.predict(x)
m.plot(x,predict_y
m.show()

```
![reg_analysis](https://github.com/dataglyder/Propane-Prices.io/blob/main/linear_reg_day.png)

The regression line travels from left to right indicating a positive correlation. For instance, as the days progress the price somewhat increase. Let's confrim the correlation
```Python
correlation = from_2000["day"].corr(from_2000["ny_state_avg_price"])
print("correlation =", correlation)
```
![cor](https://github.com/dataglyder/Propane-Prices.io/blob/main/cor.png)

The correlation is positive because it's greater than zero (0) and it's very strong because it's more than 0.5

## Regression Equation

***$`y = m + cx`$ could be rewritten as:***

***$` price = intercept + slope(day)`$***

Since we have our regression chart, we can estimate the intercept and the coefficient  from it or calculte them. Let's do the latter.
```Python
regression.fit(x,y)
coeffi = regression.coef_
intercet = regression.intercept_
print("coefficient = ", coeffi, "intercept = ", intercet)
```
![coeffandinter](https://github.com/dataglyder/Propane-Prices.io/blob/main/coeff_an_interc.png)

Now that we have our slope and intercept, the regression equation from which our regression model is formed becomes:

## Regression Model
price = 1.7929 + 0.0017(day)
This is developed from regression equation and our calculated variables like the intercept and coeficient.

So, if we pick a day say day 200, we can check the price for that particular day i.e., $`price = 1.7929 + 0.0017(200) which is approximately ($2.13)`$. But, this is from day in the present. How about the future?

## Predicting the Future with Linear Regression
Our linear regression chart stops where the days ends(i.e., last day of Dec. 2024) we can extend the day to how many days we want in 2025 by extending the regression line to the right as in:
```Python
import numpy as n
last_day_in_2024 = from_2000["day"].max()     # the value is 931
future_daz_in_2025 = np.append(from_2000["day], n.arange(932, 1022))

```
Future days in 2025 were estimated (i.e., extended to the end of March 2025) with "future_daz_in_2025". Now we can estimate the price propane on first day of 2025 or last day in the month of March as in:

first_day_of_2025 = 1.7929 + 932(0.0017) = $3.38

last_day_of_March_2025 = 1.7929 + 1021(0.0017) = $3.52

## The Uncertain Future; Predicting with Training and Test Set
It's easier to extend the rregression line and extrapolate for the future; but is the future really this predictable? Can we ascertain that the future would follow the extrapolated data? The answer is obvious - no. One way to go about this is to split the historical data into training(usually 70%-80% of data) and test sets (30%-20%). The training set would be used in the model while the test set would be used to varify how the data would perform on an unseen future data.
```Python
x_train = from_2000.loc[0:740, ["day"]].values.reshape(-1,1)
y_train = from_2000.loc[0:740, ["ny_state_avg_price"]].values.reshape(-1,1)
x_test = from_2000.loc[741:932, ["day"]]
regression.fit(x_train, y_train)
print("coefficient = ", reg.coef_, "intercept =", reg.intercept_)
```
![traini_coef_inter](https://github.com/dataglyder/Propane-Prices.io/blob/main/traini_coef_interc.png)

Now that we have the coefficient and intercept from the training set, we can generate a training model and test our test set (assumed future data) on it to guess and prepare our model for the uncertain future.

price = 1.711 + 0.0021(any day in the test set)    # let's try day 783
price_target_future = 1.711 + 0.0021(783) = $3.36


## Conclusion 
Yes! Consumers have enjoyed some reduction in price after increment according to this analyssis and maybe possibly in the futrue. Price is expected to flunctuate as days pass by; the estimates into the future are just guide into what is expected, there is no guanratee that price will increase linearly but training our model can help it prepare for any future data. 


 



