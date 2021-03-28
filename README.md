# School District Analysis
## Overview of the School District Analysis:
The school board has notified Maria and her supervisor about the evidence of academic dishonesty; specifically, reading and math grades for Thomas High School ninth graders. Although the school board does not know the full extent of the academic dishonesty, they want to uphold state-testing standards and have turned to Maria for help. Maria has asked to replace the math and reading scores for 9th Graders in Thomas High School to be omitted while keeping the rest of the data intact. Once this function is complete, Maria has asked to repeat the school district analysis that you did and write up a report to describe how these changes affected the overall analysis.

## Results:
### How is the district summary affected?
By omitting grade 9th student data from the school district summary, the change in results was very insignificant. A total of 461 student’s maths & reading scores were attributed to Grade 9th Thomas High School students out of a total of 39,170 students in the whole district. This number is 1.1% and hence has not made a significant impact on the school district data.
```python
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
