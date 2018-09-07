
# Schools' Analysis

* Overall, schools with higher budgets, did not yield better test results. By contrast, schools with higher spending per student actually  underperformed compared to schools with smaller budgets. (around 74% overall passing in schools with higher budgets (\$645-675) vs 95% in schools spending less than \$585 per student.)

* Smaller and medium sized schools dramatically out-performed large sized schools, the difference is more obvious in terms of passing math performances (Around 94% passing vs 70%).

* Overall, charter schools out-performed the public district schools across all metrics. However, more analysis will be required to validate if the effect is due to school practices or the fact that charter schools tend to serve smaller student populations per school. 
---

# Importing dependensies and csv files


```python
# Dependencies and Setup
import pandas as pd
import numpy as np

# File to Load
school_data_path = "Resources/schools_complete.csv"
student_data_path = "Resources/students_complete.csv"

# Read School and Student Data File and store into Pandas Data Frames
school_data = pd.read_csv(school_data_path)
student_data = pd.read_csv(student_data_path)

# Combine the data into a single dataset
school_data_complete = pd.merge(student_data, school_data, how="left", on=["school_name", "school_name"])
```

## District Summary

* Calculate the total number of schools

* Calculate the total number of students

* Calculate the total budget

* Calculate the average math score 

* Calculate the average reading score

* Calculate the overall passing rate (overall average score), i.e. (avg. math score + avg. reading score)/2

* Calculate the percentage of students with a passing math score (70 or greater)

* Calculate the percentage of students with a passing reading score (70 or greater)

* Create a dataframe to hold the above results

* Optional: give the displayed data cleaner formatting


```python
#school_data_complete.head()
#school_data_complete.columns
#school_data_complete.head()
#student_data["math_score"]
```


```python
#Return Series with number of distinct observations over requested axis.
total_schools = school_data_complete['school_name'].nunique()
total_students = school_data_complete['Student ID'].nunique()
total_budget = school_data['budget'].sum()
average_math_score = student_data['math_score'].mean()
average_reading_score = student_data['reading_score'].mean()
average_overall_score = (average_math_score + average_reading_score)/2
```


```python
math_passing_filter = student_data[student_data['math_score'] >= 70]
total_math_passed = math_passing_filter['math_score'].count()
percent_math_passed = (total_math_passed/total_students)*100
```


```python
reading_passing_filter = student_data[student_data['reading_score'] >= 70]
total_reading_passed = reading_passing_filter['reading_score'].count()
percent_reading_passed = (total_reading_passed/total_students)*100
```


```python
#Creating a DataFrame to hold above results and formatting the displayed data
district_summary = pd.DataFrame([[total_schools,total_students,\
                                  total_budget,\
                                  average_math_score,average_reading_score,\
                                  percent_math_passed,percent_reading_passed,\
      average_overall_score]], columns = ['Total Schools','Total Students',\
                                          'Total Budget','Average Math Score',\
                                          'Average Reading Score','%Passing Math',\
                                          '%Passing Reading','Overall Passing Rate'])
district_summary["Total Students"] = district_summary["Total Students"].astype(float).map("{:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)
district_summary["Average Math Score"] = district_summary["Average Math Score"].map("{:,.6f}".format)
district_summary["Average Reading Score"] = district_summary["Average Reading Score"].map("{:,.6f}".format)
district_summary["%Passing Math"] = district_summary["%Passing Math"].map("{:,.6f}".format)
district_summary["%Passing Reading"] = district_summary["%Passing Reading"].map("{:,.6f}".format)
district_summary["Overall Passing Rate"] = district_summary["Overall Passing Rate"].map("{:,.6f}".format)
```


```python
%%HTML
<style type="text/css">
table.dataframe td, table.dataframe th {
    border: 1px  black solid !important;
  color: black !important;
}
```


<style type="text/css">
table.dataframe td, table.dataframe th {
    border: 1px  black solid !important;
  color: black !important;
}


# High level snapshot of the District's key metrics


