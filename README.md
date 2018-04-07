

```python
import pandas as pd
path = "HeroesOfPymoli/purchase_data.json"
pymoli_df = pd.read_json(path)

pymoli_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
    </tr>
  </tbody>
</table>
</div>




```python
#Total players
player_count = pymoli_df["SN"].nunique()

player_count_df = pd.DataFrame([{"Total Players" : player_count}])
player_count_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#question: how to pass multiple columns on the subset(sequence of labels). Should pass SN, age and gender
player_count = pymoli_df.drop_duplicates(subset='SN', keep='first', inplace=False)
unique_player_count = player_count["SN"].count()

player_count_df = pd.DataFrame([{"Total Players" : unique_player_count}])
player_count_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>573</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Purchasing Analysis (Total)**

#* Number of Unique Items
#* Average Purchase Price
#* Total Number of Purchases
#* Total Revenue

unique_items = pymoli_df["Item ID"].nunique()

average_price = round(pymoli_df["Price"].mean(),2)

total_purchases = pymoli_df["Price"].count()

revenue = pymoli_df["Price"].sum()

purchasing_analysis_df = pd.DataFrame([{"Number of Unique Items": unique_items, "Average Purchase Price": average_price,
                                       "Total Number of Purchases": total_purchases, "Total Revenue": revenue}])

purchasing_analysis_df['Average Purchase Price']=purchasing_analysis_df['Average Purchase Price'].map('${:,.2f}'.format)
purchasing_analysis_df['Total Revenue']=purchasing_analysis_df['Total Revenue'].map('${:,.2f}'.format)

purchasing_analysis_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Number of Unique Items</th>
      <th>Total Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>$2.93</td>
      <td>183</td>
      <td>780</td>
      <td>$2,286.33</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Gender Demographics**

#* Percentage and Count of Male Players
#* Percentage and Count of Female Players
#* Percentage and Count of Other / Non-Disclosed

all_male = pymoli_df['Gender']=='Male'
all_female = pymoli_df['Gender']=='Female'
all_other = (pymoli_df['Gender']!='Male') & (pymoli_df['Gender']!='Female')
gender_index = ['Male', 'Female', 'Other / Non-Disclosed']

male_count =  pymoli_df[all_male]['SN'].nunique()
female_count = pymoli_df[all_female ]['SN'].nunique()
other_count = pymoli_df[all_other]['SN'].nunique()
gender_count = pd.Series([male_count, female_count, other_count], index=gender_index)

male_perc = round(male_count/player_count*100,2)
female_perc = round(female_count/player_count*100,2)
other_perc = round(other_count/player_count*100,2)
gender_perc = pd.Series([male_perc, female_perc, other_perc], index=gender_index)

gender_df = pd.DataFrame({'Total Count':gender_count, 'Percentage of Players':gender_perc})

gender_df['Percentage of Players']=gender_df['Percentage of Players'].map('{:,.2f}%'.format)
gender_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.15%</td>
      <td>465</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.45%</td>
      <td>100</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>1.40%</td>
      <td>8</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Purchasing Analysis (Gender)** 

#* The below each broken by gender
#  * Purchase Count
#  * Average Purchase Price
#  * Total Purchase Value
#  * Normalized Totals

#take the *Total Purchase Value* for each age group and divide it by the *Total Count* of the respective age group. 
#Essentially the average total per play purchased by group.

purchase_count_male = pymoli_df[all_male]['SN'].nunique()
purchase_count_female = pymoli_df[all_female]['SN'].nunique()
purchase_count_other = pymoli_df[all_other]['SN'].nunique()

gender_purchase = pd.Series([purchase_count_male, purchase_count_female, purchase_count_other],index=gender_index)


average_male_price = pymoli_df[all_male]["Price"].mean()
average_female_price = pymoli_df[all_female]["Price"].mean()
average_other_price = pymoli_df[all_other]["Price"].mean()

