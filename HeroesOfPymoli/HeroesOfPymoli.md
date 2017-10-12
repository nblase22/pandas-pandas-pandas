

```python
# import pandas
import pandas as pd

```


```python
#import data file for use
# Data formatted as SN, Age, Gender, Item ID, Item Name, Price
# code assumes that the file path is in the same folder as the .py similar to how the repository was structured on download
json_path = input("Please input the name of the datafile: ")

p_data_df = pd.read_json(json_path)

p_data_df.head()
```

    Please input the name of the datafile: purchase_data.json
    




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
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
      <td>20</td>
      <td>Male</td>
      <td>93</td>
      <td>Apocalyptic Battlescythe</td>
      <td>4.49</td>
      <td>Iloni35</td>
    </tr>
    <tr>
      <th>1</th>
      <td>21</td>
      <td>Male</td>
      <td>12</td>
      <td>Dawne</td>
      <td>3.36</td>
      <td>Aidaira26</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17</td>
      <td>Male</td>
      <td>5</td>
      <td>Putrid Fan</td>
      <td>2.63</td>
      <td>Irim47</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17</td>
      <td>Male</td>
      <td>123</td>
      <td>Twilight's Carver</td>
      <td>2.55</td>
      <td>Irith83</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22</td>
      <td>Male</td>
      <td>154</td>
      <td>Feral Katana</td>
      <td>4.11</td>
      <td>Philodil43</td>
    </tr>
  </tbody>
</table>
</div>




```python
# PLAYER COUNT
# get number of players
player_count = len(p_data_df["SN"].unique())
player_count_df = pd.DataFrame({"Total Number of Players": [player_count]})
player_count_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Total Number of Players</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>74</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis

# number of unique items
num_unique_items = len(p_data_df["Item Name"].unique())

# average purchase price
avg_purch_price = round(p_data_df["Price"].mean(), 2)

# total number of purchases
tot_purch = p_data_df["SN"].count()

# total revenue
tot_rev = round(p_data_df["Price"].sum(), 2)

purch_ana_df = pd.DataFrame({"Number of Unique Items": [num_unique_items], "Number of Purchases": [tot_purch],
                             "Average Price": ["$" + str(avg_purch_price)], "Total Revenue": ["$" + str(tot_rev)]})

purch_ana_df = purch_ana_df[["Number of Unique Items", "Average Price", "Number of Purchases", "Total Revenue"]]
purch_ana_df
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Number of Unique Items</th>
      <th>Average Price</th>
      <th>Number of Purchases</th>
      <th>Total Revenue</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>$2.92</td>
      <td>78</td>
      <td>$228.1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Gender Demographics
# percentage and count of males
only_male = p_data_df.loc[p_data_df["Gender"] == "Male", :]
count_male = len(only_male["SN"].unique())
pct_male = round(count_male/player_count*100 , 2)

# percentage and count of females
only_female = p_data_df.loc[p_data_df["Gender"] == "Female", :]
count_female = len(only_female["SN"].unique())
pct_female = round(count_female/player_count*100 , 2)

# percentage and count of Other/Non-Disclosed
only_other_nd = p_data_df.loc[p_data_df["Gender"] == "Other / Non-Disclosed", :]
count_nd = len(only_other_nd["SN"].unique())
pct_nd = round(count_nd/player_count*100 , 2)

# create the data frame for these items
gender_dem_df = pd.DataFrame({"Gender": ["Male", "Female", "Other/Non-Disclosed"], 
                              "Percentage of Players": [pct_male, pct_female, pct_nd], 
                             "Total Count": [count_male, count_female, count_nd]})

# reindex the data frame to get a usable format
gender_dem_reindex = gender_dem_df.set_index("Gender")
gender_dem_reindex
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Percentage of Players</th>
      <th>Total Count</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>81.08</td>
      <td>60</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>17.57</td>
      <td>13</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1.35</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Purchasing Analysis (Gender)
# ----- Broken Down By Gender------
# purchase count
male_purch_cnt = only_male["SN"].count()
female_purch_cnt = only_female["SN"].count()
nd_purch_cnt = only_other_nd["SN"].count()

# average purchase price
male_avg_purch = round(only_male["Price"].mean(), 2)
female_avg_purch = round(only_female["Price"].mean(), 2)
nd_avg_purch = round(only_other_nd["Price"].mean(), 2)

# total purchase value
male_val_tot = only_male["Price"].sum()
female_val_tot = only_female["Price"].sum()
nd_val_tot = only_other_nd["Price"].sum()


# normalized totals (val tot / player count), this was the original option
male_norm_tot = round(male_val_tot/count_male, 2)
female_norm_tot = round(female_val_tot/count_female, 2)
nd_norm_tot = round(nd_val_tot/count_nd, 2)

# set up the dataframe
gen_purch_ana_df = pd.DataFrame({"Gender": ["Male", "Female", "Other/Non-Disclosed"],
                                "Purchase Count": [male_purch_cnt, female_purch_cnt, nd_purch_cnt],
                                "Average Purchase Price": ["$" + str(male_avg_purch), "$" + str(female_avg_purch), "$" + str(nd_avg_purch)],
                                "Total Purchase Value": ["$" + str(male_val_tot), "$" + str(female_val_tot), "$" + str(nd_val_tot)],
                                "Normalized Totals" : ["$" + str(male_norm_tot), "$" + str(female_norm_tot), "$" + str(nd_norm_tot)]})

# reindex and print
gen_purch_ana_reindex = gen_purch_ana_df.set_index("Gender")
gen_purch_ana_reindex = gen_purch_ana_reindex[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
gen_purch_ana_reindex
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
    <tr>
      <th>Gender</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Male</th>
      <td>64</td>
      <td>$2.88</td>
      <td>$184.6</td>
      <td>$3.08</td>
    </tr>
    <tr>
      <th>Female</th>
      <td>13</td>
      <td>$3.18</td>
      <td>$41.38</td>
      <td>$3.18</td>
    </tr>
    <tr>
      <th>Other/Non-Disclosed</th>
      <td>1</td>
      <td>$2.12</td>
      <td>$2.12</td>
      <td>$2.12</td>
    </tr>
  </tbody>
</table>
</div>




```python
# AGE DEMOGRAPHICS
# Break down into bins, list purchase count, average purchase price, total purchase value, normalized totals

