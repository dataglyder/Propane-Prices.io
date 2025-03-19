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
needed_cols = data.rename(column = {"Data":"data", "New York Statewide Average ($/gal)":"ny_st_avg ($/gal)"}]
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
import matplotlib.pyplot as p
import seaborn as s
hist_chart=p.figure(figsize=(6,5), layout="constrained")
s.histplot(needed_cols, x="ny_st_avg ($/gal)", bins=20, element="step")
p.show()
```
![Histogram of Average Price Between 1997 to 2024](https://github.com/dataglyder/Propane-Prices.io/blob/main/Hist_of_data.png)

The chart shows the price on the x-axis and its frequencies on the y-axis. It is right  skewed i.e. price mostly move towards right side of the chart and it looks like it's bi-modal i.e., has two peak periods. I would lay a density curve on the chart to ascertain the bi-modal shape.
## Density Curve
``` Python
s.histplot(needed_cols, x="ny_st_avg ($/gal)", bins=20, element="step", kde=True)
p.show()
```
![Bi_modal Density Chart of Price](Path to image)
## Density Curve without bars or Elements
For better clarity, I will view the modal density shape without the bars or elements.
``` Python
s.displot(needed_cols, x="ny_st_avg ($/gal)", kind = "kde"
p.show()
```
![Bi_modal Curve](Path to image)

## Box Plot
I will examine the average price within each year with box plot.



To updated and cotinued
 



