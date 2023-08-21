# Product Sales Exploratory Data Analysis in Python

![Wholesale-Stationery-Products-Online](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/f6e97bf6-171d-4642-8a25-42373332e2ca)


# Background
About Pens and Printers
Pens and Printers was founded in 1984 and provides high-quality office products to large
organizations. We are a trusted provider of everything from pens and notebooks to desk
chairs and monitors. We don’t produce our own products but sell those made by other
companies.
We have built long-lasting relationships with our customers and they trust us to provide them
with the best products for them. As the way in which consumers buy products is changing,
our sales tactics have to change too. Launching a new product line is expensive and we need
to make sure we are using the best techniques to sell the new product effectively. The best
approach may

# Business questions:
We need to know:
- How many customers were there for each approach?
- What does the spread of the revenue look like overall? And for each method?
- Was there any difference in revenue over time for each of the methods?
- Based on the data, which method would you recommend we continue to use? Some
of these methods take more time from the team so they may not be the best for us
to use if the results are similar.


# The folowing are table of contents:
 - Data validation and cleaning:
 - Validating data type, field content,etc.
 - Cleaning errors
 - Exploratory Analysis:
 - Include two different visualizations showing single variables only to demonstrate the characteristics of data
 - Include visualizations showing two or more variables to represent the relationship between features
 - Describing findings
 - Making recommendations
-----------------------------------------------------------------------------------------------------------------------------



# Data Validation and Cleaning

- To begin we have to import all the python library that will be usefull, at least the onse we need to begin with.
- Then import our dataset
- Then take a view of our fields
- If there's any action needed for cleaning then we proceed, if not we leave them.

```python


# Import all the useful library
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.style as style
style.use('ggplot')
plt.figure(figsize=(10,6))
sns.set_palette(['gray'])

# Load the dataset
df = pd.read_csv('product_sales_data.csv')

df.info()
df.head()
df.columns

# I want to know how many na or null values are there in the dataset
df_null_count = df.isna().sum().sum()
print("dataset null:",df_null_count)

# I want to know how many na or null values are there in revenue column
revenue_nll_count = df["revenue"].isna().sum()
print("revenue null:",revenue_nll_count)
```


From the above outputs:
We could see that our data contains 15000 rows and 8 columns.
That the dataset has 1074 null and the null values are in the revenue column.
We could see that the revenue column only have 13926 numeric values and 1074 null
My quick jugement of this:
For me, becuse the revenue column values is one of the most important columns here, we have a few options here, 
1. We could either replace the null values with the revenue mean, median or mode values, 
2. We could delete all the null rows.

I would prefare to work with a genuine data rather than fabricating figures to fill in the null. I have decided to drop the null revenue rows.


```python


# Delete rows where 'revenue' column is null
df.dropna(subset=['revenue'], inplace=True)

# I want to check again if the null rows has been dropped
revenue_nll_count = df["revenue"].isna().sum()
print("revenue null:",revenue_nll_count)

# Print the updated DataFrame info
df.info()

Print the datafame discribtion
df.describe()

df.head()

# Filter the DataFrame based on the condition 'years_as_customer' > 39
filtered_customers = df[df['years_as_customer'] > 39]

print(filtered_customers)

# View the list of customers with 'years_as_customer' > 39
years_of_customers = filtered_customers['years_as_customer'].tolist()
print(years_of_customers)

# Remove rows where 'years_as_customer' is greater than 39
df = df[df['years_as_customer'] <= 39]

# Reset the index of the DataFrame
df.reset_index(drop=True, inplace=True)

# Display the modified DataFrame
print(df)


# Delete rows where 'revenue' column is null
df.dropna(subset=['revenue'], inplace=True)
```


# Validation of all fields one after the other After removing the null rows, our dataset now remains 13926 rows and 8 columns. Next steps, we are validating all columns by going through them to observe all the contents correct any error necessary.


# Column 1 week

```python
# Let's check for unique values
unique_count = df['week'].nunique()
print(unique_count)

unique_strings = df['week'].unique()
print(unique_strings)
```

**Week column validation: Week column is numeric, has 6 unique weeks with 13926 values, no cleaning needed.**


# column 2 sales_method 

```python
# let's check the unique categories of sales_method
sales_method_type = df['sales_method'].unique()
print(sales_method_type)

#Let's count number of values on each category
sales_method = df.groupby('sales_method')['sales_method'].count()
print(sales_method)

#Let's clean the sales_method categories, the categories are supposed to be 3: Email + Call, Email, then Call, instead of 5, let's correct the em + call spelling to match Email + Call, and  email to match Email. 

df['sales_method'] = df['sales_method'].replace({'em + call': 'Email + Call', 'email': 'Email'})

#Let's count number of values on each category to see if the errors are corrected.
sales_method = df.groupby('sales_method')['sales_method'].count()
print(sales_method)
```