# create the bins and group names
bins = [0, 9, 14, 19, 24, 29, 34, 39, 150]
group_names = ["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"]

# add the bin groups to the main data frame
p_data_df["Age Group"] = pd.cut(p_data_df["Age"], bins, labels=group_names)

# group by the age range
group_age = p_data_df.groupby(['Age Group'])

# get the column values desired for these groups and format them
age_group_cnt = p_data_df["Age Group"].value_counts()
age_group_avg = round(group_age["Price"].mean(), 2).map("${:.2f}".format)
age_group_tot = group_age["Price"].sum()
age_group_norm = round(age_group_tot/age_group_cnt, 2)
age_group_tot = age_group_tot.map("${:.2f}".format)
age_group_norm = age_group_norm.map("${:.2f}".format)

# create the data frame we need
age_demo_df = pd.DataFrame({"Purchase Count": age_group_cnt, "Average Purchase Price": age_group_avg,
                           "Total Purchase Value": age_group_tot, "Normalized Totals": age_group_norm})

# reorder the columns like we want
age_demo_df = age_demo_df[["Purchase Count", "Average Purchase Price", "Total Purchase Value", "Normalized Totals"]]
age_demo_df.reindex(["<10", "10-14", "15-19", "20-24", "25-29", "30-34", "35-39", "40+"])
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Purchase Count</th>
      <th>Average Purchase Price</th>
      <th>Total Purchase Value</th>
      <th>Normalized Totals</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;10</th>
      <td>5</td>
      <td>$2.76</td>
      <td>$13.82</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>10-14</th>
      <td>3</td>
      <td>$2.99</td>
      <td>$8.96</td>
      <td>$2.99</td>
    </tr>
    <tr>
      <th>15-19</th>
      <td>11</td>
      <td>$2.76</td>
      <td>$30.41</td>
      <td>$2.76</td>
    </tr>
    <tr>
      <th>20-24</th>
      <td>36</td>
      <td>$3.02</td>
      <td>$108.89</td>
      <td>$3.02</td>
    </tr>
    <tr>
      <th>25-29</th>
      <td>9</td>
      <td>$2.90</td>
      <td>$26.11</td>
      <td>$2.90</td>
    </tr>
    <tr>
      <th>30-34</th>
      <td>7</td>
      <td>$1.98</td>
      <td>$13.89</td>
      <td>$1.98</td>
    </tr>
    <tr>
      <th>35-39</th>
      <td>6</td>
      <td>$3.56</td>
      <td>$21.37</td>
      <td>$3.56</td>
    </tr>
    <tr>
      <th>40+</th>
      <td>1</td>
      <td>$4.65</td>
      <td>$4.65</td>
      <td>$4.65</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Spenders
