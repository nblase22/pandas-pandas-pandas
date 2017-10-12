

```python
# add definitions
import pandas as pd
import os
```


```python
# import school data file for use
# code assumes the file is in the raw_data folder
school_file = input("Please input the name of the school data file: ")
school_path = os.path.join('raw_data', school_file)


school_df = pd.read_csv(school_path)
school_df.head()
```

    Please input the name of the school data file: schools_complete.csv
    




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
      <th>School ID</th>
      <th>name</th>
      <th>type</th>
      <th>size</th>
      <th>budget</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Huang High School</td>
      <td>District</td>
      <td>2917</td>
      <td>1910635</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Figueroa High School</td>
      <td>District</td>
      <td>2949</td>
      <td>1884411</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Shelton High School</td>
      <td>Charter</td>
      <td>1761</td>
      <td>1056600</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Hernandez High School</td>
      <td>District</td>
      <td>4635</td>
      <td>3022020</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Griffin High School</td>
      <td>Charter</td>
      <td>1468</td>
      <td>917500</td>
    </tr>
  </tbody>
</table>
</div>




```python
# import student data file for use
# code assumes the file is in the raw_data folder
student_file = input("Please input the name of the student data file: ")
student_path = os.path.join('raw_data', student_file)
student_df = pd.read_csv(student_path)
student_df.head()
```

    Please input the name of the student data file: students_complete.csv
    




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
      <th>Student ID</th>
      <th>name</th>
      <th>gender</th>
      <th>grade</th>
      <th>school</th>
      <th>reading_score</th>
      <th>math_score</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>Paul Bradley</td>
      <td>M</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>66</td>
      <td>79</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1</td>
      <td>Victor Smith</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>94</td>
      <td>61</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2</td>
      <td>Kevin Rodriguez</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>90</td>
      <td>60</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>Dr. Richard Scott</td>
      <td>M</td>
      <td>12th</td>
      <td>Huang High School</td>
      <td>67</td>
      <td>58</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>Bonnie Ray</td>
      <td>F</td>
      <td>9th</td>
      <td>Huang High School</td>
      <td>97</td>
      <td>84</td>
    </tr>
  </tbody>
</table>
</div>