**Sales_method column validation: Sales_method is a category values with Call, Email + Call and Email, there were errors in the categories, but it's corrected and cleaned up.**


# column 3 n_id:

```python

# let's check the data type of customer_id
customer_id = df['customer_id'].dtype
print(customer_id)

# let's check the count of customer_id
customer_id = df['customer_id'].nunique()
print(customer_id)

duplicates = df['customer_id'].duplicated().sum()
print(duplicates)
```

**Customer_id column validation: customer_id is a unique identifier with no duplicates, so we have 13926 unique customers.**


# column 4 nb_sold

```python
# let's check the data type of nb_sold
nb_sold_type = df['nb_sold'].dtype
print(nb_sold_type)

# let's check the count of nb_sold
unique_count = df['nb_sold'].nunique()
print(unique_count)

# let's check the values in nb_sold column
unique_values = df['nb_sold'].unique()
print(unique_values)

#Let's count number of values on each nb_sold
nb_sold = df.groupby('nb_sold')['nb_sold'].count()
print(nb_sold)
```

**nb_sold column validation: Number sole column is numeric, it has 10 unique values of 7 to 16, no cleaning needed.**


# column 5 revenue

```python
# let's check the data type of revenue
revenue_type = df['revenue'].dtype
print(revenue_type)

# let's check the sum of revenue
unique_sum = df['revenue'].sum()
print(unique_sum)

# Let's check again if there's null values in revenue column
revenue_null_count = df["revenue"].isna().sum()
print("revenue null:",revenue_null_count)
```

**Revenue column validation: Revenue is float64 numeric with two decimal place, I initially remove all rows with revenue null values, after cleaning we have 13926 values.**


# column 6 years_as_customer

```python
# let's check the data type of years_as_customer
years_as_customer_type = df['years_as_customer'].dtype
print(years_as_customer_type)

# let's check the count of years_as_customer
unique_count = df['years_as_customer'].nunique()
print(unique_count)

# let's check the values in years_as_customer column
unique_values = df['years_as_customer'].unique()
print(unique_values)

#Let's check the oldest customer
years_as_customer_max = df['years_as_customer'].max()
print(years_as_customer_max)

#Let's count number of values on each years_as_customer
years_as_customer = df.groupby('years_as_customer')['years_as_customer'].count()
print(years_as_customer)

# Filter the DataFrame based on the condition 'years_as_customer' > 39
filtered_customers = df[df['years_as_customer'] > 39]
print(filtered_customers)

# View the list of customers with 'years_as_customer' > 39
years_of_customers = filtered_customers['years_as_customer'].tolist()
print(years_of_customers)

# Remove rows where 'years_as_customer' is greater than 39
df = df[df['years_as_customer'] <= 39]

# Reset the index of the DataFrame
df.reset_index(drop=True, inplace=True)
```

**years_as_customer column validation: Years as cutommer is numeric with number of years of cutomers patronage, there are 39 unique years with 39 years as the oldest customer and 0 year as the new customers. 2 outliers reomoved 42 and 63. column cleaned.**


# column 7 nb_site_visits

```python
# let's check the data type of nb_site_visits
nb_site_visits_type = df['nb_site_visits'].dtype
print(nb_site_visits_type)

# let's check the count of nb_site_visits
unique_count = df['nb_site_visits'].nunique()
print(unique_count)

# let's check the values in nb_site_visits column
unique_values = df['nb_site_visits'].unique()
print(unique_values)

# Display the modified DataFrame
print(df.head())
```

**nb_site_visits column validation: nb_site_visits is numeric, the same as descrbtion. No cleaning needed.**


# column 8 state

```python
# let's check the data type of state
state_type = df['state'].dtype
print(nb_site_visits_type)

# let's check the count of state
unique_count = df['state'].nunique()
print(unique_count)

# let's check the values in state column
unique_values = df['state'].unique()
print(unique_values)

states = df.groupby('state')['state'].count()
print(states)
```

Cleaned data head:
![Product_sales_data_head](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/f3f08fc0-965a-462d-8cd7-6bb3b9712245)



**state column validation: state is a category values with 50 unique state values, the same as descrbtion. No cleaning needed.**


# Data Validation Conclusion: The original dataset is 15000 rows and 8 columns. I have validated all the columns against the criteria in the dataset table:

`week`: Numeric, 6 unique weeks without missing values, same as the description. No cleaning was needed.

`sales_method`: 3 categories Email, Call, Email + Call, same as the description and 2 typo error categories not the same as the description. The 2 typo categories was cleaned to match the 3 correct categories.