# id the top 5 spenders by total purchase value, List: SN, Purchase Count, Item Price, Total Purchase Value

# group by the SN
sn_group = p_data_df.groupby(['SN'])

# get the data we need for our columns, and format them
sn_purch_count = p_data_df["SN"].value_counts()
sn_avg = round(sn_group["Price"].mean(), 2).map("${:.2f}".format)
sn_tot = sn_group["Price"].sum().map("${:.2f}".format)

# create the data frame
sn_df = pd.DataFrame({"Purchase Count": sn_purch_count, "Average Item Price": sn_avg, "Total Purchase Value": sn_tot})

# sort by total purchase value and print the top 5
top_spenders = sn_df.sort_values(['Total Purchase Value'], ascending = False)
top_spenders.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Average Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Sundaky74</th>
      <td>$3.70</td>
      <td>2</td>
      <td>$7.41</td>
    </tr>
    <tr>
      <th>Aidaira26</th>
      <td>$2.56</td>
      <td>2</td>
      <td>$5.13</td>
    </tr>
    <tr>
      <th>Eusty71</th>
      <td>$4.81</td>
      <td>1</td>
      <td>$4.81</td>
    </tr>
    <tr>
      <th>Chanirra64</th>
      <td>$4.78</td>
      <td>1</td>
      <td>$4.78</td>
    </tr>
    <tr>
      <th>Alarap40</th>
      <td>$4.71</td>
      <td>1</td>
      <td>$4.71</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Popular Items
# id 5 most pop items by purchase count, List: Item ID, Item Name, Purchase Count, Item Price, Total Purchase Value

# group by item name
item_group = p_data_df.groupby(['Item Name'])

# get the columns we want in the proper format
item_purch_count = p_data_df["Item Name"].value_counts()
item_id = item_group["Item ID"].mean()
item_price = round(item_group["Price"].mean(), 2).map("${:.2f}".format)
item_tot = item_group["Price"].sum().map("${:.2f}".format)

# create the data frame
item_df = pd.DataFrame({"Item ID": item_id, "Purchase Count": item_purch_count, "Item Price": item_price, "Total Purchase Value": item_tot})

#sort by purchase count
pop_items = item_df.sort_values(['Purchase Count'], ascending = False)
pop_items.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Mourning Blade</th>
      <td>94</td>
      <td>$3.64</td>
      <td>3</td>
      <td>$10.92</td>
    </tr>
    <tr>
      <th>Deadline, Voice Of Subtlety</th>
      <td>98</td>
      <td>$1.29</td>
      <td>2</td>
      <td>$2.58</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>180</td>
      <td>$2.77</td>
      <td>2</td>
      <td>$5.54</td>
    </tr>
    <tr>
      <th>Relentless Iron Skewer</th>
      <td>176</td>
      <td>$2.12</td>
      <td>2</td>
      <td>$4.24</td>
    </tr>
    <tr>
      <th>Apocalyptic Battlescythe</th>
      <td>93</td>
      <td>$4.49</td>
      <td>2</td>
      <td>$8.98</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Most Profitable Items
# id the 5 most profitable items by total purchase value, List: Item Id, Item Name, Purchase Count, Item Price, TPV


# sort by total purchase value
prof_items = item_df.sort_values(['Total Purchase Value'], ascending = False)
prof_items.head()
```




<div>
<style>
    .dataframe thead tr:only-child th {
        text-align: right;
    }

    .dataframe thead th {
        text-align: left;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Item ID</th>
      <th>Item Price</th>
      <th>Purchase Count</th>
      <th>Total Purchase Value</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Heartstriker, Legacy of the Light</th>
      <td>117</td>
      <td>$4.71</td>
      <td>2</td>
      <td>$9.42</td>
    </tr>
    <tr>
      <th>Apocalyptic Battlescythe</th>
      <td>93</td>
      <td>$4.49</td>
      <td>2</td>
      <td>$8.98</td>
    </tr>
    <tr>
      <th>Betrayer</th>
      <td>90</td>
      <td>$4.12</td>
      <td>2</td>
      <td>$8.24</td>
    </tr>
    <tr>
      <th>Feral Katana</th>
      <td>154</td>
      <td>$4.11</td>
      <td>2</td>
      <td>$8.22</td>
    </tr>
    <tr>
      <th>Stormcaller</th>
      <td>180</td>
      <td>$2.77</td>
      <td>2</td>
      <td>$5.54</td>
    </tr>
  </tbody>
</table>
</div>




```python

```


```python

```