average_price_per_gender = pd.Series([average_male_price, average_female_price, average_other_price],index=gender_index)


total_male_price = pymoli_df[all_male]["Price"].sum()
total_female_price = pymoli_df[all_female]["Price"].sum()
total_other_price = pymoli_df[all_other]["Price"].sum()

total_purchase_value_per_gender = pd.Series([total_male_price, total_female_price, total_other_price],index=gender_index)

normalized_male = round(total_purchases/male_count,2)
normalized_female = round(total_purchases/female_count,2)
normalized_totals = round(total_purchases/other_count,2)

normalized_per_gender = pd.Series([normalized_male, normalized_female, normalized_totals], index=gender_index)

purchasing_analysis_df = pd.DataFrame({"Purchase Count":gender_purchase, "Average Purchase Price": average_price_per_gender, 
                                        "Total Purchase Value": total_purchase_value_per_gender, "Normalized Totals":normalized_per_gender})

purchasing_analysis_df['Average Purchase Price']=purchasing_analysis_df['Average Purchase Price'].map('${:,.2f}'.format)
purchasing_analysis_df['Total Purchase Value']=purchasing_analysis_df['Total Purchase Value'].map('${:,.2f}'.format)

purchasing_analysis_df

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Normalized Totals</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>$2.95</td>
      <td>1.68</td>
      <td>465</td>
      <td>$1,867.68</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>$2.82</td>
      <td>7.80</td>
      <td>100</td>
      <td>$382.91</td>
    </tr>
    <tr>
      <th>Other / Non-Disclosed</th>
      <td>$3.25</td>
      <td>97.50</td>
      <td>8</td>
      <td>$35.74</td>
    </tr>
  </tbody>
</table>
</div>




```python
pymoli_df["Age"].max()
```




    45




```python
pymoli_df["Age"].min()
```




    7




```python
#**Age Demographics**

#* The below each broken into bins of 4 years (i.e. >10, 10-14, 15-19, etc.) 
 # * Purchase Count
 # * Average Purchase Price
 # * Total Purchase Value
 # * Normalized Totals

bins = [0, 10, 14, 19, 24, 29, 34, 39, 100]
# the outter item is inclusive on the right

# Create labels for these bins
bins_labels = ["> 10", "10 to 14", "15 to 19", "20 to 24", "25 to 29", "30 to 34", "35 to 39", "40+"]

# Slice the data and place it into bins
pd.cut(pymoli_df["Age"], bins, labels=bins_labels)

pymoli_df["Age Demographics"] = pd.cut(pymoli_df["Age"], bins, labels=bins_labels)
pymoli_df.head()


```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Age</th>
      <th>Gender</th>
      <th>Item ID</th>
      <th>Item Name</th>
      <th>Price</th>
      <th>SN</th>
      <th>Age Demographics</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>38</td>
      <td>Male</td>
      <td>165</td>
      <td>Bone Crushing Silver Skewer</td>
      <td>3.37</td>
      <td>Aelalis34</td>
      <td>35 to 39</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>119</td>
      <td>Stormbringer, Dark Blade of Ending Misery</td>
      <td>2.32</td>
      <td>Eolo46</td>
      <td>20 to 24</td>
    </tr>
    <tr>
      <th>2</th>
      <td>34</td>
      <td>Male</td>
      <td>174</td>
      <td>Primitive Blade</td>
      <td>2.46</td>
      <td>Assastnya25</td>
      <td>30 to 34</td>
    </tr>
    <tr>
      <th>3</th>
      <td>21</td>
      <td>Male</td>
      <td>92</td>
      <td>Final Critic</td>
      <td>1.36</td>
      <td>Pheusrical25</td>
      <td>20 to 24</td>
    </tr>
    <tr>
      <th>4</th>
      <td>23</td>
      <td>Male</td>
      <td>63</td>
      <td>Stormfury Mace</td>
      <td>1.27</td>
      <td>Aela59</td>
      <td>20 to 24</td>
    </tr>
  </tbody>
