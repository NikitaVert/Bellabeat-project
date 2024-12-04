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