```python
district_summary
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
      <th>Total Schools</th>
      <th>Total Students</th>
      <th>Total Budget</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>15</td>
      <td>39,170.0</td>
      <td>$24,649,428.00</td>
      <td>78.985371</td>
      <td>81.877840</td>
      <td>74.980853</td>
      <td>85.805463</td>
      <td>80.431606</td>
    </tr>
  </tbody>
</table>
</div>



## School Summary

**Create an overview table that summarizes key metrics about each school, including:**
  * School Name
  * School Type
  * Total Students
  * Total School Budget
  * Per Student Budget
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
#school_data_complete.columns
```


```python
#Observations over requested axis.
school_names = school_data['school_name']
school_type = school_data['type']
school_data_complete_group = school_data_complete.groupby("School ID")
```


```python
total_students_per_school = school_data_complete_group['Student ID'].count()
total_school_budget = school_data['budget']
```


```python
per_student_budget = school_data['budget']/school_data['size']

```


```python
total_math_score = school_data_complete_group['math_score'].sum()
average_math_score_ps = total_math_score/total_students_per_school
```


```python
total_reading_score = school_data_complete_group['reading_score'].sum()
average_reading_score_ps = total_reading_score/total_students_per_school
```


```python
total_math_passed = school_data_complete[school_data_complete['math_score'] >= 70]
math_passed_per_school = total_math_passed.groupby("School ID")['Student ID'].count()
percent_math_passed_ps = (math_passed_per_school/total_students_per_school)*100
```


```python
total_reading_passed = school_data_complete[school_data_complete['reading_score'] >= 70]
reading_passed_per_school = total_reading_passed.groupby("School ID")['Student ID'].count()
percent_reading_passed_ps = (reading_passed_per_school/total_students_per_school)*100
```


```python
percent_overall_passed = (percent_math_passed_ps + percent_reading_passed_ps)/2
```


```python
#Creating a DataFrame to hold above results and formatting the displayed data
school_summary = pd.DataFrame({'Schools':school_names,'School Type':school_type,\
                               'Total Students':total_students_per_school,\
                               'Total School Budget':total_school_budget,\
                               'Per Student Budget':per_student_budget,\
                               'Average Math Score':average_math_score_ps,\
                               'Average Reading Score':average_reading_score_ps,\
                               '%Passing Math':percent_math_passed_ps,\
                               '%Passing Reading':percent_reading_passed_ps,\
                               'Overall Passing Rate':percent_overall_passed})

school_summary["Total School Budget"] = school_summary["Total School Budget"].map("${:,.2f}".format)
school_summary["Average Math Score"] = school_summary["Average Math Score"].map("{:,.6f}".format)
school_summary["Average Reading Score"] = school_summary["Average Reading Score"].map("{:,.6f}".format)
school_summary["%Passing Math"] = school_summary["%Passing Math"].map("{:,.6f}".format)
school_summary["%Passing Reading"] = school_summary["%Passing Reading"].map("{:,.6f}".format)
school_summary["Overall Passing Rate"] = school_summary["Overall Passing Rate"].map("{:,.6f}".format)
school_summary["Average Math Score"]=school_summary["Average Math Score"].astype(str).astype(float)
school_summary["Average Reading Score"] = school_summary["Average Reading Score"].astype(str).astype(float)
school_summary["%Passing Math"] = school_summary["%Passing Math"].astype(str).astype(float)
school_summary["%Passing Reading"] = school_summary["%Passing Reading"].astype(str).astype(float)
school_summary["Overall Passing Rate"] = school_summary["Overall Passing Rate"].astype(str).astype(float)
```


```python
# With proper formatting 
school_summary_fmt = school_summary[["Schools","Total Students","Total School Budget","Per Student Budget",\
                                     "Average Math Score","Average Math Score","Average Reading Score","%Passing Math","%Passing Reading",\
                                     "Overall Passing Rate"]]
school_summary_fmt["Per Student Budget"] = school_summary["Per Student Budget"].map("${:,.2f}".format)
```

## Key metrics of each school (School Summary)


