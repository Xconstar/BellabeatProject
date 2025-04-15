# Bellabeat Project
## How Can a Wellness Technology Company Play It Smart? 

### A Google Data Analytics Professional Certificate Capstone Project
[Bellabeat](https://bellabeat.com/) was co-founded by Urška Sršen and Sando Mur in 2013. Bellabeat is a high-tech company that creates health-focused smart devices and fitness products. Some of their products include their renowned [Ivy Health Tracker](https://bellabeat.com/product/ivy/) bracelet, [Yoga Mats](https://bellabeat.com/product/yoga-mat/), and their [Leaf Urban](https://bellabeat.com/product/leaf-urban/). You can find a full list of their products on [their website](https://bellabeat.com/)

Bellabeat's co-founders Urška Sršen and Sando Mur want to analyze smart device data to gain insight into how consumers are using their smart devices. Urška believes that analyzing smart device fitness data could help unlock new growth opportunities for the company. Bellabeat hopes that these new findings can help guide marketing strategy for the company.

## ASK

### Business Task

Analyze smart device usage data in order to gain insight into how consumers use non-Bellabeat smart devices using those newfound insights and one Bellabeat device help guide the marketing strategies at Bellabeat.

### Key Stakeholders
- **Urška Sršen:** Bellabeat's co-founder and Chief Creative Officer
- **Sando Mur:** Mathematician and Bellabeat's cofounder; a key member of the Bellabeat executive team
- **Bellabeat marketing analytics team:** A team of data analysts responsible for collecting, analyzing, and reporting data that helps guide Bellabeat's marketing strategy.

## PREPARE

### Data Source

The data was sourced from an upload named "FitBit Fitness Tracker Data" on [Kaggle](https://www.kaggle.com/datasets/arashnic/fitbithttps://www.kaggle.com/datasets/arashnic/fitbit) consisting of 18 CSV files. The dataset generated by thirty respondents to a distributed survey via Amazon Mechanical Turk. The data was collected between March, 12th 2016 - May, 12th 2016. Thirty eligible Fitbit users submitted personal tracker data, including minute-level output for physical activity, heart rate, and sleep monitoring.

### Data Factors and Limitations

- **Data Collection:** The way the data was collected has to be taken into consideration the reason being the data was collected through a survey. This can cause the data to be less accurate for the fact that respondents might not given fully honest or accurate responses.
- **Sample Size:** Being there are only thirty respondents this sample size would not give us as strong correlations or trends as a larger sample size.
- **Data Age:** FitBit Fitness Tracker Data was collected 7 years ago and updated 3 years ago. The new FitBit tracker devices may be more accurate and reliable.

## PROCESS

### Data Files

For the processing step, I've chosen three of the data files that were in the dataset. The reason is these three datasets: `dailyActivity_merged.csv`, `sleepDay_merged.csv`, and `weightLogInfo_merged` all provide the majority of the data and have a broad look at the data.

### Tools

In the process phase, knowing this is a small dataset I used **Excel** to get an overview of the data and to make simple changes like the column data type conversions or fix any spelling mistakes. Next, I will use **SQL** for the necessary cleaning steps and to transform the data. Last, for the data analysis and visualisation I will use **Tableau** and **R Studio**.

### Transformation

**SQL Steps**:
1. Check for duplicates in each dataset. 
```
SELECT
ActivityDate, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories
FROM courseracourse4week3-392618.Bellabeat.dailyActivity_data_2
GROUP BY 
ActivityDate, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories
HAVING COUNT(*) > 1;
```
```
SELECT 
  Id, Date, WeightKg, WeightPounds, BMI, IsManualReport, LogId
FROM 
  `courseracourse4week3-392618.Bellabeat.weightLogInfo_data`
GROUP BY 
  Id, Date, WeightKg, WeightPounds, BMI, IsManualReport, LogId
HAVING COUNT(*) > 1;

```
3. Make sure the "Date" column in each dataset are in the correct data type.
```
SELECT
  FORMAT_DATE('%d-%m-%Y', ActivityDate ) AS formatted_date
FROM
`courseracourse4week3-392618.Bellabeat.dailyActivity_data`;
```
```
SELECT
  FORMAT_DATE('%Y-%m-%d', Date) AS formatted_date
FROM
  `courseracourse4week3-392618.Bellabeat.weightLogInfo_data`;
```
```
SELECT
  FORMAT_DATE('%Y-%m-%d', SleepDay) AS formatted_date
FROM
  `courseracourse4week3-392618.Bellabeat.sleepDay_data`;
```
4. Change the names of the date columns in each table to "Date"
5. Left join by Id
```
SELECT dailyActivity.Id, ActivityDate, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, SleepDay, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed, Date,WeightKg, WeightPounds, BMI, IsManualReport, LogId
FROM `courseracourse4week3-392618.Bellabeat.dailyActivity_data` as dailyActivity
LEFT OUTER JOIN `courseracourse4week3-392618.Bellabeat.sleepDay_data` AS test
ON dailyActivity.Id = test.Id
AND dailyActivity.activityDate = test.SleepDay
LEFT OUTER JOIN `courseracourse4week3-392618.Bellabeat.weightLogInfo_data` as weight
ON test.Id = weight.Id
and test.SleepDay = weight.Date
GROUP BY dailyActivity.Id, ActivityDate, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, SleepDay, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed, Date,WeightKg, WeightPounds, BMI, IsManualReport, LogId
```
6. Left Join by Date
```
SELECT Id, TempTable.Date, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed, WeightKg, WeightPounds, BMI, IsManualReport, LogId
FROM `courseracourse4week3-392618.Bellabeat.temp_table` as TempTable
LEFT JOIN `courseracourse4week3-392618.Bellabeat.SleepDay` AS Sleep
ON TempTable.Date = Sleep.Date
AND TempTable.Date = Sleep.Date
LEFT JOIN `courseracourse4week3-392618.Bellabeat.Date_1` as Date1
ON Sleep.Date = Date1.Date
and Sleep.Date = Date1.Date
GROUP BY Id, TempTable.Date, TotalSteps, TotalDistance, TrackerDistance, LoggedActivitiesDistance, VeryActiveDistance, ModeratelyActiveDistance, LightActiveDistance, SedentaryActiveDistance, VeryActiveMinutes, FairlyActiveMinutes, LightlyActiveMinutes, SedentaryMinutes, Calories, TotalSleepRecords, TotalMinutesAsleep, TotalTimeInBed, WeightKg, WeightPounds, BMI, IsManualReport, LogId
```
7. Remove duplicates

## ANALYZE

### Visualization
***Calories Burned Per Step***
![image](https://github.com/Xconstar/BellabeatProject/blob/main/Calories%20burned%20per%20step.png)
In this visualization we can see that when a user takes steps they burn calories. Meaning the more steps taken the more calories burned, clearly.
```
ggplot(data=Fitness_Data) + 
  geom_point(mapping=aes(x=TotalSteps, y=Calories, color=Calories)) +
  geom_hline(mapping = aes(yintercept=mean_calories), color="blue", lwd=1.0) +
  geom_vline(mapping = aes(xintercept=mean_steps), color="darkred", lwd=1.0) +
  geom_text(mapping = aes(x=10000, y=500, label="Average Steps", srt=-90)) +
  geom_text(mapping = aes(x=29000, y=2500, label="Average Calories")) +
  labs(x="Steps Taken", y="Calories Burned", title = "Calories burned per step") +
  scale_color_gradient(low = "darkgreen", high = "lightgreen")
```
***Very Active Minutes Vs Calories***
![image](https://github.com/Xconstar/BellabeatProject/blob/main/VActiveMinVsCal.png)
Here we see another strong correlation this time between very active minutes and calories. The more activity a user engages in the more calories they will burn.
```
ggplot(data=Fitness_Data, aes(x=VeryActiveMinutes, y=Calories, color = Calories)) + geom_point() +
   geom_smooth(method = "loess",color="blue") + 
   labs(x="Very Active Minutes",y="calories",title="Very Active Minutes vs Calories") +
   scale_color_gradient(low = "#144F19", high = "#26D835")
```
***Steps By Day***
![image](https://github.com/Xconstar/BellabeatProject/blob/main/StepsByDay.png)
Here we are looking at the Distribution of steps by day and as we can see here the two most notable decrease in steps are Sunday and Monday.

***Sedentary Minutes Vs Time Asleep***
![image](https://github.com/Xconstar/BellabeatProject/blob/main/Sedentary%20Minutes%20Vs%20Time%20Asleep.png)
This is an interesting chart in my opinion. It is showing a negative correlation between sedentary minutes and time asleep. Meaning the more inactive a user is the less sleep they will get.
```
ggplot(data=Fitness_Data, aes(x=SedentaryMinutes, y=TotalMinutesAsleep)) + geom_point(color = '#0F8E19') + stat_smooth(method = lm) +
  labs(x="Sedentary Minutes", y="Total Minutes Asleep", title = "Sedentary Minutes Vs Time Asleep")

```
## ACT

- Knowing that the more steps a user takes the more calories a user will burn, we can use this as an oppurtunity to have Bellabeat Fitness Tracker encourage users to have step goals. Where a user can have a goal weight or if a user simply wants to get more active and have the fitness tracker count the steps and how many are left to reach the goal. To motivate the users the fitness tracker can send users notifications letting them know how many steps there are left until they reach their desired goal.

- Piggybacking off the previous recommendation. Since the minutes a user is active also has a strong correlation on how many calories a user burns, Bellabeat can let users add a active minute goal. This can be with heart rate monitoring, users can be reminded on their goals throughout the day. This can also be an alternative if the user would rather be doing something else than walking or running and have their calories burned tracked by active minutes through any activity of their choosing.

- Through my analysis users tend to take less steps on Sunday and Monday. Wether this may be because of work or the anticipation of have to work and would rather relax. What Bellabeat could do is try to encourage users to keep up their streak and goal. This can be through reminder notifications.

- Lastly, knowing that being less active can affect sleep in a negative way, Bellabeat can incorporate ways to show that with their fitness tracker, users may see improvements in their sleep. Bellabeat can also try to implement a default reminder for all users to have less around 800 sedentary minutes.


