# School District Analysis
## Overview of the School District Analysis:
The school board has notified Maria and her supervisor about the evidence of academic dishonesty; specifically, reading and math grades for Thomas High School ninth graders. Although the school board does not know the full extent of the academic dishonesty, they want to uphold state-testing standards and have turned to Maria for help. Maria has asked to replace the math and reading scores for 9th Graders in Thomas High School to be omitted while keeping the rest of the data intact. Once this function is complete, Maria has asked to repeat the school district analysis that you did and write up a report to describe how these changes affected the overall analysis.

```python
# Dependencies and Setup
import pandas as pd

# File to Load (Remember to change the path if needed.)
school_data_to_load = ("C:/Users/ritwi/OneDrive/Desktop/Classwork/Weekly Challenges/Challenge 4/Resources/schools_complete.csv")
student_data_to_load = ("C:/Users/ritwi/OneDrive/Desktop/Classwork/Weekly Challenges/Challenge 4/Resources/students_complete.csv")

# Read the School Data and Student Data and store into a Pandas DataFrame
school_data_df = pd.read_csv(school_data_to_load)
student_data_df = pd.read_csv(student_data_to_load)

# Cleaning Student Names and Replacing Substrings in a Python String
# Add each prefix and suffix to remove to a list.
prefixes_suffixes = ["Dr. ", "Mr. ","Ms. ", "Mrs. ", "Miss ", " MD", " DDS", " DVM", " PhD"]

# Iterate through the words in the "prefixes_suffixes" list and replace them with an empty space, "".
for word in prefixes_suffixes:
    student_data_df["student_name"] = student_data_df["student_name"].str.replace(word,"")

# Check names.
student_data_df.head(10)

# Install numpy using conda install numpy or pip install numpy. 
# Step 1. Import numpy as np.
import numpy as np

# Step 2. Use the loc method on the student_data_df to select all the reading scores from the 9th grade at Thomas High School and replace them with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "reading_score"]=np.nan

#  Step 3. Refactor the code in Step 2 to replace the math scores with NaN.
student_data_df.loc[(student_data_df["school_name"] == "Thomas High School") & (student_data_df["grade"] == "9th"), "math_score"]=np.nan

```

