# Bellabeat Project - case study for Google analytics certificate

## An analysis of FitBit fitness tracker data

This case study will involve generating insights for Bellabeat company using open-source data from FitBit fitness tracker.

Bellabeat produces stylish health and fitness tracking devices for women. The products are equipped with smart data collection features for sleep, activity, stress and reproductive health which provides women with knowledge about their health and habits.

## Ask

The task involves cleaning, manipulating and analysing data from FitBit users in order to identify trends and deliver recommendations to Bellabeat. Below are the proposed questions to use as a guidance and reference in the project:

1. What are some trends in sma  device usage?
2. How could these trends apply to Bellabeat customers?
3. How could these trends help in uence Bellabeat marketing strategy?

## Prepare

The data is open-source and can be accessed [here](https://www.kaggle.com/datasets/arashnic/fitbit).
This data set contains personal fitness data from 30 FitBit users. Thi rty eligible Fitbit users consented to the submission of
personal tracker data, including minute-level output for physical activity, hea  rate, and sleep monitoring. It includes
information about daily activity, steps, and hea  rate that can be used to explore users’ habits.

Below is a brief information about the preparation of the data:
  
  - The data is stored on a local device in folder "Bellabeat Project"
  - The data is organised in a long format
  - The data seems to be reliable, original, comprehensive and relatively current (all data is from 2016).
  - The data contains information about users' sleep, activity, etc. which could be useful in identifying patterns and trends.
  - Some data files are missing information which could pose a problem when joining tables together.

## Excel

The first document that would be examined and cleaned would be "hourlySteps_merged". It contains information about users' number of steps at a particular hour.

<img width="342" alt="Screenshot 2024-12-02 at 19 56 40" src="https://github.com/user-attachments/assets/9bfee8eb-49fc-4b87-ba12-58f16b7ec374">

There are several issues with the file:
  - The "ActivityHour" column is stored in a string format instead of DateTime.
  - The "ActivityHour" column contains two parameters that could be split into two columns (date and time).
  - Some rows store time in 12 hour format while other in 24 hour format.
  - Each row must possess unique combination of values therefore no duplicate rows should be present.

### Duplicate rows

The file needs to be checked for duplicate rows and, if found, remove them.

<img width="344" alt="Screenshot 2024-12-04 at 13 32 03" src="https://github.com/user-attachments/assets/4458ad5b-f88f-4f79-a56b-932a00b6627b">

After selecting "Data", "Advanced filter" and checking "Unique records only" the file still displayed all the records.

<img width="974" alt="Screenshot 2024-12-04 at 13 34 25" src="https://github.com/user-attachments/assets/4eed6914-5269-40c0-b42d-12cfb200fc6e">

To double check that there are no duplicate rows, the "Data", "Table tools" and "Remove duplicates" were selected with the result presented above. 

Now that it is confirmed that the file does not contain any duplicate rows and all rows are unique, next step in data cleaning could be performed.

### "Activity Hour" column.

The first step would be to convert the column values into the correct format. In the "Format cells" a custom type is selected for the whole column which is "mm/dd/yyyy hh:mm". The function seems to have no effect on the column. After testing multiple other types it is clear that only the rows with already correct cell type are affected. It could be possible that the function don't recognise cell values of rows where 12 hour is present.

Next step would be to try an split the column into two by selecting "Data", "Text to columns" and "Fixed width". Below is the result:

<img width="536" alt="Screenshot 2024-12-04 at 14 31 42" src="https://github.com/user-attachments/assets/f5c2ac2a-bb55-44dc-8a55-60bfb9e26416">

Then the format of the new columns needs to be changed. For the "ActivityDay" the format would be mm/dd/yyyy and for "ActivityHour" (old "ActivityHour column had a "reference" added to it) hh:mm.

<img width="536" alt="Screenshot 2024-12-04 at 14 38 13" src="https://github.com/user-attachments/assets/5425f050-b35a-4299-ad9a-d46ff81a96a3">

## SQL

Another file would be cleaned with SQL. Below is the origial file viewed using TablePlus and queried by Select * From statement.

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
ADD SleepDate***

The code below takes values from "Sleepday" column, converts them into a correct format and inserts them into the "SleepDate" column. (For some reason when inserting just the values from "Sleepday" into "SleepDate" the values for column "SleepDate" appear below the rest of the values of other columns. So by transfering all the columns' values ensures that all rows will have matching data).

<img width="990" alt="Screenshot 2024-11-08 at 17 20 47" src="https://github.com/user-attachments/assets/7a83ae73-47ff-4b60-8ad6-f8491677e6c8">

This generates another issue: the code added another 410 rows making the total row number of 820. The solution is to delete all rows which don't have data in the "SleepDate" column:

***DELETE FROM sleepday_merged_updated
WHERE SleepDate IS NULL***

Next step would be to identify what day of the week was a parcticular date. A WeekDay column was created to accomodate new values. This would be achieved by applying DAYNAME function to the "SleepDate" column:

<img width="990" alt="Screenshot 2024-11-13 at 17 17 34" src="https://github.com/user-attachments/assets/18f3303d-3bc8-4850-8ca9-7ad9c091e540">

The same issue arises with the data apearing below the table. 
<img width="996" alt="Screenshot 2024-11-13 at 17 17 59" src="https://github.com/user-attachments/assets/2817e948-b700-45d0-a3bc-b68fd97b1d0e">