```python
school_summary_fmt.head()
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
      <th>Schools</th>
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>Huang High School</td>
      <td>2917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Figueroa High School</td>
      <td>2949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>2</th>
      <td>Shelton High School</td>
      <td>1761</td>
      <td>$1,056,600.00</td>
      <td>$600.00</td>
      <td>83.359455</td>
      <td>83.359455</td>
      <td>83.725724</td>
      <td>93.867121</td>
      <td>95.854628</td>
      <td>94.860875</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Hernandez High School</td>
      <td>4635</td>
      <td>$3,022,020.00</td>
      <td>$652.00</td>
      <td>77.289752</td>
      <td>77.289752</td>
      <td>80.934412</td>
      <td>66.752967</td>
      <td>80.862999</td>
      <td>73.807983</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Griffin High School</td>
      <td>1468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
  </tbody>
</table>
</div>



## Top Performing Schools (By Passing Rate)


```python
school_summary_fmt.reset_index(drop=True).set_index('Schools').\
sort_values(by=['Overall Passing Rate'],ascending = False).head(5)
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
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Schools</th>
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
      <th>Cabrera High School</th>
      <td>1858</td>
      <td>$1,081,356.00</td>
      <td>$582.00</td>
      <td>83.061895</td>
      <td>83.061895</td>
      <td>83.975780</td>
      <td>94.133477</td>
      <td>97.039828</td>
      <td>95.586652</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>1635</td>
      <td>$1,043,130.00</td>
      <td>$638.00</td>
      <td>83.418349</td>
      <td>83.418349</td>
      <td>83.848930</td>
      <td>93.272171</td>
      <td>97.308869</td>
      <td>95.290520</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>962</td>
      <td>$585,858.00</td>
      <td>$609.00</td>
      <td>83.839917</td>
      <td>83.839917</td>
      <td>84.044699</td>
      <td>94.594595</td>
      <td>95.945946</td>
      <td>95.270270</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>1468</td>
      <td>$917,500.00</td>
      <td>$625.00</td>
      <td>83.351499</td>
      <td>83.351499</td>
      <td>83.816757</td>
      <td>93.392371</td>
      <td>97.138965</td>
      <td>95.265668</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>2283</td>
      <td>$1,319,574.00</td>
      <td>$578.00</td>
      <td>83.274201</td>
      <td>83.274201</td>
      <td>83.989488</td>
      <td>93.867718</td>
      <td>96.539641</td>
      <td>95.203679</td>
    </tr>
  </tbody>
</table>
</div>



## Bottom Performing Schools (By Passing Rate)