## Results:
### How is the district summary affected?
By omitting grade 9th student data from the school district summary, the change in results was very insignificant. A total of 461 student’s maths & reading scores were attributed to Grade 9th Thomas High School students out of a total of 39,170 students in the whole district. This number is 1.1% and hence has not made a significant impact on the school district data.
```python
# Combine the data into a single dataset
school_data_complete_df = pd.merge(student_data_df, school_data_df, how="left", on=["school_name", "school_name"])
# Calculate the Totals (Schools and Students)
school_count = len(school_data_complete_df["school_name"].unique())
student_count = school_data_complete_df["Student ID"].count()

# Calculate the Total Budget
total_budget = school_data_df["budget"].sum()

# Calculate the Average Scores using the "clean_student_data".
average_reading_score = school_data_complete_df["reading_score"].mean()
average_math_score = school_data_complete_df["math_score"].mean()

# Step 1. Get the number of students that are in ninth grade at Thomas High School.
# These students have no grades. 
Thomas_Student_count = school_data_complete_df.loc[(school_data_complete_df["school_name"] == "Thomas High School") & (school_data_complete_df["grade"] == "9th")]
ninth_student_count = Thomas_Student_count["Student ID"].count()


# Get the total student count 
student_count = school_data_complete_df["Student ID"].count()


# Step 2. Subtract the number of students that are in ninth grade at 
# Thomas High School from the total student count to get the new total student count.
new_total_student_df = (student_count - ninth_student_count)

# Calculate the passing rates using the "clean_student_data".
passing_math_count = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)].count()["student_name"]
passing_reading_count = school_data_complete_df[(school_data_complete_df["reading_score"] >= 70)].count()["student_name"]

# Step 3. Calculate the passing percentages with the new total student count.
passing_math_percentage = passing_math_count / float(new_total_student_df) * 100
passing_reading_percentage = passing_reading_count / float(new_total_student_df) * 100

# Calculate the students who passed both reading and math.
passing_math_reading = school_data_complete_df[(school_data_complete_df["math_score"] >= 70)
                                               & (school_data_complete_df["reading_score"] >= 70)]

# Calculate the number of students that passed both reading and math.
overall_passing_math_reading_count = passing_math_reading["student_name"].count()


# Step 4.Calculate the overall passing percentage with new total student count.
overall_passing_percentage = overall_passing_math_reading_count / (new_total_student_df) * 100



# Create a DataFrame
district_summary_df = pd.DataFrame(
          [{"Total Schools": school_count, 
          "Total Students": student_count, 
          "Total Budget": total_budget,
          "Average Math Score": average_math_score, 
          "Average Reading Score": average_reading_score,
          "% Passing Math": passing_math_percentage,
         "% Passing Reading": passing_reading_percentage,
        "% Overall Passing": overall_passing_percentage}])



# Format the "Total Students" to have the comma for a thousands separator.
district_summary_df["Total Students"] = district_summary_df["Total Students"].map("{:,}".format)
# Format the "Total Budget" to have the comma for a thousands separator, a decimal separator and a "$".
district_summary_df["Total Budget"] = district_summary_df["Total Budget"].map("${:,.2f}".format)
# Format the columns.
district_summary_df["Average Math Score"] = district_summary_df["Average Math Score"].map("{:.1f}".format)
district_summary_df["Average Reading Score"] = district_summary_df["Average Reading Score"].map("{:.1f}".format)
district_summary_df["% Passing Math"] = district_summary_df["% Passing Math"].map("{:.1f}".format)
district_summary_df["% Passing Reading"] = district_summary_df["% Passing Reading"].map("{:.1f}".format)
district_summary_df["% Overall Passing"] = district_summary_df["% Overall Passing"].map("{:.1f}".format)

# Display the data frame
district_summary_df

<style  type="text/css" >
</style>  
<table id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691" > 
<thead>    <tr> 
        <th class="blank level0" ></th> 
        <th class="col_heading level0 col0" >Total Schools</th> 
        <th class="col_heading level0 col1" >Total Students</th> 
        <th class="col_heading level0 col2" >Total Budget</th> 
        <th class="col_heading level0 col3" >Average Reading Score</th> 
        <th class="col_heading level0 col4" >Average Math Score</th> 
        <th class="col_heading level0 col5" >% Passing Reading</th> 
        <th class="col_heading level0 col6" >% Passing Math</th> 
        <th class="col_heading level0 col7" >Overall Passing Rate</th> 
    </tr></thead> 
<tbody>    <tr> 
        <th id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691" class="row_heading level0 row0" >0</th> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col0" class="data row0 col0" >15</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col1" class="data row0 col1" >39170</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col2" class="data row0 col2" >$24,649,428.00</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col3" class="data row0 col3" >81.9</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col4" class="data row0 col4" >79.0</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col5" class="data row0 col5" >85.8%</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col6" class="data row0 col6" >75.0%</td> 
        <td id="T_1a0abed2_9a44_11e7_bb3a_0c4de9c48691row0_col7" class="data row0 col7" >65.2%</td> 
    </tr></tbody> 
</table> 


```
### How is the school summary affected?
The school summary shows a significant impact of the results from omission of 9th grade students at Thomas High School. Prior to omission of these 9th graders, the passing scores for math, reading & overall passing for Thomas High was 66.91%, 69.67% & 65.07% respectively. After omitting these students, the passing scores for math, reading & overall passing for Thomas High now is 93.19%, 97.01% & 90.63% respectively.
### How does replacing the ninth graders’ math and reading scores affect Thomas High School’s performance relative to the other schools?
After omitting the passing scores for math, reading & overall passing for 9th graders in Thomas High, the school is now the within the top 5 schools in the district and is in the 2nd position.
```python
# Sort and show top five schools.
top_schools = per_school_summary_df.sort_values(["% Overall Passing"], ascending=False)
top_schools.head()
```

### How does replacing the ninth-grade scores affect the following:
#### Math and reading scores by grade:
The values for math & reading scores are set to nan for 9th graders in Thomas high. Hence there is not a significant impact to the data itself.
#### Scores by school spending
Spending data is affected a little because of the omission sine now it appears as if all the allocation of funds has been made towards grades 10th, 11th & 12th because of nan values of grade 9th Students.
#### Scores by school size
After omitting 641 students from grade 9th the school size for Thomas High changes from 1635 to 994 which may cause the school to now be classified as a small school as opposed to medium
#### Scores by school type
There is no change to school type. Thomas High was classified as a charter prior to omission and remains to be a charter past omission too.
## Summary
There have been 4 major changes to the school district summary after the omission of scores of 9thgrade Thomas High students which are as follows.
- Thomas High now among the top 5 schools in the district coming in at 2nd place
- After omitting these students, the passing scores for math, reading & overall passing for Thomas High has increased to 93.19%, 97.01% & 90.63% respectively.
- The school size for Thomas High changes from 1635 to 994 which may cause the school to now be classified as a small school as opposed to medium
- Spending data is affected a little because of the omission sine now it appears as if all the allocation of funds has been made towards grades 10th, 11th & 12th