</table>
</div>




```python
purchase_by_age = pymoli_df.groupby('Age Demographics')

purchase_count = purchase_by_age['Price'].count()
average_purchase = purchase_by_age['Price'].mean()
total_age_purchase = purchase_by_age['Price'].sum()
#normalized_totals = total_age_purchase / unique purchases

# Still need to format final value with $ .2f********

age_demographics_df = pd.DataFrame({'Purchase Count':purchase_count, 'Average Purchase Price':average_purchase,'Total Purchase Value':total_age_purchase}, index=age_index)

age_demographics_df['Average Purchase Price']=age_demographics_df['Average Purchase Price'].map('${:,.2f}'.format)
age_demographics_df['Total Purchase Value']=age_demographics_df['Total Purchase Value'].map('${:,.2f}'.format)

age_demographics_df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&gt; 10</th>
      <td>$3.02</td>
      <td>32</td>
      <td>$96.62</td>
    </tr>
    <tr>
      <th>10 to 14</th>
      <td>$2.70</td>
      <td>31</td>
      <td>$83.79</td>
    </tr>
    <tr>
      <th>15 to 19</th>
      <td>$2.91</td>
      <td>133</td>
      <td>$386.42</td>
    </tr>
    <tr>
      <th>20 to 24</th>
      <td>$2.91</td>
      <td>336</td>
      <td>$978.77</td>
    </tr>
    <tr>
      <th>25 to 29</th>
      <td>$2.96</td>
      <td>125</td>
      <td>$370.33</td>
    </tr>
    <tr>
      <th>30 to 34</th>
      <td>$3.08</td>
      <td>64</td>
      <td>$197.25</td>
    </tr>
    <tr>
      <th>35 to 39</th>
      <td>$2.84</td>
      <td>42</td>
      <td>$119.40</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>$3.16</td>
      <td>17</td>
      <td>$53.75</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Top Spenders**

#* Identify the the top 5 spenders in the game by total purchase value, then list (in a table):
 # * SN
  #* Purchase Count
  #* Average Purchase Price
  #* Total Purchase Value


group_spenders = pymoli_df.groupby("SN")
index=group_spenders

spenders_purchase_count = group_spenders['Price'].count()
average_purchase_price_by_spender = group_spenders['Price'].mean()
total_purchase_by_spender = group_spenders['Price'].sum()

top_spenders_df = pd.DataFrame({'Purchase Count':spenders_purchase_count, 'Average Purchase Price':average_purchase_price_by_spender,'Total Purchase Value':total_purchase_by_spender})

sorted_top_spenders = top_spenders_df.sort_values("Total Purchase Value", ascending=False)

sorted_top_spenders['Average Purchase Price']=sorted_top_spenders['Average Purchase Price'].map('${:,.2f}'.format)
sorted_top_spenders['Total Purchase Value']=sorted_top_spenders['Total Purchase Value'].map('${:,.2f}'.format)

sorted_top_spenders.head()

```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Purchase Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>SN</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Undirrala66</th>
      <td>$3.41</td>
      <td>5</td>
      <td>$17.06</td>
    </tr>
    <tr>
      <th>Saedue76</th>
      <td>$3.39</td>
      <td>4</td>
      <td>$13.56</td>
    </tr>
    <tr>
      <th>Mindimnya67</th>
      <td>$3.18</td>
      <td>4</td>
      <td>$12.74</td>
    </tr>
    <tr>
      <th>Haellysu29</th>
      <td>$4.24</td>
      <td>3</td>
      <td>$12.73</td>
    </tr>
    <tr>
      <th>Eoda93</th>
      <td>$3.86</td>
      <td>3</td>
      <td>$11.58</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Popular Items**

#* Identify the 5 most popular items by purchase count, then list (in a table):
#  * Item ID
#  * Item Name
#  * Purchase Count
#  * Item Price
#  * Total Purchase Value

