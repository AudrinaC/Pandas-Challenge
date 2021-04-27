# Pandas-Challenge
Week 4 Assignment 

GitHub is having issues converting Jupyter Notebook's ipynb when there is a .map("${:,.2f}".format used within the data table. However, the data is listed below: 

# Pandas Homework - Pandas, Pandas, Pandas

## Background

The data dive continues!

Now, it's time to take what you've learned about Python Pandas and apply it to new situations. For this assignment, you'll need to complete **one of two** (not both)  Data Challenges. Once again, which challenge you take on is your choice. Just be sure to give it your all -- as the skills you hone will become powerful tools in your data analytics tool belt.

### Before You Begin

1. Create a new repository for this project called `pandas-challenge`. **Do not add this homework to an existing repository**.

2. Clone the new repository to your computer.

3. Inside your local git repository, create a directory for the Pandas Challenge you choose. Use folder names corresponding to the challenges: **HeroesOfPymoli** or  **PyCitySchools**.

4. Add your Jupyter notebook to this folder. This will be the main script to run for analysis.

5. Push the above changes to GitHub or GitLab.

## Option 1: Heroes of Pymoli

Congratulations! After a lot of hard work in the data munging mines, you've landed a job as Lead Analyst for an independent gaming company. You've been assigned the task of analyzing the data for their most recent fantasy game Heroes of Pymoli.

Like many others in its genre, the game is free-to-play, but players are encouraged to purchase optional items that enhance their playing experience. As a first task, the company would like you to generate a report that breaks down the game's purchasing data into meaningful insights.

Your final report should include each of the following:

### Player Count

* Total Number of Players

### Purchasing Analysis (Total)

* Number of Unique Items
* Average Purchase Price
* Total Number of Purchases
* Total Revenue

### Gender Demographics

* Percentage and Count of Male Players
* Percentage and Count of Female Players
* Percentage and Count of Other / Non-Disclosed

### Purchasing Analysis (Gender)

* The below each broken by gender
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Gender

### Age Demographics

* The below each broken into bins of 4 years (i.e. &lt;10, 10-14, 15-19, etc.)
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value
  * Average Purchase Total per Person by Age Group

### Top Spenders

* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
  * SN
  * Purchase Count
  * Average Purchase Price
  * Total Purchase Value

### Most Popular Items

* Identify the 5 most popular items by purchase count, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value

### Most Profitable Items

* Identify the 5 most profitable items by total purchase value, then list (in a table):
  * Item ID
  * Item Name
  * Purchase Count
  * Item Price
  * Total Purchase Value

As final considerations:

* You must use the Pandas Library and the Jupyter Notebook.
* You must submit a link to your Github/Git Lab repo that contains your Jupyter Notebook.
* You must include a written description of three observable trends based on the data.
* See [Example Solution](HeroesOfPymoli/HeroesOfPymoli_starter.ipynb) for a reference on expected format.

### Copyright

© 2021 Trilogy Education Services, LLC, a 2U, Inc. brand. Confidential and Proprietary. All Rights Reserved.


# Dependencies and Setup
import pandas as pd
import numpy as np
​
# File to Load (Remember to Change These)
file_to_load = "../PandaStuff/purchase_data.csv"
​
# Read Purchasing File and store into Pandas data frame
purchase_data = pd.read_csv(file_to_load)
Player Count
Display the total number of players
In [5]:

total_players = len(purchase_data["SN"].value_counts())
player_count = pd.DataFrame({"Total Players":[total_players]})
player_count
Out[5]:
Total Players
0	576
Purchasing Analysis (Total)
Run basic calculations to obtain number of unique items, average price, etc.
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
In [10]:

# unique items, average price, purchase count, and revenue variables 
number_of_unique_items = len((purchase_data["Item ID"]).unique())
average_price = (purchase_data["Price"]).mean()
number_of_purchases = (purchase_data["Purchase ID"]).count()
total_revenue = (purchase_data["Price"]).sum()
 
summary_df = pd.DataFrame({"Number of Unique Items":[number_of_unique_items],
                           "Average Price":[average_price], 
                           "Number of Purchases": [number_of_purchases], 
                           "Total Revenue": [total_revenue]})
​
# Data Formatting 
summary_df.style.format({'Average Price':"${:,.2f}",
                         'Total Revenue': '${:,.2f}'}) 