```python
school_summary_fmt.reset_index(drop=True).set_index('Schools').\
sort_values(by=['Overall Passing Rate']).head(5)
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
      <th>Total Students</th>
      <th>Total School Budget</th>
      <th>Per Student Budget</th>
      <th>Average Math Score</th>
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Schools</th>
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
      <td>3999</td>
      <td>$2,547,363.00</td>
      <td>$637.00</td>
      <td>76.842711</td>
      <td>76.842711</td>
      <td>80.744686</td>
      <td>66.366592</td>
      <td>80.220055</td>
      <td>73.293323</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>2949</td>
      <td>$1,884,411.00</td>
      <td>$639.00</td>
      <td>76.711767</td>
      <td>76.711767</td>
      <td>81.158020</td>
      <td>65.988471</td>
      <td>80.739234</td>
      <td>73.363852</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>2917</td>
      <td>$1,910,635.00</td>
      <td>$655.00</td>
      <td>76.629414</td>
      <td>76.629414</td>
      <td>81.182722</td>
      <td>65.683922</td>
      <td>81.316421</td>
      <td>73.500171</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>4761</td>
      <td>$3,094,650.00</td>
      <td>$650.00</td>
      <td>77.072464</td>
      <td>77.072464</td>
      <td>80.966394</td>
      <td>66.057551</td>
      <td>81.222432</td>
      <td>73.639992</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>2739</td>
      <td>$1,763,916.00</td>
      <td>$644.00</td>
      <td>77.102592</td>
      <td>77.102592</td>
      <td>80.746258</td>
      <td>68.309602</td>
      <td>79.299014</td>
      <td>73.804308</td>
    </tr>
  </tbody>
</table>
</div>




```python
#school_data_complete.columns
#school_summary.dtypes
```

## Scores by Grade

**Create a table that lists the average Math Score for students of each grade level (9th, 10th, 11th, 12th) at each school.**

Create a pandas series for each grade.

Group each series by school

Combine the series into a dataframe


```python
score_9thgrade = school_data_complete.loc[school_data_complete["grade"]=="9th"]
ninth_grade = score_9thgrade.groupby("School ID")[['math_score','reading_score']].mean()
```


```python
score_10thgrade = school_data_complete.loc[school_data_complete["grade"]=="10th"]
tenth_grade = score_10thgrade.groupby("School ID")[['math_score','reading_score']].mean()
```


```python
score_11thgrade = school_data_complete.loc[school_data_complete["grade"]=="11th"]
eleventh_grade = score_11thgrade.groupby("School ID")[['math_score','reading_score']].mean()
```


```python
score_12thgrade = school_data_complete.loc[school_data_complete["grade"]=="12th"]
twelth_grade = score_12thgrade.groupby("School ID")[['math_score','reading_score']].mean()
```


```python
math_score_bygrade = pd.DataFrame({'Schools':school_names,"9th":ninth_grade["math_score"],"10th":tenth_grade["math_score"],\
                                  "11th":eleventh_grade["math_score"],"12th":twelth_grade["math_score"]})
math_score_bygrade["9th"] = math_score_bygrade["9th"].map("{:,.6f}".format)
math_score_bygrade["10th"] = math_score_bygrade["10th"].map("{:,.6f}".format)
math_score_bygrade["11th"] = math_score_bygrade["11th"].map("{:,.6f}".format)
math_score_bygrade["12th"] = math_score_bygrade["12th"].map("{:,.6f}".format)
```

## Math Scores by Grade


```python
math_score_bygrade.reset_index(drop=True).set_index('Schools').sort_values(by=['Schools'])
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>Schools</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>77.083676</td>
      <td>76.996772</td>
      <td>77.515588</td>
      <td>76.492218</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.094697</td>
      <td>83.154506</td>
      <td>82.765560</td>
      <td>83.277487</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>76.403037</td>
      <td>76.539974</td>
      <td>76.884344</td>
      <td>77.151369</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>77.361345</td>
      <td>77.672316</td>
      <td>76.918058</td>
      <td>76.179963</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>82.044010</td>
      <td>84.229064</td>
      <td>83.842105</td>
      <td>83.356164</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>77.438495</td>
      <td>77.337408</td>
      <td>77.136029</td>
      <td>77.186567</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.787402</td>
      <td>83.429825</td>
      <td>85.000000</td>
      <td>82.855422</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>77.027251</td>
      <td>75.908735</td>
      <td>76.446602</td>
      <td>77.225641</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>77.187857</td>
      <td>76.691117</td>
      <td>77.491653</td>
      <td>76.863248</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.625455</td>
      <td>83.372000</td>
      <td>84.328125</td>
      <td>84.121547</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>76.859966</td>
      <td>76.612500</td>
      <td>76.395626</td>
      <td>77.690748</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>83.420755</td>
      <td>82.917411</td>
      <td>83.383495</td>
      <td>83.778976</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.590022</td>
      <td>83.087886</td>
      <td>83.498795</td>
      <td>83.497041</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.085578</td>
      <td>83.724422</td>
      <td>83.195326</td>
      <td>83.035794</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.264706</td>
      <td>84.010288</td>
      <td>83.836782</td>
      <td>83.644986</td>
    </tr>
  </tbody>
</table>
</div>



**Create a table that lists the average Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school:**

Create a pandas series for each grade.

Group each series by school

Combine the series into a dataframe


