# Bellabeat Project - case study for Google analytics certificate

## An analysis of FitBit fitness tracker data

This case study will involve generating insights for Bellabeat company using open-source data from FitBit fitness tracker.

Bellabeat produces stylish health and fitness tracking devices for women. The products are equipped with smart data collection features for sleep, activity, stress and reproductive health which provides women with knowledge about their health and habits.

## Ask

The task involves cleaning, manipulating and analysing data from FitBit users in order to identify trends and deliver recommendations to Bellabeat. Below are the proposed questions to use as guidance and reference in the project:

1. What are some trends in smart device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help influence Bellabeat marketing strategy?

## Prepare and Process

The data is open-source and can be accessed [here](https://www.kaggle.com/datasets/arashnic/fitbit).
This data set contains personal fitness data from 30 FitBit users. Thirty eligible Fitbit users consented to submitting
personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring. It includes
information about daily activity, steps, and heart rate that can be used to explore users’ habits.

Below is a brief information about the preparation of the data:
  
  - The data is stored on a local device in the folder "Bellabeat Project"
  - The data is organised in a long format
  - The data seems to be reliable, original, comprehensive and relatively current (all data is from 2016).
  - The data contains information about users' sleep, activity, etc. which could be useful in identifying patterns and trends.
  - Some data files are missing information which could pose a problem when joining tables together.

## Excel

The first document that would be examined and cleaned is "hourlySteps_merged". It contains information about users' number of steps at a particular hour.

<img width="342" alt="Screenshot 2024-12-02 at 19 56 40" src="https://github.com/user-attachments/assets/9bfee8eb-49fc-4b87-ba12-58f16b7ec374">

There are several issues with the file:
  - The "ActivityHour" column is stored in a string format instead of DateTime.
  - The "ActivityHour" column contains two parameters that could be split into two columns (date and time).
  - Some rows store time in 12-hour format while others in 24-hour format.
  - Each row must possess a unique combination of values therefore no duplicate rows should be present.

### Duplicate rows

The file needs to be checked for duplicate rows and, if found, remove them.

<img width="344" alt="Screenshot 2024-12-04 at 13 32 03" src="https://github.com/user-attachments/assets/4458ad5b-f88f-4f79-a56b-932a00b6627b">

After selecting "Data", "Advanced filter" and checking "Unique records only" the file still displayed all the records.

<img width="974" alt="Screenshot 2024-12-04 at 13 34 25" src="https://github.com/user-attachments/assets/4eed6914-5269-40c0-b42d-12cfb200fc6e">

To double-check that there are no duplicate rows, the "Data", "Table tools" and "Remove duplicates" were selected with the result presented above. 

Now that it is confirmed that the file does not contain any duplicate rows and all rows are unique, the next step in data cleaning could be performed.

### "Activity Hour" column.

The first step would be to convert the column values into the correct format. In the "Format cells" a custom type is selected for the whole column which is "mm/dd/yyyy hh:mm". The function seems to have no effect on the column. After testing multiple other types it is clear that only the rows with already correct cell type are affected. It could be possible that the function doesn't recognise cell values of rows where 12 hour is present.

The next step would be to try and split the column into two by selecting "Data", "Text to columns" and "Fixed width". Below is the result:

<img width="536" alt="Screenshot 2024-12-04 at 14 31 42" src="https://github.com/user-attachments/assets/f5c2ac2a-bb55-44dc-8a55-60bfb9e26416">

Then the format of the new columns needs to be changed. For the "ActivityDay" the format would be mm/dd/yyyy and for "ActivityHour" (old "ActivityHour column had a "reference" added to it) hh:mm.

<img width="536" alt="Screenshot 2024-12-04 at 14 38 13" src="https://github.com/user-attachments/assets/5425f050-b35a-4299-ad9a-d46ff81a96a3">

## SQL

Another file would be cleaned with SQL. Below is the original file viewed using TablePlus and queried by Select * From statement.

<img width="713" alt="Screenshot 2024-12-04 at 14 41 42" src="https://github.com/user-attachments/assets/bf1105fc-ec90-4c37-9d96-b55d2b6e18c2">

Several problems could be identified within the data in the document:

  - Each row should be unique and no duplicate rows should be present.
  - "SleepDay" column values should be in the correct format. The time value is the same and could be ignored for analysis.
  - Null values need to be checked as they can negatively impact analysis.

### Duplicate rows

The code below checks for duplicate rows and presents them in the output.

<img width="990" alt="Screenshot 2024-11-06 at 12 01 19" src="https://github.com/user-attachments/assets/82eb2f39-e61a-41b5-858d-fa5b979f01a1">

There are three rows that could be removed. 

A new table will be created with no duplicate rows to keep the original table as a reference. This is presented below:

<img width="990" alt="Screenshot 2024-11-07 at 14 55 00" src="https://github.com/user-attachments/assets/8d6105ad-95f0-4020-9f19-a5229b3b2c59">

When the table is queried it shows 410 rows instead of 413:

<img width="990" alt="Screenshot 2024-11-07 at 14 55 15" src="https://github.com/user-attachments/assets/28bdd1ce-793e-42d9-adfb-b7ebb437f727">

### Null values

The table should be checked for null values in every column:

<img width="990" alt="Screenshot 2024-11-07 at 15 01 05" src="https://github.com/user-attachments/assets/f69b820b-dc6e-4072-b8b6-16e451bc4c6c">

No Null values are found.

### "SleepDay" column

After checking the type of the "SleepDay" column it is clear that the data is stored as a variable character:

<img width="990" alt="Screenshot 2024-11-07 at 15 23 48" src="https://github.com/user-attachments/assets/10782609-2b29-431b-8e85-390658e9d98b">

A new column should be created called "SleepDate" which would store only the date of the record in a form yyyy-mm-dd. The code below adds new column to the table:

***ALTER TABLE sleepday_merged_updated
ADD SleepDate DATE***

The code below takes values from "Sleepday" column, converts them into a correct format and inserts them into the "SleepDate" column. (For some reason when inserting just the values from "Sleepday" into "SleepDate" the values for column "SleepDate" appear below the rest of the values of other columns. So by transfering all the columns' values ensures that all rows will have matching data).

<img width="990" alt="Screenshot 2024-11-08 at 17 20 47" src="https://github.com/user-attachments/assets/7a83ae73-47ff-4b60-8ad6-f8491677e6c8">

This generates another issue: the code added another 410 rows making the total row number of 820. The solution is to delete all rows which don't have data in the "SleepDate" column:

***DELETE FROM sleepday_merged_updated
WHERE SleepDate IS NULL***

The next step would be to identify what day of the week was a particular date. A WeekDay column was created to accomodate new values. This would be achieved by applying DAYNAME function to the "SleepDate" column:

<img width="990" alt="Screenshot 2024-11-13 at 17 17 34" src="https://github.com/user-attachments/assets/18f3303d-3bc8-4850-8ca9-7ad9c091e540">

The same issue arises with the data appearing below the table:

<img width="996" alt="Screenshot 2024-11-13 at 17 17 59" src="https://github.com/user-attachments/assets/2817e948-b700-45d0-a3bc-b68fd97b1d0e">

Another DELETE FROM WHERE is used to remove rows without values in WeekDay column.

### "TimeWaste"

Another column "TimeWaste" is added to the table which stores the difference between "TotalTimeInBed" and "TotalMinutesAsleep":

***ALTER TABLE sleepday_merged_updated
    ADD TimeWaste INT***

Then the values are inserted into the columns:

<img width="995" alt="Screenshot 2024-11-18 at 17 54 25" src="https://github.com/user-attachments/assets/1245bac3-427d-4a91-9b17-ad5abfc7f609">

After DELETE FROM WHERE, it is used to keep only the rows with values in the "TimeWaste" column. The final result is:

<img width="944" alt="Screenshot 2024-12-15 at 17 37 47" src="https://github.com/user-attachments/assets/1a7b5c26-0f39-4fdf-8e74-735f9e8a6355">


## Analyse and Share

This stage will involve performing an analysis of the cleaned files.

### Excel

The data could be sorted in a more meaningful manner in order to make analysis simpler, which would help make reliable conclusions. A pivot table will be used. 

Below is an image of the pivot table created:

<img width="1411" alt="Screenshot 2024-12-09 at 17 41 23" src="https://github.com/user-attachments/assets/a202776e-11e8-48e1-b01f-4fb25d3e5806">

The values highlighted in red are below average, and those highlighted in green are above average. The average number of steps taken in an hour is only 286.

The "ActivityHour" column was chosen for the "Row labels," and the "StepTotal" column was used for values in the form of an average number. This table shows the average number of steps taken by different users at what time of day.

For a better understanding of the table's data, a graph is generated as a visualisation in the form of a bar chart:

<img width="1019" alt="image" src="https://github.com/user-attachments/assets/5bbe10da-0b37-40fa-a414-86b5ae577698">

The line crossing the bars represents the average value of steps.
The graph shows that the most active hours of the day are from 08:00 to 20:00. During this 12-hour period, users accumulated the most steps. The total number of number of steps taken per day on average is about 6,864.

### Tableau

The data cleaned and organised in SQL will be used in Tableau to generate meaningful visualisation.

The bar chart below shows users' average time being asleep:

<img width="545" alt="Screenshot 2024-12-16 at 15 25 33" src="https://github.com/user-attachments/assets/bdfa951a-2f34-491a-aede-7f926a68735e">

The red represents being asleep for less than 420 minutes (less than 7 hours), and the green represents being asleep for more than 420 minutes.
Users get the most sleep on Wednesday and Sunday.

The bar chart below shows the average time of being in bed but not asleep (time waste):

<img width="545" alt="Screenshot 2024-12-16 at 15 51 44" src="https://github.com/user-attachments/assets/4f0c6ccb-7ddb-4f23-b07f-e6daf4a1417a">

The red bars represent a time waste of more than 35 minutes, while the green bars represent a time waste of less than 35 minutes. For the simplicity of the analysis, it would be assumed that users spend their time in bed before they go to sleep and not after.

According to Olson (2023), the healthy amount of sleep for adults (above 18 years old) is at least 7 hours or 420 minutes. 

When looking at the first graph it becomes clear that users do not usually get enough sleep as their sleep time falls below 420 minutes in most days of the week. Could the problem be linked to very busy schedules and users simply do not have time to lay in bed? Let's explore. 

Rausch-Phung and Rausch-Phung (2023) state that adults should not take more than 20 minutes to fall asleep, although some people may take longer or faster. The second graph shows that the users spend more than 35 minutes in bed before going to sleep which could be caused by several issues:

- Fighting the urge to fall asleep in order to stay on a laptop or phone longer
- Not feeling tired enough
- Having insomnia caused by anxiety, stress, etc.
- Having trouble falling asleep quickly
- Hormone levels change due to pregnancy

## Rstudio
The file "dailyIntensities_merged" was cleaned, modified and analysed using Rstudio.

### Packages and files upload

<img width="956" alt="Screenshot 2024-12-24 at 18 33 13" src="https://github.com/user-attachments/assets/d66d2d71-93a4-4c0b-9d1f-a6c7dbf6a554" />


### Duplicate rows

<img width="956" alt="Screenshot 2024-12-24 at 18 36 11" src="https://github.com/user-attachments/assets/fb840224-c9b6-49fa-8e0a-475b38f00873">

<img width="956" alt="Screenshot 2024-12-24 at 18 33 27" src="https://github.com/user-attachments/assets/99a2af46-e675-4337-aa11-88901c3c5632" />

<img width="956" alt="Screenshot 2024-12-24 at 18 34 12" src="https://github.com/user-attachments/assets/6c85032c-3816-449c-a098-83204ade8895" />

<img width="956" alt="Screenshot 2024-12-24 at 18 34 34" src="https://github.com/user-attachments/assets/3d97870a-a50b-4d74-98b9-0294e390eeb5" />

<img width="956" alt="Screenshot 2024-12-24 at 18 35 41" src="https://github.com/user-attachments/assets/fee6e88c-d363-4981-ad0d-0bbc0316d76f" />


### Null values



### Date column


# Act

## Recommendations

This section will focus on providing recommendations based on the analysis done in previous sections.

## Excel document (Daily steps)

According to Paluch et al., (2022), the healthy amount of steps per day for adults younger than 60 years is 8,000 - 10,000 and for adults older than 60 years is 6,000 - 8,000. It is assumed that the age of users participating in the Fitbit study is higher than 18 and lower than 60 years. 

The findings inform that users, on average, take 6,864 steps a day which falls short of the recommended number of 8,000 to 10,000. There are possible solutions to this problem:

- Provide opportunities for users to plan their steps intake with Bellabeat app. This will help them accommodate the necessary amount of steps into their busy schedules. Considering that the 12-hour period from 8:00 to 20:00 is the most active, users could take 600 steps an hour during that period (which would add up to 7,200 steps) and then split the rest 800 between less active hours.
- Bellabeat app, along with wearable devices could remind people to stay active and commit to their daily steps goal.
- Ensure that users are aware of the benefits of walking by providing medical articles about the topic. This will motivate and encourage users to commit to a healthier lifestyle. 


## Tableau document (Sleep)

The findings show that users, on average, get less sleep than required and spend more unnecessary time in bed than needed to fall asleep. Not getting enough sleep could negatively impact users' mood, appetite, mental health, and physical and mental performances. 

To fix the issue a few steps could be taken:

- Provide users with sufficient knowledge about sleep through Bellabeat app (scientific articles, researchers, etc.)
- Provide recommendations based on the data collected by the watch and Leaf devices to help develop better sleep habits and improve the quality of sleep
- Users should be able to plan their sleep schedules to fit their lifestyles and achieve higher sleep quality through Bellabeat app.
- Dietary suggestions could be offered to improve overall health and sleep.
- Users could receive notifications reminding them to not use their electronic devices at certain times
- Users could receive guidance on breathing exercises which would help them relax and fall asleep faster. This could be controlled by a wearable device which would alarm users when to inhale and exhale.



# References

Olson, E.J. (2023). How many hours of sleep do you need? [online] Mayo Clinic. Available at: https://www.mayoclinic.org/healthy-lifestyle/adult-health/expert-answers/how-many-hours-of-sleep-are-enough/faq-20057898.
‌
Rausch-Phung, E. and Rausch-Phung, E. (2023) How long should it take to fall asleep? https://www.sleepfoundation.org/sleep-faqs/how-long-should-it-take-to-fall-asleep#:~:text=Most%20adults%20with%20healthy%20sleep,sleep%20or%20a%20medical%20condition.

Paluch, A.E., Bajpai, S., Bassett, D.R., Carnethon, M.R., Ekelund, U., Evenson, K.R., Galuska, D.A., Jefferis, B.J., Kraus, W.E., Lee, I-Min., Matthews, C.E., Omura, J.D., Patel, A.V., Pieper, C.F., Rees-Punia, E., Dallmeier, D., Klenk, J., Whincup, P.H., Dooley, E.E. and Pettee Gabriel, K. (2022). Daily steps and all-cause mortality: a meta-analysis of 15 international cohorts. The Lancet Public Health, [online] 7(3), pp.e219–e228. doi:https://doi.org/10.1016/S2468-2667(21)00302-9.
‌