Out[10]:
Number of Unique Items	Average Price	Number of Purchases	Total Revenue
0	179	$3.05	780	$2,379.77
Gender Demographics
Percentage and Count of Male Players
Percentage and Count of Female Players
Percentage and Count of Other / Non-Disclosed
In [11]:

# Group purchase_data by Gender
gender_stats = purchase_data.groupby("Gender")
​
total_count_gender = gender_stats.nunique()["SN"]
​
percentage_of_players = total_count_gender / total_players * 100
​
# Data Frame Creation 
gender_demographics = pd.DataFrame({"Percentage of Players": percentage_of_players, "Total Count": total_count_gender})
​
gender_demographics.index.name = None
​
# Formatting data
gender_demographics.sort_values(["Total Count"], ascending = False).style.format({"Percentage of Players":"{:.2f}"})
Out[11]:
Percentage of Players	Total Count
Male	84.03	484
Female	14.06	81
Other / Non-Disclosed	1.91	11
Purchasing Analysis (Gender)
Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. by gender
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
In [8]:

# Breakdown of Gender purchases 
purchase_count = gender_stats["Purchase ID"].count()
​
avg_purchase_price = gender_stats["Price"].mean()
​
avg_purchase_total = gender_stats["Price"].sum()
​
avg_purchase_per_person = avg_purchase_total/total_count_gender
​
# Data Frame Creation
gender_demographics = pd.DataFrame({"Purchase Count": purchase_count, 
                                    "Average Purchase Price": avg_purchase_price,
                                    "Average Purchase Value":avg_purchase_total,
                                    "Avg Purchase Total per Person": avg_purchase_per_person})
​
gender_demographics.index.name = "Gender"
​
# Formatting data 
gender_demographics.style.format({"Average Purchase Value":"${:,.2f}",
                                  "Average Purchase Price":"${:,.2f}",
                                  "Avg Purchase Total per Person":"${:,.2f}"})
Out[8]:
Purchase Count	Average Purchase Price	Average Purchase Value	Avg Purchase Total per Person
Gender				
Female	113	$3.20	$361.94	$4.47
Male	652	$3.02	$1,967.64	$4.07
Other / Non-Disclosed	15	$3.35	$50.19	$4.56
Age Demographics
Establish bins for ages
Categorize the existing players using the age bins. Hint: use pd.cut()
Calculate the numbers and percentages by age group
Create a summary data frame to hold the results
Optional: round the percentage column to two decimal points
Display Age Demographics Table
In [12]:

# Age Bins
age_bins = [0, 9.90, 14.90, 19.90, 24.90, 29.90, 34.90, 39.90, 99999]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]
​
purchase_data["Age Group"] = pd.cut(purchase_data["Age"],age_bins, labels=group_names)
purchase_data
​
age_grouped = purchase_data.groupby("Age Group")
​
total_count_age = age_grouped["SN"].nunique()
​
percentage_by_age = (total_count_age/total_players) * 100
​
# Data Frame Creation
age_demographics = pd.DataFrame({"Percentage of Players": percentage_by_age, "Total Count": total_count_age})
​
# Formatting Data
age_demographics.index.name = None
 
age_demographics.style.format({"Percentage of Players":"{:,.2f}"})
Out[12]:
Percentage of Players	Total Count
<10	2.95	17
10-14	3.82	22
15-19	18.58	107
20-24	44.79	258
25-29	13.37	77
30-34	9.03	52
35-39	5.38	31
40+	2.08	12
Purchasing Analysis (Age)
Bin the purchase_data data frame by age
Run basic calculations to obtain purchase count, avg. purchase price, avg. purchase total per person etc. in the table below
Create a summary data frame to hold the results
Optional: give the displayed data cleaner formatting
Display the summary data frame
In [14]:

# Count purchases by age group
purchase_count_age = age_grouped["Purchase ID"].count()
​
avg_purchase_price_age = age_grouped["Price"].mean()
​
total_purchase_value = age_grouped["Price"].sum()
​
avg_purchase_per_person_age = total_purchase_value/total_count_age
​
# Data Frame Creation
age_demographics = pd.DataFrame({"Purchase Count": purchase_count_age,
                                 "Average Purchase Price": avg_purchase_price_age,
                                 "Total Purchase Value":total_purchase_value,
                                 "Average Purchase Total per Person": avg_purchase_per_person_age})
