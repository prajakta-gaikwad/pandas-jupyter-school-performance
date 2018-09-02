# District-wide schools' performance analysis
This project is to analyze the district-wide standardized test results. We have been given access to every student's math and reading scores, as well as various information on the schools they attend. our responsibility is to aggregate the data and showcase obvious trends in school performance.

#### Tech/framework used:

* Pandas Library

* Jupyter Notebook

(the data has thousands of records. Hacking through the data to look for obvious trends in Excel is not a feasible option. The size of the data may seem daunting, but *Pandas* will efficiently parse through it.)

### Observable trends based on the data analysis:
* As a whole, schools with higher budgets, did not yield better test results. By contrast, schools with higher spending per student actually underperformed compared to schools with smaller budgets. (around 74% overall passing in schools with higher budgets (\$645-675) vs 95% in schools spending less than \$585 per student.)
* Smaller and medium sized schools dramatically out-performed large sized schools, the difference is more obvious in terms of passing math performances (Around 94% passing vs 70%).
* Overall, charter schools out-performed the public district schools across all metrics. However, more analysis will be required to glean if the effect is due to school practices or the fact that charter schools tend to serve smaller student populations per school.

#### The final report includes the following:

* District Summary
  * a high level snapshot (in table form) of the district's key metrics, including:
    *  Total Schools
    *  Total Students
    *  Total Budget
    *  Average Math Score
    *  Average Reading Score
    *  % Passing Math
    *  % Passing Reading
    *  Overall Passing Rate (Average of the above two)
* School Summary
  * an overview table that summarizes key metrics about each school, including:
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
* Top and Bottom Performing Schools (By Passing Rate)
  * tables that highlights the top 5 and bottom 5 performing schools based on Overall Passing Rate, including:
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
* Math and Reading Scores by Grade
  * tables that list the average Math and Reading Score for students of each grade level (9th, 10th, 11th, 12th) at each school.
* Scores by School Spending, Size and Type 
  * tables that break down school performances based on average Spending Ranges (Per Student),school size and the type of school including:
    * Average Math Score
    * Average Reading Score
    * % Passing Math
    * % Passing Reading
    * Overall Passing Rate (Average of the above two)