```python
# DISTRICT SUMMARY

#basic counts
total_schools = len(school_df["School ID"].unique())
total_students = len(student_df["Student ID"].unique())
total_budget = school_df["budget"].sum()

#averages
avg_math_score = round(student_df["math_score"].mean(),2)
avg_read_score = round(student_df["reading_score"].mean(),2)

# passing percentages, call 70 a passing grade
only_pass_math = student_df.loc[student_df["math_score"] > 70]
only_pass_read = student_df.loc[student_df["reading_score"] > 70]

count_pass_math = len(only_pass_math["Student ID"].unique())
count_pass_read = len(only_pass_read["Student ID"].unique())

pct_pass_math = round(count_pass_math/total_students * 100, 2)
pct_pass_read = round(count_pass_read/total_students * 100, 2)
overall_pass = round((pct_pass_math+pct_pass_read)/2, 2)

# create dataframe
summary_dist = pd.DataFrame({"Total Schools": [total_schools], "Total Students": [total_students], "Total Budget": [total_budget],
                            "Average Math Score": [avg_math_score], "Average Reading Score": [avg_read_score],
                            "% Passing Math": [pct_pass_math], "% Passing Reading": [pct_pass_read], "% Overall Passing Rate": [overall_pass]})

summary_dist["Total Budget"]= summary_dist["Total Budget"].map("${:,.0f}".format)

summary_dist_reindex = summary_dist[["Total Schools", "Total Students", "Total Budget", "Average Math Score", "Average Reading Score",
                            "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]

summary_dist_reindex
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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39170</td>
      <td>$24,649,428</td>
      <td>78.99</td>
      <td>81.88</td>
      <td>72.39</td>
      <td>82.97</td>
      <td>77.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# SCHOOL SUMMARY
school_df_clean = school_df.rename(columns= {"name": "school"})

merge_table = pd.merge(student_df, school_df_clean, on="school")


# group by
grouper = merge_table.groupby("school")


# get the sum and counting totals
school_type = grouper["type"].unique().str.get(0)

total_students_sch = grouper["Student ID"].count()
total_budget_sch = grouper["budget"].unique().astype(float)

per_student_bud = round((total_budget_sch/total_students_sch), 2).astype(float)



# get the average scores
avg_math_sch = round(grouper["math_score"].mean(), 2)
avg_read_sch = round(grouper["reading_score"].mean(), 2)


# passing percentages, call 70 a passing grade
only_pass_math_sch = merge_table.loc[merge_table["math_score"] > 70]
only_pass_read_sch = merge_table.loc[merge_table["reading_score"] > 70]

opms_group = only_pass_math_sch.groupby("school")
oprs_group = only_pass_read_sch.groupby("school")



count_pass_math_sch = opms_group["Student ID"].count()
count_pass_read_sch = oprs_group["Student ID"].count()

pct_pass_math_sch = round(count_pass_math_sch/total_students_sch * 100, 2)
pct_pass_read_sch = round(count_pass_read_sch/total_students_sch * 100, 2)
overall_pass_sch = round((pct_pass_math_sch+pct_pass_read_sch)/2, 2)

school_shc = pd.DataFrame({"School Type": school_type, "Total Students": total_students_sch, "Total School Budget": total_budget_sch, "Per Student Budget": per_student_bud, "Average Math Score": avg_math_sch, "Average Reading Score": avg_read_sch,
                          "% Passing Math": pct_pass_math_sch, "% Passing Reading": pct_pass_read_sch, "% Overall Passing Rate": overall_pass_sch})

school_sch_reindex = school_shc[["School Type", "Total Students", "Total School Budget", "Per Student Budget", "Average Math Score", "Average Reading Score", "% Passing Math", "% Passing Reading", "% Overall Passing Rate"]]

school_sch_reindex["Total School Budget"]= school_sch_reindex["Total School Budget"].map("${:,.0f}".format)
school_sch_reindex["Per Student Budget"]= school_sch_reindex["Per Student Budget"].map("${:,.0f}".format)

school_sch_reindex

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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>District</td>
      <td>4976</td>
      <td>$3,124,928</td>
      <td>$628</td>
      <td>77.05</td>
      <td>81.03</td>
      <td>64.63</td>
      <td>79.30</td>
      <td>71.96</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356</td>
      <td>$582</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>89.56</td>
      <td>93.86</td>
      <td>91.71</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411</td>
      <td>$639</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>63.75</td>
      <td>78.43</td>
      <td>71.09</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>District</td>
      <td>2739</td>
      <td>$1,763,916</td>
      <td>$644</td>
      <td>77.10</td>
      <td>80.75</td>
      <td>65.75</td>
      <td>77.51</td>
      <td>71.63</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>Charter</td>
      <td>1468</td>
      <td>$917,500</td>
      <td>$625</td>
      <td>83.35</td>
      <td>83.82</td>
      <td>89.71</td>
      <td>93.39</td>
      <td>91.55</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$3,022,020</td>
      <td>$652</td>
      <td>77.29</td>
      <td>80.93</td>
      <td>64.75</td>
      <td>78.19</td>
      <td>71.47</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087</td>
      <td>$581</td>
      <td>83.80</td>
      <td>83.81</td>
      <td>90.63</td>
      <td>92.74</td>
      <td>91.68</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635</td>
      <td>$655</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>63.32</td>
      <td>78.81</td>
      <td>71.06</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650</td>
      <td>$650</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>63.85</td>
      <td>78.28</td>
      <td>71.06</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858</td>
      <td>$609</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>91.68</td>
      <td>92.20</td>
      <td>91.94</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363</td>
      <td>$637</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>64.07</td>
      <td>77.74</td>
      <td>70.90</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>Charter</td>
      <td>1761</td>
      <td>$1,056,600</td>
      <td>$600</td>
      <td>83.36</td>
      <td>83.73</td>
      <td>89.89</td>
      <td>92.62</td>
      <td>91.26</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>Charter</td>
      <td>1635</td>
      <td>$1,043,130</td>
      <td>$638</td>
      <td>83.42</td>
      <td>83.85</td>
      <td>90.21</td>
      <td>92.91</td>
      <td>91.56</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574</td>
      <td>$578</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>90.93</td>
      <td>93.25</td>
      <td>92.09</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,049,400</td>
      <td>$583</td>
      <td>83.68</td>
      <td>83.96</td>
      <td>90.28</td>
      <td>93.44</td>
      <td>91.86</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Top Performing Schools
# sort by overall passing rate and print the top 5
top_performers = school_sch_reindex.nlargest(5, '% Overall Passing Rate')
top_performers
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Wilson High School</th>
      <td>Charter</td>
      <td>2283</td>
      <td>$1,319,574</td>
      <td>$578</td>
      <td>83.27</td>
      <td>83.99</td>
      <td>90.93</td>
      <td>93.25</td>
      <td>92.09</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>Charter</td>
      <td>962</td>
      <td>$585,858</td>
      <td>$609</td>
      <td>83.84</td>
      <td>84.04</td>
      <td>91.68</td>
      <td>92.20</td>
      <td>91.94</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>Charter</td>
      <td>1800</td>
      <td>$1,049,400</td>
      <td>$583</td>
      <td>83.68</td>
      <td>83.96</td>
      <td>90.28</td>
      <td>93.44</td>
      <td>91.86</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>Charter</td>
      <td>1858</td>
      <td>$1,081,356</td>
      <td>$582</td>
      <td>83.06</td>
      <td>83.98</td>
      <td>89.56</td>
      <td>93.86</td>
      <td>91.71</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>Charter</td>
      <td>427</td>
      <td>$248,087</td>
      <td>$581</td>
      <td>83.80</td>
      <td>83.81</td>
      <td>90.63</td>
      <td>92.74</td>
      <td>91.68</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Bottom Performing Schools
# bottom 5 schools by passing rate
bottom_performers = school_sch_reindex.nsmallest(5, '% Overall Passing Rate')
bottom_performers
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
      <th>School Type</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Rodriguez High School</th>
      <td>District</td>
      <td>3999</td>
      <td>$2,547,363</td>
      <td>$637</td>
      <td>76.84</td>
      <td>80.74</td>
      <td>64.07</td>
      <td>77.74</td>
      <td>70.90</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>District</td>
      <td>2917</td>
      <td>$1,910,635</td>
      <td>$655</td>
      <td>76.63</td>
      <td>81.18</td>
      <td>63.32</td>
      <td>78.81</td>
      <td>71.06</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>District</td>
      <td>4761</td>
      <td>$3,094,650</td>
      <td>$650</td>
      <td>77.07</td>
      <td>80.97</td>
      <td>63.85</td>
      <td>78.28</td>
      <td>71.06</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>District</td>
      <td>2949</td>
      <td>$1,884,411</td>
      <td>$639</td>
      <td>76.71</td>
      <td>81.16</td>
      <td>63.75</td>
      <td>78.43</td>
      <td>71.09</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>District</td>
      <td>4635</td>
      <td>$3,022,020</td>
      <td>$652</td>
      <td>77.29</td>
      <td>80.93</td>
      <td>64.75</td>
      <td>78.19</td>
      <td>71.47</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Math scores by grade
grade_grouper = merge_table.groupby(["school", "grade"])
avg_math_grd = round(grade_grouper["math_score"].mean(), 2)

math_grd = pd.DataFrame({"math_score": avg_math_grd})

math_grd_reorder = math_grd.pivot_table('math_score', 'school', 'grade')
math_grd_reorder.columns.name = None
math_grd_clean = math_grd_reorder[["9th", "10th", "11th", "12th"]]

math_grd_clean
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.08</td>
      <td>77.00</td>
      <td>77.52</td>
      <td>76.49</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.09</td>
      <td>83.15</td>
      <td>82.77</td>
      <td>83.28</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.40</td>
      <td>76.54</td>
      <td>76.88</td>
      <td>77.15</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.36</td>
      <td>77.67</td>
      <td>76.92</td>
      <td>76.18</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.04</td>
      <td>84.23</td>
      <td>83.84</td>
      <td>83.36</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.44</td>
      <td>77.34</td>
      <td>77.14</td>
      <td>77.19</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.79</td>
      <td>83.43</td>
      <td>85.00</td>
      <td>82.86</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.03</td>
      <td>75.91</td>
      <td>76.45</td>
      <td>77.23</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.19</td>
      <td>76.69</td>
      <td>77.49</td>
      <td>76.86</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.63</td>
      <td>83.37</td>
      <td>84.33</td>
      <td>84.12</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.86</td>
      <td>76.61</td>
      <td>76.40</td>
      <td>77.69</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.42</td>
      <td>82.92</td>
      <td>83.38</td>
      <td>83.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.59</td>
      <td>83.09</td>
      <td>83.50</td>
      <td>83.50</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.09</td>
      <td>83.72</td>
      <td>83.20</td>
      <td>83.04</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.26</td>
      <td>84.01</td>
      <td>83.84</td>
      <td>83.64</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Reading scores by grade
avg_read_grd = round(grade_grouper["reading_score"].mean(), 2)

read_grd = pd.DataFrame({"reading_score": avg_read_grd})

read_grd_reorder = read_grd.pivot_table('reading_score', 'school', 'grade')
read_grd_reorder.columns.name = None
read_grd_clean = read_grd_reorder[["9th", "10th", "11th", "12th"]]

read_grd_clean
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>school</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.30</td>
      <td>80.91</td>
      <td>80.95</td>
      <td>80.91</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.68</td>
      <td>84.25</td>
      <td>83.79</td>
      <td>84.29</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.20</td>
      <td>81.41</td>
      <td>80.64</td>
      <td>81.38</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.63</td>
      <td>81.26</td>
      <td>80.40</td>
      <td>80.66</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.37</td>
      <td>83.71</td>
      <td>84.29</td>
      <td>84.01</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.87</td>
      <td>80.66</td>
      <td>81.40</td>
      <td>80.86</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.68</td>
      <td>83.32</td>
      <td>83.82</td>
      <td>84.70</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.29</td>
      <td>81.51</td>
      <td>81.42</td>
      <td>80.31</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.26</td>
      <td>80.77</td>
      <td>80.62</td>
      <td>81.23</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.81</td>
      <td>83.61</td>
      <td>84.34</td>
      <td>84.59</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.99</td>
      <td>80.63</td>
      <td>80.86</td>
      <td>80.38</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.12</td>
      <td>83.44</td>
      <td>84.37</td>
      <td>82.78</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.73</td>
      <td>84.25</td>
      <td>83.59</td>
      <td>83.83</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.94</td>
      <td>84.02</td>
      <td>83.76</td>
      <td>84.32</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.83</td>
      <td>83.81</td>
      <td>84.16</td>
      <td>84.07</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Scores by school spending
# create the bins and group names
bins = [0, 585, 615, 645, 1000]
group_names = ["<$585", "$585-615", "$615-645", " >$645"]

school_shc_m = school_shc.reset_index()

double_merger = pd.merge(school_shc_m, merge_table, on="school")

# add the bin groups to the main data frame
double_merger["Spending Ranges (Per Student)"] = pd.cut(double_merger["Per Student Budget"], bins, labels=group_names)

# group by the spending range
group_spend = double_merger.groupby(['Spending Ranges (Per Student)'])

# Get the averages

g_math_avg = round(group_spend["math_score"].mean(), 2)
g_read_avg = round(group_spend["reading_score"].mean(), 2)

# getting the percentages
total_students_g = group_spend["Student ID"].count()
only_pass_math_g = double_merger.loc[double_merger["math_score"] > 70]
only_pass_read_g = double_merger.loc[double_merger["reading_score"] > 70]

ogm_group = only_pass_math_g.groupby("Spending Ranges (Per Student)")
ogr_group = only_pass_read_g.groupby("Spending Ranges (Per Student)")

count_pass_math_g = ogm_group["Student ID"].count()
count_pass_read_g = ogr_group["Student ID"].count()

pct_pass_math_g = round(count_pass_math_g/total_students_g * 100, 2)
pct_pass_read_g = round(count_pass_read_g/total_students_g * 100, 2)
overall_pass_g = round((pct_pass_math_g+pct_pass_read_g)/2, 2)

group_bins = pd.DataFrame({"Average Math Score": g_math_avg, "Average Reading Score": g_read_avg,
                          "% Passing Math": pct_pass_math_g, "% Passing Reading": pct_pass_read_g,
                          "% Overall Passing Rate": overall_pass_g})

group_bins_reindex = group_bins[["Average Math Score", "Average Reading Score", "% Passing Math"
                                , "% Passing Reading", "% Overall Passing Rate"]]

group_bins_reindex.reindex(["<$585", "$585-615", "$615-645", " >$645"])
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges (Per Student)</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>&lt;$585</th>
      <td>83.36</td>
      <td>83.96</td>
      <td>90.33</td>
      <td>93.45</td>
      <td>91.89</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.53</td>
      <td>83.84</td>
      <td>90.53</td>
      <td>92.47</td>
      <td>91.50</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>78.06</td>
      <td>81.43</td>
      <td>68.96</td>
      <td>80.95</td>
      <td>74.96</td>
    </tr>
    <tr>
      <th>&gt;$645</th>
      <td>77.05</td>
      <td>81.01</td>
      <td>64.06</td>
      <td>78.37</td>
      <td>71.22</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Scores by school size
# create the bins and group names
bins_ss = [0, 1000, 2000, 10000]
group_names_ss = ["Small (< 1000)", "Medium (1000-2000)", "Large (> 2000)"]

# add the bin groups to the main data frame
double_merger["School Size"] = pd.cut(double_merger["Total Students"], bins_ss, labels=group_names_ss)

# group by the size range
group_size = double_merger.groupby(['School Size'])

# Get the averages

ss_math_avg = round(group_size["math_score"].mean(), 2)
ss_read_avg = round(group_size["reading_score"].mean(), 2)

# getting the percentages
total_students_ss = group_size["Student ID"].count()
only_pass_math_ss = double_merger.loc[double_merger["math_score"] > 70]
only_pass_read_ss = double_merger.loc[double_merger["reading_score"] > 70]

ossm_group = only_pass_math_ss.groupby("School Size")
ossr_group = only_pass_read_ss.groupby("School Size")

count_pass_math_ss = ossm_group["Student ID"].count()
count_pass_read_ss = ossr_group["Student ID"].count()

pct_pass_math_ss = round(count_pass_math_ss/total_students_ss * 100, 2)
pct_pass_read_ss = round(count_pass_read_ss/total_students_ss * 100, 2)
overall_pass_ss = round((pct_pass_math_ss+pct_pass_read_ss)/2, 2)

# making the data frame
group_bins_ss = pd.DataFrame({"Average Math Score": ss_math_avg, "Average Reading Score": ss_read_avg,
                          "% Passing Math": pct_pass_math_ss, "% Passing Reading": pct_pass_read_ss,
                          "% Overall Passing Rate": overall_pass_ss})

group_bins_reindex_ss = group_bins_ss[["Average Math Score", "Average Reading Score", "% Passing Math"
                                , "% Passing Reading", "% Overall Passing Rate"]]

group_bins_reindex_ss.reindex(["Small (< 1000)", "Medium (1000-2000)", "Large (> 2000)"])
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Size</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Small (&lt; 1000)</th>
      <td>83.83</td>
      <td>83.97</td>
      <td>91.36</td>
      <td>92.37</td>
      <td>91.86</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.37</td>
      <td>83.87</td>
      <td>89.93</td>
      <td>93.25</td>
      <td>91.59</td>
    </tr>
    <tr>
      <th>Large (&gt; 2000)</th>
      <td>77.48</td>
      <td>81.20</td>
      <td>66.38</td>
      <td>79.53</td>
      <td>72.96</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Scores by school type
# group by the size range
group_type = merge_table.groupby(['type'])

# Get the averages

st_math_avg = round(group_type["math_score"].mean(), 2)
st_read_avg = round(group_type["reading_score"].mean(), 2)

# getting the percentages
total_students_st = group_type["Student ID"].count()
only_pass_math_st = merge_table.loc[merge_table["math_score"] > 70]
only_pass_read_st = merge_table.loc[merge_table["reading_score"] > 70]

ostm_group = only_pass_math_st.groupby("type")
ostr_group = only_pass_read_st.groupby("type")


count_pass_math_st = ostm_group["Student ID"].count()
count_pass_read_st = ostr_group["Student ID"].count()

pct_pass_math_st = round(count_pass_math_st/total_students_st * 100, 2)
pct_pass_read_st = round(count_pass_read_st/total_students_st * 100, 2)
overall_pass_st = round((pct_pass_math_st+pct_pass_read_st)/2, 2)

# making the data frame and formatting it
group_bins_st = pd.DataFrame({"Average Math Score": st_math_avg, "Average Reading Score": st_read_avg,
                          "% Passing Math": pct_pass_math_st, "% Passing Reading": pct_pass_read_st,
                          "% Overall Passing Rate": overall_pass_st})

group_bins_reindex_st = group_bins_st[["Average Math Score", "Average Reading Score", "% Passing Math"
                                , "% Passing Reading", "% Overall Passing Rate"]]

group_bins_reindex_st.index.names = ['School Type']

group_bins_reindex_st
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>% Passing Math</th>
      <th>% Passing Reading</th>
      <th>% Overall Passing Rate</th>
    </tr>
    <tr>
      <th>School Type</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Charter</th>
      <td>83.41</td>
      <td>83.90</td>
      <td>90.28</td>
      <td>93.15</td>
      <td>91.72</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.99</td>
      <td>80.96</td>
      <td>64.31</td>
      <td>78.37</td>
      <td>71.34</td>
    </tr>
  </tbody>
</table>
</div>




```python

```