```python
reading_score_bygrade = pd.DataFrame({'Schools':school_names,"9th":ninth_grade["reading_score"],"10th":tenth_grade["reading_score"],\
                                  "11th":eleventh_grade["reading_score"],"12th":twelth_grade["reading_score"]})
reading_score_bygrade["9th"] = reading_score_bygrade["9th"].map("{:,.6f}".format)
reading_score_bygrade["10th"] = reading_score_bygrade["10th"].map("{:,.6f}".format)
reading_score_bygrade["11th"] = reading_score_bygrade["11th"].map("{:,.6f}".format)
reading_score_bygrade["12th"] = reading_score_bygrade["12th"].map("{:,.6f}".format)
```

## Reading Score by Grade 


```python
reading_score_bygrade.reset_index(drop=True).set_index('Schools').sort_values(by=['Schools'])
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
      <th>9th</th>
      <th>10th</th>
      <th>11th</th>
      <th>12th</th>
    </tr>
    <tr>
      <th>Schools</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Bailey High School</th>
      <td>81.303155</td>
      <td>80.907183</td>
      <td>80.945643</td>
      <td>80.912451</td>
    </tr>
    <tr>
      <th>Cabrera High School</th>
      <td>83.676136</td>
      <td>84.253219</td>
      <td>83.788382</td>
      <td>84.287958</td>
    </tr>
    <tr>
      <th>Figueroa High School</th>
      <td>81.198598</td>
      <td>81.408912</td>
      <td>80.640339</td>
      <td>81.384863</td>
    </tr>
    <tr>
      <th>Ford High School</th>
      <td>80.632653</td>
      <td>81.262712</td>
      <td>80.403642</td>
      <td>80.662338</td>
    </tr>
    <tr>
      <th>Griffin High School</th>
      <td>83.369193</td>
      <td>83.706897</td>
      <td>84.288089</td>
      <td>84.013699</td>
    </tr>
    <tr>
      <th>Hernandez High School</th>
      <td>80.866860</td>
      <td>80.660147</td>
      <td>81.396140</td>
      <td>80.857143</td>
    </tr>
    <tr>
      <th>Holden High School</th>
      <td>83.677165</td>
      <td>83.324561</td>
      <td>83.815534</td>
      <td>84.698795</td>
    </tr>
    <tr>
      <th>Huang High School</th>
      <td>81.290284</td>
      <td>81.512386</td>
      <td>81.417476</td>
      <td>80.305983</td>
    </tr>
    <tr>
      <th>Johnson High School</th>
      <td>81.260714</td>
      <td>80.773431</td>
      <td>80.616027</td>
      <td>81.227564</td>
    </tr>
    <tr>
      <th>Pena High School</th>
      <td>83.807273</td>
      <td>83.612000</td>
      <td>84.335938</td>
      <td>84.591160</td>
    </tr>
    <tr>
      <th>Rodriguez High School</th>
      <td>80.993127</td>
      <td>80.629808</td>
      <td>80.864811</td>
      <td>80.376426</td>
    </tr>
    <tr>
      <th>Shelton High School</th>
      <td>84.122642</td>
      <td>83.441964</td>
      <td>84.373786</td>
      <td>82.781671</td>
    </tr>
    <tr>
      <th>Thomas High School</th>
      <td>83.728850</td>
      <td>84.254157</td>
      <td>83.585542</td>
      <td>83.831361</td>
    </tr>
    <tr>
      <th>Wilson High School</th>
      <td>83.939778</td>
      <td>84.021452</td>
      <td>83.764608</td>
      <td>84.317673</td>
    </tr>
    <tr>
      <th>Wright High School</th>
      <td>83.833333</td>
      <td>83.812757</td>
      <td>84.156322</td>
      <td>84.073171</td>
    </tr>
  </tbody>
</table>
</div>



## School performances based on average spending ranges of schools per student

**Create a table that breaks down school performances based on average Spending Ranges (Per Student). Include in the table each of the following:**
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
#school_summary['Per Student Budget'].max()
#school_summary['Per Student Budget'].min()
#school_summary.columns
#school_data_complete
```


