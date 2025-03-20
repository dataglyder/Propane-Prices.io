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
needed_cols = data.rename(column={"Data":"data", "New York Statewide Average ($/gal)":"ny_state_avg_price ($/gal)"})
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
s.histplot(needed_cols, x="ny_state_avg_price ($/gal)", bins=20, element="step")
m.ylabel("frequncy")
m.show()
```
![Histogram of Average Price Between 1997 to 2024](https://github.com/dataglyder/Propane-Prices.io/blob/main/Hist_of_data.png)

The chart shows the price on the x-axis and its frequencies on the y-axis. It is right  skewed i.e. price mostly move towards right side of the chart and it looks like it's bi-modal i.e., has two peak periods. Let's lay a density curve on the chart to ascertain the bi-modal shape.

## Density Curve
Density curve summarises the approximate shape and the pattern a data distribution.
``` Python
s.histplot(needed_cols, x="ny_state_avg_price ($/gal)", bins=20, element="step", kde=True)
m.show()
```
![Bi_modal Density Chart of Price](Path to image)
## Density Curve without bars or Elements
For better clarity, I will view the modal density shape without the bars or elements.
``` Python
s.displot(needed_cols, x="ny_state_avg_price ($/gal)", kind = "kde"
m.show()
```
![Bi_modal Curve](Path to image)

The distribution appears to be bi-modal, one peak at around price $1.5 and another peak period around price $3.0

## Box Plot
Let's examine the average price within each year with box plot.

```Python
fig, ax=m.subplots(figsize=(10,8))
s.boxplot(x=renamed_df["year"], y=["ny_state_avg_price ($/gal)"]
ax.ticks_params(rotation=90)
m.show()
```
![boxplot](path)
The boxplot summarises the price for each year.The "T" extensions show the minimum (bottom) and maximumm (top) price for the year. The horizontal line that cut through the box indicate the median for the year. The spade shape around the box indicate [outliers](). There seems to have been some relieve in the past: price drop in 2002 after the increase in 2001,it also drop in 2009 after a preceeding consistent hike in price and the most recent in hike was in 2022 before a bit of relief in 2023 and 2024. 

## Statistical Analysis
Let's take some sample and carryout statistical analysis on the sample. Year 2003 and 2004 are very recent; are there any sigificant difference between their price? Let's check with a T- statistics. You can check my write up on [traditional way of conductiong statistical analysis](). Let's quickly run it with Python here.

**Let's state our hypothesis**

**Null hypothsis $`H_{0}`$**: Propane price in 2024 was higher than 2023; $`\mu_{1} > \mu_{2}`$

**Alternamte hypothesis $`H_{1}`$** Propane price in 2024 was not higher than 2023 $`\mu_{1} \leq\mu_{2}`$

Now, let's import the necesarry library before we cotinue.
```Python
import scipy.stats



 


To updated and cotinued
 