`customer_id`: 15000 Character, unique identifier for the customer, same as the description, but 13926 remaining after cleaning revenue null rows.

`nb_sold`: Numeric,10 unique values, number of new products sold, same as the description. No cleaning was needed.

`revenue`: Numeric, revenue from the sales, rounded to 2 decimal places, with 13926 values, and 1074 null values, i dropped all rows with null revenue values. revenue was cleaned.

`years_as_customer`: Numeric, 42 unique values as number of years customer has been buying products, same as the description. But since the business started in 1984 which is 39 years currently, there were customers with 47 and 63 years. Years as Customers above 39 were removed, and now have 0 to 39 unique customers. Years as customer column was cleaned.

`nb_site_visits`: Numeric, 27 unique values without missing values, same as the description. No cleaning was needed.

`state`: 50 unique Chategories, States location of the customers, same as the description. No cleaning was needed.

**After carrying out the data validation and cleaning, the dataset were 13926 rows and 8 columns.**



# Exploratory Analysis & Visualization


# Question 1: How many customers were there for each approach?

**The below single variable graph indicates that each strategic approach provides insights into the distribution of each sales method, and the result of each methods over the period of time as included in the dataset.**

```python

# Group the data by 'sales_method' and count the unique 'customer_id' values
grouped = df.groupby('sales_method')['customer_id'].nunique().sort_values(ascending=False)

import matplotlib.pyplot as plt
import seaborn as sns
import matplotlib.style as style
style.use('ggplot')
plt.figure(figsize=(10,6));
sns.set_palette(['gray']);

# Create the figure and axes with specified figsize
fig, ax = plt.subplots(figsize=(10, 6));

# Label each bar with their respective numbers
for i, value in enumerate(grouped.values):
    plt.text(i, value, str(value), ha='center', va='bottom')

# Plot the bar chart with gray bars
ax.bar(grouped.index, grouped.values, color='gray')  # Fixed the error by providing the color directly

# Set labels and title
ax.set_xlabel('Sales Method')
ax.set_ylabel('Count of Unique Customers')
ax.set_title('Distribution of Customers by Sales Method')

# Display the chart
plt.show();
```
![1  Distribution of Sales Method](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/94bcfcf8-57fb-4d9a-a509-e6ac01855136)



**Distribution of Customers by Sales Method: We could see that The Email approach reached the majority of 45.17% with 6922 customers, followed by Call aproach 31.14% with 4781, and the Email + Call approaches 23.69% with 2223, these makes the total number of 13926 customers.**



# Question 2: What does the spread of the revenue look like overall? And for each method?

```python

# Distribution of Revenue
plt.figure(figsize=(10,6))
sns.histplot(data=df, x='revenue')
plt.ylabel('Count of Revenue')
plt.xlabel('Revenue')
plt.title('Distribution of Revenue')
```
![2  Distribution of Revenue](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/5dcb6043-2670-4d43-8946-fb4f094588f0)



**The Spread of the Revenue: Looking at the distribution of the revenue we can see from the graph that most single products sold had amount less than 200 USD, and the largest number of products sold are within the amount of 45 and 100 USD. The distribution of the revenue is right skewed. There are some sold more than 220 USD and above could be considered outliers.**



# Distribution of Revenue by Sales Method

```python
# import matplotlib
import matplotlib.pyplot as plt

# Group the data by 'sales_method' and sum the 'revenue' values
grouped = df.groupby('sales_method')['revenue'].sum().sort_values(ascending=False)

# Set the gray color for bars and grid lines
bar_color = 'gray'
grid_color = 'lightgray'

# Create the figure and axes with specified figsize
fig, ax = plt.subplots(figsize=(10, 6))

# Label each bar with their respective numbers
for i, value in enumerate(grouped.values):
    plt.text(i, value, str(value), ha='center', va='bottom')

# Plot the bar chart with gray bars
ax.bar(grouped.index, grouped.values, color=bar_color)

# Set labels and title
ax.set_xlabel('Sales Method')
ax.set_ylabel('Revenue')
ax.set_title('Distribution of Revenue by Sales Method')

# Display the chart
plt.show()

```

![2  1 Distribution of Revenue by Sales Method](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/bf3481b0-b366-4560-bfdf-3fa192b49e5b)


**Distribution of Revenue by Sales Method:The chart above shows the overal spread of the revenue for each method. The Email method had the highest revenue with 50.53%, followed by Email + Call with 30.71%, and Call with 18.76%.**


# Corrolation of Revenue and Product sold
```python

# The heatmap relationship between the values

# Create the figure and axes with specified figsize
plt.figure(figsize=(10, 6));

# Heatmap plot to showcase the overall relationship
sns.heatmap(df.corr(),annot=True);
plt.title('The relationship between the values');

```