item_count=pd.DataFrame(pymoli_df.groupby(['Item ID','Item Name']) ["Price"].count())
item_mean=pd.DataFrame(pymoli_df.groupby(["Item ID", "Item Name"])["Price"].mean())
item_total=pd.DataFrame(pymoli_df.groupby(["Item ID", "Item Name"])["Price"].sum())


#create dataframe with groupby objects
top_items=pd.DataFrame(
    {"Purchase Count": item_count['Price'], 
     "Item Price": (item_mean['Price']).map("${:,.2f}".format),
     "Total Purchase Value": (item_total['Price']).map("${:,.2f}".format)})
top_items=top_items.sort_values(["Purchase Count"], ascending=False)

top_items.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>39</th>
      <th>Betrayal, Whisper of Grieving Widows</th>
      <td>$2.35</td>
      <td>11</td>
      <td>$25.85</td>
    </tr>
    <tr>
      <th>84</th>
      <th>Arcane Gem</th>
      <td>$2.23</td>
      <td>11</td>
      <td>$24.53</td>
    </tr>
    <tr>
      <th>31</th>
      <th>Trickster</th>
      <td>$2.07</td>
      <td>9</td>
      <td>$18.63</td>
    </tr>
    <tr>
      <th>175</th>
      <th>Woeful Adamantite Claymore</th>
      <td>$1.24</td>
      <td>9</td>
      <td>$11.16</td>
    </tr>
    <tr>
      <th>13</th>
      <th>Serenity</th>
      <td>$1.49</td>
      <td>9</td>
      <td>$13.41</td>
    </tr>
  </tbody>
</table>
</div>




```python
#**Most Profitable Items**

#* Identify the 5 most profitable items by total purchase value, then list (in a table):
 # * Item ID
  #* Item Name
  #* Purchase Count
  #* Item Price
  #* Total Purchase Value


item_count=pd.DataFrame(pymoli_df.groupby(['Item ID','Item Name']) ["Price"].count())
item_mean=pd.DataFrame(pymoli_df.groupby(["Item ID", "Item Name"])["Price"].mean())
item_total=pd.DataFrame(pymoli_df.groupby(["Item ID", "Item Name"])["Price"].sum())

#create dataframe with groupby objects
top_profit=pd.DataFrame(
    {"Purchase Count": item_count['Price'], 
     "Item Price": (item_mean['Price']).map("${:,.2f}".format),
     "Total Purchase Value": (item_total['Price'])})

top_profit = top_profit.sort_values(["Total Purchase Value"], ascending=False)

top_profit['Total Purchase Value']=top_profit['Total Purchase Value'].map('${:,.2f}'.format)
top_profit.head()
#most_profit=pymoli_df.groupby(['Item ID', 'Item Name', 'Price']).Price.sum().nlargest(5)
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th></th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
    <tr>
      <th>Item ID</th>
      <th>Item Name</th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>34</th>
      <th>Retribution Axe</th>
      <td>$4.14</td>
      <td>9</td>
      <td>$37.26</td>
    </tr>
    <tr>
      <th>115</th>
      <th>Spectral Diamond Doomblade</th>
      <td>$4.25</td>
      <td>7</td>
      <td>$29.75</td>
    </tr>
    <tr>
      <th>32</th>
      <th>Orenmir</th>
      <td>$4.95</td>
      <td>6</td>
      <td>$29.70</td>
    </tr>
    <tr>
      <th>103</th>
      <th>Singed Scalpel</th>
      <td>$4.87</td>
      <td>6</td>
      <td>$29.22</td>
    </tr>
    <tr>
      <th>107</th>
      <th>Splitter, Foe Of Subtlety</th>
      <td>$3.61</td>
      <td>8</td>
      <td>$28.88</td>
    </tr>
  </tbody>
</table>
</div>