​
# Formatting Data
age_demographics.index.name = None
​
age_demographics.style.format({"Average Purchase Price":"${:,.2f}",
                               "Total Purchase Value":"${:,.2f}",
                               "Average Purchase Total per Person":"${:,.2f}"})
Out[14]:
Purchase Count	Average Purchase Price	Total Purchase Value	Average Purchase Total per Person
<10	23	$3.35	$77.13	$4.54
10-14	28	$2.96	$82.78	$3.76
15-19	136	$3.04	$412.89	$3.86
20-24	365	$3.05	$1,114.06	$4.32
25-29	101	$2.90	$293.00	$3.81
30-34	73	$2.93	$214.00	$4.12
35-39	41	$3.60	$147.67	$4.76
40+	13	$2.94	$38.24	$3.19
Top Spenders
Run basic calculations to obtain the results in the table below
Create a summary data frame to hold the results
Sort the total purchase value column in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the summary data frame
In [16]:

# Group purchase data
spender_stats = purchase_data.groupby("SN")
​
purchase_count_spender = spender_stats["Purchase ID"].count()
​
avg_purchase_price_spender = spender_stats["Price"].mean()
​
purchase_total_spender = spender_stats["Price"].sum()
​
# DF Creation
top_spenders = pd.DataFrame({"Purchase Count": purchase_count_spender,
                             "Average Purchase Price": avg_purchase_price_spender,
                             "Total Purchase Value":purchase_total_spender})
​
# Data Formatting
formatted_spenders = top_spenders.sort_values(["Total Purchase Value"], ascending=False).head()
​
formatted_spenders.style.format({"Average Purchase Total":"${:,.2f}",
                                 "Average Purchase Price":"${:,.2f}", 
                                 "Total Purchase Value":"${:,.2f}"})
Out[16]:
Purchase Count	Average Purchase Price	Total Purchase Value
SN			
Lisosia93	5	$3.79	$18.96
Idastidru52	4	$3.86	$15.45
Chamjask73	3	$4.61	$13.83
Iral74	4	$3.40	$13.62
Iskadarya95	3	$4.37	$13.10
Most Popular Items
Retrieve the Item ID, Item Name, and Item Price columns
Group by Item ID and Item Name. Perform calculations to obtain purchase count, average item price, and total purchase value
Create a summary data frame to hold the results
Sort the purchase count column in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the summary data frame
In [17]:

items = purchase_data[["Item ID", "Item Name", "Price"]]
​
item_stats = items.groupby(["Item ID","Item Name"])
​
purchase_count_item = item_stats["Price"].count()
​
purchase_value = (item_stats["Price"].sum()) 
​
item_price = purchase_value/purchase_count_item
​
# DF Creation
most_popular_items = pd.DataFrame({"Purchase Count": purchase_count_item, 
                                   "Item Price": item_price,
                                   "Total Purchase Value":purchase_value})
​
# Formatting
popular_formatted = most_popular_items.sort_values(["Purchase Count"], ascending=False).head()
​
popular_formatted.style.format({"Item Price":"${:,.2f}",
                                "Total Purchase Value":"${:,.2f}"})
Out[17]:
Purchase Count	Item Price	Total Purchase Value
Item ID	Item Name			
92	Final Critic	13	$4.61	$59.99
178	Oathbreaker, Last Hope of the Breaking Storm	12	$4.23	$50.76
145	Fiery Glass Crusader	9	$4.58	$41.22
132	Persuasion	9	$3.22	$28.99
108	Extraction, Quickblade Of Trembling Hands	9	$3.53	$31.77
Most Profitable Items
Sort the above table by total purchase value in descending order
Optional: give the displayed data cleaner formatting
Display a preview of the data frame
In [18]:

popular_formatted = most_popular_items.sort_values(["Total Purchase Value"],
                                                   ascending=False).head()
# Data Formatting
popular_formatted.style.format({"Item Price":"${:,.2f}",
                                "Total Purchase Value":"${:,.2f}"})
Out[18]:
Purchase Count	Item Price	Total Purchase Value
Item ID	Item Name			
92	Final Critic	13	$4.61	$59.99
178	Oathbreaker, Last Hope of the Breaking Storm	12	$4.23	$50.76
82	Nirvana	9	$4.90	$44.10
145	Fiery Glass Crusader	9	$4.58	$41.22
103	Singed Scalpel	8	$4.35	$34.80