![Heatmap -The relationship between the values](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/0996c644-9c3b-476f-9161-02dc3f7a909c)


**Revenue and product sold are positively correlated**



# Revenue Differences over Time for each Sales Method
```python

# Create the figure and axes with specified figsize
fig, ax = plt.subplots(figsize=(10, 6))

# Create a box plot to visualize the difference in 'revenue' over 'years_as_customer' for each 'sales_method'
sns.boxplot(x='sales_method', y='revenue', hue='sales_method', data=df, palette='Set2');
plt.xlabel('Sales Method')
plt.ylabel('Revenue')
plt.title('Difference in Revenue over Years as Customer for each Sales Method')

# Display the plot
plt.legend(title='Sales Method', facecolor='lightgray')
plt.show()
```
![3 1 Difference in Revenue over Years as Customer for each Sales Method](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/34d24b3a-d8dc-4d4f-abba-ff3e3784e540)



**Revenue Differences over Time for each Sales Method: With just 2223 customers Email + Call generated 30.71% of the overal revenue comapared to other method with higher numbers of customers. Also, combining the use of both Email and Call as sales methods tends to produce a quicker and effective result compared to Email or Call.**




# Question 3: Was there any difference in revenue over time for each of the methods?

```python
# Create the figure and axes with specified figsize
fig, ax = plt.subplots(figsize=(10, 6))

# Create the line plot with markers
sns.lineplot(x='years_as_customer', y='revenue', hue='sales_method', data=df, marker='o', palette='Set2')

# Set the labels and title of the plot
plt.xlabel('Years as Customer')
plt.ylabel('Revenue')
plt.title('Revenue Differences over Time for each Sales Method')

# Adjust the legend position
plt.legend(title='Sales Method', loc='upper right')

# Display the plot
plt.show()
```

![3  0 Revenue Differences over Time for each Sales Method](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/12aef444-97b5-45bc-8a98-4777f1df72c1)


**Difference in Revenue over Years as Customer for each Sales Method: The difference indicates the approach of Email + Call method generated revenue within the range of 140 to 205 per sale on some of the top products and the Years of customers patronage is between 0 and 39 years. Also, combining the use of both Email and Call as sales methods tends to produce a quicker and effective result compared to Email or Call.**


# Question 4:Based on the data, which method would you recommend we continue to use?

```python

# Pivot the data to create a DataFrame with 'years_as_customer' as index, 'sales_method' as columns, and 'revenue' as values
pivot_df = df.pivot_table(index='week', columns='sales_method', values='revenue')

# Create the figure and axes with specified figsize
fig, ax = plt.subplots(figsize=(10, 6))

# Create a grouped bar chart to visualize the revenue difference between sales methods over time
pivot_df.plot(kind='bar', color=['salmon', 'brown', 'olive'], ax=ax)
plt.xlabel('Week sales was made')
plt.ylabel('Revenue')
plt.title('Sales Method and Week sale was made since product launch')
# Rotate the x-axis labels horizontally
plt.xticks(rotation='horizontal')

# Display the plot
plt.show()
```
![4  Sales Method and Week sale was made since product launch](https://github.com/mikeolaniyi/Product_Sales_Analysis/assets/120651356/725f8257-2a03-4cbb-aa32-387e9055c0bd)


**Sales Method and Week sale was made since product launch: The chart above depicts the revenue spread across the sales methods over a 6-week period post-product launch. Email + Call consistently shows productive and consistent performance throughout the weeks.**



# The Business Metrics
**Since every business goal is selling products that helps customers and in returns increace the business revenue.**
**I would recommend the the percentage of Email and Call Approach on total number of customers of the last 6 weeks sales to implement Email + Call as our metric.**
**Based on our last 6 weeks product launched data, 45% sales was from Email approach and 31% from Call approch.**
**Therefore, if Email + Call method is implemented as our metric, and this number is increasing next 6 weeks, it indicates a very good sign to achieve our goal.**


# Recommendation
**For the following 6 weeks, I would recommend the company can focus on the following steps:
- Using key metrics to monitor can provide insights into the likelihood of sales growth.
- To implement an Email marketing campaign using customer segmentation, automation, and analytics to deliver targeted emails, track engagement metrics, and optimize Email performance.
- To effectively implement the Call approach within a call center setting for customer follow-ups regarding product inquiries and interests.
- To implement a CRM system if it's not already inplace, to enable us build and maintain strong customer relationships and improve customer experience.

- Data Collection for in-depth analysis:
	- Improve data quality - how much time was spent on each customer?
	- New related data - Date product was sold, delivery date, and other time stamp.**