```python
#Using 4 bins to group school spending.
spending_bins = [0, 585, 615, 645, 675]
group_names = ["<$585", "$585-615", "$615-645", "$645-675"]
```


```python
school_summary['Spending Ranges(Per Student)'] = pd.cut(school_summary['Per Student Budget'],spending_bins,labels=group_names) 
```


```python
avg_math_score_by_spending = school_summary.groupby('Spending Ranges(Per Student)')['Average Math Score'].mean()
avg_reading_score_by_spending = school_summary.groupby('Spending Ranges(Per Student)')['Average Reading Score'].mean()
pct_passed_math_by_spending = school_summary.groupby('Spending Ranges(Per Student)')['%Passing Math'].mean()
pct_passed_reading_by_spending = school_summary.groupby('Spending Ranges(Per Student)')['%Passing Reading'].mean()
overall_passing_by_spending = (pct_passed_math_by_spending + pct_passed_reading_by_spending)/2
```


```python
scores_by_school_spending = pd.DataFrame({'Average Math Score':avg_math_score_by_spending,\
                                         'Average Reading Score':avg_reading_score_by_spending,\
                                         '%Passing Math':pct_passed_math_by_spending,\
                                          '%Passing Reading':pct_passed_reading_by_spending,\
                                          'Overall Passing Rate':overall_passing_by_spending})
scores_by_school_spending['Average Math Score']=scores_by_school_spending['Average Math Score'].map('{:.6f}'.format)
scores_by_school_spending['Average Reading Score']=scores_by_school_spending['Average Reading Score'].map('{:.6f}'.format)
scores_by_school_spending['%Passing Math']=scores_by_school_spending['%Passing Math'].map('{:.6f}'.format)
scores_by_school_spending['%Passing Reading']=scores_by_school_spending['%Passing Reading'].map('{:.6f}'.format)
scores_by_school_spending['Overall Passing Rate']=scores_by_school_spending['Overall Passing Rate'].map('{:.6f}'.format)
```

## Scores by School Spending


```python
scores_by_school_spending
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
    </tr>
    <tr>
      <th>Spending Ranges(Per Student)</th>
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
      <td>83.455399</td>
      <td>83.933814</td>
      <td>93.460096</td>
      <td>96.610877</td>
      <td>95.035486</td>
    </tr>
    <tr>
      <th>$585-615</th>
      <td>83.599686</td>
      <td>83.885211</td>
      <td>94.230858</td>
      <td>95.900287</td>
      <td>95.065573</td>
    </tr>
    <tr>
      <th>$615-645</th>
      <td>79.079225</td>
      <td>81.891436</td>
      <td>75.668212</td>
      <td>86.106569</td>
      <td>80.887391</td>
    </tr>
    <tr>
      <th>$645-675</th>
      <td>76.997210</td>
      <td>81.027843</td>
      <td>66.164813</td>
      <td>81.133951</td>
      <td>73.649382</td>
    </tr>
  </tbody>
</table>
</div>



## School Performances based on the size of the school

**Create a table that breaks down school performances based on the size of the school. Include in the table each of the following:**
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
#Using 3 bins to group school sizes.
size_bins = [0, 1000, 2000, 5000]
size_names = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
```


```python
school_summary['School Size'] = pd.cut(school_summary['Total Students'],size_bins,labels=size_names) 
```


```python
avg_math_score_by_size = school_summary.groupby('School Size')['Average Math Score'].mean()
avg_reading_score_by_size = school_summary.groupby('School Size')['Average Reading Score'].mean()
pct_passed_math_by_size = school_summary.groupby('School Size')['%Passing Math'].mean()
pct_passed_reading_by_size = school_summary.groupby('School Size')['%Passing Reading'].mean()
overall_passing_by_size = (pct_passed_math_by_size + pct_passed_reading_by_size)/2
```


