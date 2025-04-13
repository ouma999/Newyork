The data set represent the statistics of arrests made in the state of New York. It breaks the arrest further in demographics and geographical location the crimes comitted.
From the dataset we can get useful insights such as prevalance rate of crime based on crimes as well as mitigation ways that can be best implemented to tackle the various crime.
# -*- coding: utf-8 -*-
"""
Created on Wed Nov 27 16:37:11 2024

@author: polex
"""

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Import the dataset for analysis 
data = pd.read_csv('C:/Users/polex/Downloads/NYPD_Arrest_Data__Year_to_Date_ (1).csv')
data.head()

#displays column names, count of data items in each column, and the type of data. 
data.info()

# Check for missings values in the dataset and sums it
data.isna().sum()

# Drop missing values empty
data_cleaned = data.dropna()
data_cleaned



#
data_cleaned.describe()

data_cleaned['OFNS_DESC'].value_counts()

data_cleaned['PD_DESC'].value_counts()

# Here we can do a histogram representing crime per borough

data_cleaned['ARREST_BORO'].value_counts()
plt.figure(figsize=(15, 6))
sns.countplot(x='ARREST_BORO', data=data_cleaned)
plt.title('Arrests by Borough')
plt.show()


# Plot the distribution Histogram of gender

sns.countplot(data=data_cleaned, x='PERP_SEX')
plt.title('Distribution of Gender')
plt.xlabel('Gender')
plt.ylabel('Individuals arrested')
plt.show()


#Display the various age croup in a histogram visualization.

data_cleaned['AGE_GROUP'].value_counts().plot(kind='bar')

#Display the various racial agegroup in a histogram 

data_cleaned['PERP_RACE'].value_counts().plot(kind='bar')




# Distribution of LAW_CAT_CD
law_cat_distribution = data['LAW_CAT_CD'].value_counts()



# Top 10 Law Codes visualized
top_law_codes = data['LAW_CODE'].value_counts().head(10)

plt.figure(figsize=(12, 6))
sns.barplot(x=top_law_codes.index, y=top_law_codes.values)
plt.title('Top 10 Most Common Law Codes')
plt.xlabel('Law Code')
plt.ylabel('Count')
plt.xticks(rotation=90)
plt.show()


# display the most type of offence commited
top_offence = data['OFNS_DESC'].value_counts().head()

plt.figure(figsize=(8, 6))
sns.barplot(x=top_offence.index, y=top_offence.values)
plt.title('Top  Most offence')
plt.xlabel('offence discription')
plt.ylabel('Count')
plt.xticks(rotation=45)
plt.show()

# Group by the arrest date and count the number of arrests
arrests_per_day = data_cleaned.groupby('ARREST_DATE').size()
# Plot the time series to visualize number of arrest as days go by 
arrests_per_day.plot(kind='line', figsize=(15, 6))
plt.xlabel('Month')
plt.ylabel('Number of Arrests')
plt.title('Number of Arrests Over Time')
plt.show()

##We can calculate the 10-measure moving average 
#The 10-point moving average is the average of the last 10 days  of arrest
q=arrests_per_day.rolling(10).mean()
q.plot(kind='line',figsize=(15, 6))
## we can plot the 30-day rolling standard deviation
#we can see a marked increase in volatility during mid mar, may followed by
#a steady decrease july then a return to the earlier levels of volatility during october.
p=arrests_per_day.rolling(30).std().plot()

#histogram 
#here we see that most arrest count btw (550 - 900)

arrests_per_day.plot(kind='hist',bins = 15)

# Smoothing the histogram
arrests_per_day.plot(kind='kde')

# from the boxplot we can see the median is a value slightly above 700
arrests_per_day.plot(kind='box')

# Apivot table for the categorical variables.
pivot_table = pd.pivot_table(data_cleaned, values='ARREST_KEY', index='ARREST_BORO', columns='OFNS_DESC', aggfunc='count', fill_value=0)
# Display the pivot table
pivot_table.head()
max(pivot_table)
min(pivot_table)

t=pivot_table

t.plot(kind='line',figsize=(18,6))


#descriprive stats 
data_cleaned['AGE_GROUP'].mode()


data_cleaned['PERP_SEX'].mode()

#convert strings to the pandas Timestamp type
data_cleaned['ARREST_DATE'] = pd.to_datetime(data_cleaned['ARREST_DATE'], errors='coerce')



p=data_cleaned.PERP_SEX.map({'M':1,'F':0})
#