```python
scores_by_school_size = pd.DataFrame({'Average Math Score':avg_math_score_by_size,\
                                         'Average Reading Score':avg_reading_score_by_size,\
                                         '%Passing Math':pct_passed_math_by_size,\
                                          '%Passing Reading':pct_passed_reading_by_size,\
                                          'Overall Passing Rate':overall_passing_by_size})
scores_by_school_size['Average Math Score']=scores_by_school_size['Average Math Score'].map('{:.6f}'.format)
scores_by_school_size['Average Reading Score']=scores_by_school_size['Average Reading Score'].map('{:.6f}'.format)
scores_by_school_size['%Passing Math']=scores_by_school_size['%Passing Math'].map('{:.6f}'.format)
scores_by_school_size['%Passing Reading']=scores_by_school_size['%Passing Reading'].map('{:.6f}'.format)
scores_by_school_size['Overall Passing Rate']=scores_by_school_size['Overall Passing Rate'].map('{:.6f}'.format)
```

## Scores by School Size


```python
scores_by_school_size
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
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
      <th>Small (&lt;1000)</th>
      <td>83.821598</td>
      <td>83.929844</td>
      <td>93.550225</td>
      <td>96.099436</td>
      <td>94.824831</td>
    </tr>
    <tr>
      <th>Medium (1000-2000)</th>
      <td>83.374684</td>
      <td>83.864438</td>
      <td>93.599695</td>
      <td>96.790680</td>
      <td>95.195187</td>
    </tr>
    <tr>
      <th>Large (2000-5000)</th>
      <td>77.746417</td>
      <td>81.344493</td>
      <td>69.963361</td>
      <td>82.766635</td>
      <td>76.364998</td>
    </tr>
  </tbody>
</table>
</div>



## School Performances based on the type of school

**Create a table that breaks down school performances based on the type of the school. Include in the table each of the following:**
  * Average Math Score
  * Average Reading Score
  * % Passing Math
  * % Passing Reading
  * Overall Passing Rate (Average of the above two)


```python
avg_math_by_type = school_summary.groupby('School Type')['Average Math Score'].mean()
avg_reading_by_type = school_summary.groupby('School Type')['Average Reading Score'].mean()
pct_passed_math_by_type = school_summary.groupby('School Type')['%Passing Math'].mean()
pct_passed_reading_by_type = school_summary.groupby('School Type')['%Passing Reading'].mean()
overall_passing_by_type = school_summary.groupby('School Type')['Overall Passing Rate'].mean()
```


```python
scores_by_school_type = pd.DataFrame({'Average Math Score':avg_math_by_type,\
                                         'Average Reading Score':avg_reading_by_type,\
                                         '%Passing Math':pct_passed_math_by_type,\
                                          '%Passing Reading':pct_passed_reading_by_type,\
                                          'Overall Passing Rate':overall_passing_by_type})
scores_by_school_type['Average Math Score']=scores_by_school_type['Average Math Score'].map('{:.6f}'.format)
scores_by_school_type['Average Reading Score']=scores_by_school_type['Average Reading Score'].map('{:.6f}'.format)
scores_by_school_type['%Passing Math']=scores_by_school_type['%Passing Math'].map('{:.6f}'.format)
scores_by_school_type['%Passing Reading']=scores_by_school_type['%Passing Reading'].map('{:.6f}'.format)
scores_by_school_type['Overall Passing Rate']=scores_by_school_type['Overall Passing Rate'].map('{:.6f}'.format)
```

## Scores by School Type


```python
scores_by_school_type
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
      <th>Average Math Score</th>
      <th>Average Reading Score</th>
      <th>%Passing Math</th>
      <th>%Passing Reading</th>
      <th>Overall Passing Rate</th>
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
      <td>83.473852</td>
      <td>83.896421</td>
      <td>93.620830</td>
      <td>96.586489</td>
      <td>95.103660</td>
    </tr>
    <tr>
      <th>District</th>
      <td>76.956733</td>
      <td>80.966636</td>
      <td>66.548453</td>
      <td>80.799062</td>
      <td>73.673757</td>
    </tr>
  </tbody>
</table>
</div>


