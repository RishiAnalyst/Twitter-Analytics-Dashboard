# Twitter-Analytics-Dashboard
## Introduction
The Twitter Analytics Dashboard Project was undertaken to provide actionable insights into Twitter engagement metrics, helping to analyse trends, patterns, and correlations within Twitter data. The project involved extensive data preprocessing, cleaning, and visualization tasks using Power BI, DAX, and Power Query, emphasizing the importance of real-time filtering and interactivity in data analysis.
This report details the processes, tools, challenges, and outcomes associated with the development of the dashboard, highlighting key learnings and skill advancements achieved during the project.

## Background
Twitter is one of the most popular platforms for digital interaction, with vast amounts of user-generated content. Organizations often rely on Twitter analytics to evaluate their digital marketing strategies, user engagement, and brand visibility.
The project aimed to analyse several key performance indicators (KPIs) such as impressions, clicks, retweets, likes, replies, and engagement rates, catering to the following objectives:
•	Gain insights into how users interact with tweets.
•	Identify high-performing tweets based on engagement metrics.
•	Evaluate the impact of media, hashtags, and time of posting on engagement.
By visualizing these metrics through interactive dashboards, the project provides valuable data-driven recommendations to optimize Twitter strategies.












 
## Learning Objectives
### The key learning goals for this project were:
1.	Data Cleaning and Transformation
   -	Removing unnecessary columns and standardizing data formats.
   -	Extracting meaningful features from raw data using Power Query and DAX.
2.	Visualization Skills
- Crafting impactful visualizations like pie charts, scatter plots, clustered bar charts, and line graphs.
-	Implementing drill-down features for enhanced interactivity.
3.	Advanced Filtering Mechanisms
-	Time-based filters (e.g., even/odd dates, specific time intervals).
-	Numerical and categorical filters for impressions, clicks, etc.
4.	Problem-Solving Through Iterative Development
-	Overcoming challenges with data filtering, DAX expressions, and chart creation.
-	Debugging issues through trial-and-error and external resources.

 
## Activities and Tasks
### 1. Data Extraction and Cleaning
**Activities:**
-	Imported Twitter data containing columns such as impressions, clicks, likes, retweets, and more from data world.
-	Removed unwanted columns -  ‘ dial phone’, ‘email tweets’, ‘permalink clicks’, ‘follows’, ‘app installs’.
-	Converted date time which was in ‘text’ to ‘date time’ using M language
```M
=DateTime.FromText([time])
```
-	Extracted additional features:	Year, Month, Weekday/Weekend Indicators: Useful for time-based analysis.

**Tools Used:**
-	Power Query Editor: Applied transformations, splits, and extractions.
-	DAX Calculations: Enhanced data with derived metrics.
________________________________________
### 2. Task 1: Pie Chart –
Build a pie chart that represents the proportion of total clicks (URL clicks, user profile clicks, and hashtag clicks) for tweets with more than 500 impressions. Include a drill-down to view the specific types of clicks for each tweet           

**Objective:**
Visualize the distribution of click types (URL clicks, profile clicks, hashtag clicks) for tweets with impressions >500, with a drill-down feature to view tweet-level data.

**Steps:**
-	have created a new table “Clicks(>500)” with only 3 types of clicks and In which data of tweets with more than 500 impressions.
```dax
clicks(>500) = SELECTCOLUMNS(filter(SocialMedia,SocialMedia[impressions]>500),"tweets",SocialMedia[Tweet],"URL clicks",SocialMedia[url clicks],"user profile clicks",SocialMedia[user profile clicks],"hashtag clicks",SocialMedia[hashtag clicks],"impressions",SocialMedia[impressions])
```
-	Unpivoted click columns: Transformed columns for each click type into a single "Click Type" column using UNION for better visualization.
-	Created a pie chart to display proportions and added a drill-down to specific tweets contributing to each category.

**Challenges & Solutions:**
-	Filter down data for tweets with more than 500 impressions:  I used select column DAX function and inside that filter function.

-	Drill-Down Implementation:  From the table I have created Earlier created one more table with union and select columns to get a result as unpivoting table with tweets click type and click. Then I have taken the pie chart, dragged the “click type” in the legend and below that the “tweets” and “clicks” to the values.

```dax
‘ Click Types Table = 
UNION (
    SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[tweets], "Click Type", "URL clicks","Clicks", 'clicks(>500)'[URL clicks]),
    SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[Tweets], "Click Type", "User Profile Clicks", "Clicks", 'clicks(>500)'[user profile clicks]),
    SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[Tweets], "Click Type", "Hashtag Clicks", "Clicks", 'clicks(>500)'[hashtag clicks])
) ‘
```
![Screenshot (353)](https://github.com/user-attachments/assets/4e7b6925-f590-461f-b4ba-f13f54e25fc6)
  
### 3. Task 2: 
Column Chart – Build a chart to identify the top 10 tweets by the sum of retweets and likes. Filter out tweets posted on weekends and show the user profile that posted each tweet and this graph should work between 3 PM to 6 PM and the tweet impression should be even number and tweet date should be odd number as well as tweet word count be below 30

#### Objective:
Identify and visualize the top 10 tweets based on the sum of likes and retweets, filtered by specific conditions like time and word count.

#### Steps:
-	Created a measure to calculate the combined likes and retweets for each tweet.
-	created 5 new column for filtering down for the need of the task, such as :
  1.	column with sum of likes and retweets
  2.	column with tweet on “weekday” or ”weekend”
  3.	column with tweet impression is “even” or “odd”
  4.	column with tweet date “even” or “odd”
  5.	column with count or words of tweet
-	Created a column chart taking “tweets” in the x axis and the new column “sum of like and retweets” in the y axis, and for the details of the tweets id took “id” column in the legends.
-	Then I have put all these in the chart’s filter pane and set Top 10 by “sum of retweet & like” in the top N option.

#### Other requirement in the chart (Task):
#### 1.	This graph should work between 3 PM to 6 PM
For this created a measure for current time 
```dax
‘ nowhour = HOUR(NOW()) ’
```
 Then created another measure 
 ```dax
3pm-6pm = IF(AND([nowhour]>=15,[nowhour]<18),1,0)
 ```
Added this measure to the filter pane and gave value as 1.

#### 2. The tweet impression should be even number
For this created a new measure using DAX
```dax
odd/even impression = IF(ISEVEN(SocialMedia[impressions])=True,"even","odd")
```
then added this in the filter pane and gave value as “even” in basic filtering option

#### 3.Tweet date should be odd number
For this created a new measure using DAX
```dax
odd/even date = IF(ISEVEN(SocialMedia[date].[Day])=true,"even","odd")
```
then added this in the filter pane and gave value as “odd” in basic filtering option

#### 4. Tweet word count be below 30
For this created a new measure using DAX
```dax
tweetwordcount = LEN(SocialMedia[Tweet])-LEN(SUBSTITUTE(SocialMedia[Tweet]," ",""))+1
```
then added this measure in the filter pane and set ‘is less than’ 30 in advance filtering option.

#### Challenges:
-	To enable the chart to show only between time 3pm and 6 pm: used the help of Youtube and Google, I have created 2 measures with DAX functions for “current hour” and “time between”, then I have added the “time between” measure in the filter pane of chart and set the filter.

![Screenshot (354)](https://github.com/user-attachments/assets/4c3fa4ee-8d67-4c6e-827b-abc2e486a10d)
 
### 4. Task 3: 
Scatter Plot – Plot a scatter chart to analyse the relationship between media engagements and media views for tweets that received more than 10 replies. Highlight tweets with an engagement rate above 5% and this graph should work only between 12 PM to 6 PM and the tweet date should be odd number as well as tweet word count be below 50.

### Objective:
Analyse the relationship between media engagements and media views for tweets with more than 10 replies, highlighting tweets with an engagement rate >5%.

#### Steps:
-	I have taken the scatter chart, then added media engagement in the x axis and set don’t summarize also taken “media views” in the y axis and set don’t summarize.

- For this task I have created 2 new columns
#### 1. “>10 replies” for knowing if the tweet has more than 10 replies
 ```dax
  >10 replies = IF(SocialMedia[replies]>10,"Yes","No")
 ```
Added this in the filter and given value ‘Yes’

#### 2. “>5% engagement rate” for knowing if the tweet has more than 5% engagement rate
 ```dax
   >5% engagmnt rate = IF(SocialMedia[engagement rate]>0.05,1,0)
 ```
Added this in the filter and given value ‘1’.

- Then I have made one new measurement
 “12 pm to 6 pm”  for working only in that time frame
 ```dax
12pm-6pm = IF(AND([nowhour]>=12,[nowhour]<18),1,0)
 ```
- For odd tweet date and below 50 words count columns were created in the previous task.

#### Insights:
-	Tweets with media had higher engagement rates compared to those without.

![Screenshot (355)](https://github.com/user-attachments/assets/a8f5c6f0-6eb5-4738-8652-46e2ebcca24c)

 
### 5. Task 4: 
Clustered Bar Chart Create a clustered bar chart that breaks down the sum of URL clicks, user profile clicks, and hashtag clicks by tweet category (e.g., tweets with media, tweets with links, tweets with hashtags). Only include tweets that have at least one of these interaction types and this graph should work between 3 PM to 6 PM and the tweet date should be even number as well as tweet word count be below 40.

#### Objective:
Categorize tweets into eight interaction types and visualize the sum of clicks (URL, profile, hashtag) for each category.

#### Steps:
-	made total 8 categories based on the different type of clicks media views and engagement of tweet for the newly creating column such as
   1.	Tweet with media, link & hashtags
   2.	Tweet with media & Link
   3.	Tweet with media & hashtags
   4.	Tweet with Link & Hashtags
   5.	Tweet with Only media
   6.	Tweet with only Link
   7.	Tweet with only hashtags
   8.	Tweet with Only profile
-	Created new column with DAX function, I used “switch” function consist of “true” function instead of “if“ so that result is more accurate and fast.
```dax
Tweet Category =
SWITCH(TRUE(),
(SocialMedia[media views] > 0 || [media engagements] > 0) && [url clicks] > 0 && [hashtagclicks] > 0, "Tweet with Media, Link & Hashtag",
([Media Views] > 0 || [media engagements] > 0) && [URL Clicks] > 0, "Tweet with Media & Link",
([media views] > 0 || [media engagements] > 0) && [Hashtag Clicks] > 0, "Tweet with Media & Hashtag",
[URL Clicks] > 0 && [hashtag clicks] > 0, "Tweet with Link & Hashtag",
[Media Views] > 0 || [media engagements] > 0, "Tweet with only Media",
[URL Clicks] > 0, "Tweet with only Link",
[Hashtag Clicks] > 0, "Tweet with only Hashtag",
SocialMedia[user profile clicks] > 0,"Tweet with only Profile",
"Other"
)
```

-	Taken a clustered bar chart and put the category column in the y axis and 3 clicks columns in the x axis

#### Other requirements in the task:
#### 1.	This graph should work between 3 PM to 6 PM.
#### 2.	The tweet date should be even number.
#### 3.	Tweet word count be below 40.
All the needed measures are created in the previous tasks and then added in the filter pane.

#### Challenges:
-	creating conditional columns for categories, and how to differentiate categories.

![Screenshot (356)](https://github.com/user-attachments/assets/904a92be-8a2c-46cd-9b6a-8ae04ce3e6f6)
 
### 6.Task 5:
Line Chart – Create a line chart showing the trend of the average engagement rate over each month of the year. Separate the lines for tweets with media content and those without and this graph should work between 3 PM to 6 PM and the tweet engagement should be even number and tweet date should be odd number

#### Objective:
Display monthly trends in engagement rates, differentiating tweets with and without media content.

#### Steps:
-	Created a new column “tweet media category” in which only two values are there ‘tweet with media’ and ‘tweet without media’ with the reference of the previously created column “tweet category” in the previous task. Used If function in the DAX code.

```dax
  tweet media category = IF(SocialMedia[Tweet Category]="Tweet with Media & Link" ||
                          SocialMedia[Tweet Category]="Tweet with Media & Hashtag"||
                          SocialMedia[Tweet Category]="Tweet with Media, Link & Hashtag"||
                          SocialMedia[Tweet Category]="Tweet with only Media","Tweet with Media","Tweet without Media")
```

-	Created the line chart and took months in the x axis and average engagement rate in the y axis, then added newly created column (tweet media category) in the legend, then it became two lines.

#### Other requirements in the task:
#### 1.  The tweet engagement should be even number 
For this created a new measure using DAX
```dax 
odd/even engagement = IF(ISEVEN(SocialMedia[engagements])=TRUE,"Even", "Odd")
```
Added this in the filter pane and set value as “even” in basic filtering option.
#### 2. this graph should work between 3 PM to 6 PM
#### 3. tweet date should be odd number
Other 2 measures required for adding in the filter pane are already created in the previous task.

#### Challenges:
-	DAX function for making new column: Tried chat GPT, done with if function.

![Screenshot (357)](https://github.com/user-attachments/assets/7cb19ddd-5c66-4066-ab80-8271b4d4663e)

### 7. Task 6: 
Engagement Rate Comparison -- Analyse tweets to show a comparison of the engagement rate for tweets with app opens versus tweets without app opens. Include only tweets posted between 9 AM and 5 PM on weekdays and this graph should work between 12 PM to 6 PM and the tweet impression should be even number and tweet date should be odd number as well as tweet word count be below 40.

#### Objective:
Compare engagement rates for tweets with and without app opens.

#### Steps:
-	Added a column “Tweet with app open/close” indicating tweets involving app opens.
```dax
‘ tweet with App open/close = 
IF(
    SocialMedia[app opens]>0,
    "Tweet with App Opens",
    "Tweet without App Opens"
) ’
```
-	Built a column chart for side-by-side comparison of engagement rates. Inserted this custom column to the x axis and engagement rate to the Y axis, and custom column to the legend.
-	To Include only tweets posted between 9 AM and 5 PM, created a new measure “tweet 9-5”
```dax
‘ tweet 9-5 = IF(AND(HOUR(SocialMedia[time])>=9,HOUR(SocialMedia[time])<=17),1,0) ’
```
and added in the filter and gave value as 1.

#### Other requirements in the task: 
#### 1. This graph should work between 12 PM to 6 PM 
#### 2. Tweet impression should be even number 
#### 3. Tweet date should be odd number as well as tweet word count be below 40.
All the needed measures for these requirements are created in the previous tasks and then added in the filter pane.

![Screenshot (358)](https://github.com/user-attachments/assets/8961890e-fb4f-4f12-960a-2be6f3ed9a5d)

 
### 8. Task 7: 
Dual-axis chart -- Create a dual-axis chart that shows the number of media views and media engagements by the day of the week for the last quarter. Highlight days with significant spikes in media interactions. this graph should work between 3 PM to 6 PM and the tweet impression should be even number and tweet date should be odd number as well as tweet word count be below 30.

#### Objective:
The purpose of this task is to create a dual-axis chart to visualize media views and media engagements across the days of the week for the last quarter, specifically under certain conditions. This chart will provide insights into the effectiveness of media content, identify days with significant spikes in media interactions, and help refine content strategies.

#### Steps:
-	Taken the line chart and dragged the “media views” column to the y axis and “media engagement” column to the secondary y axis and “day of the week” part to the x axis.
-	Dragged the quarter field to the filter pane and selected only 4th quarter as per condition of the task.

#### Other requirements in the task: 
#### 1. This graph should work between 3 PM to 6 PM 
#### 2. Tweet impression should be even number 
#### 3. Tweet date should be odd number as well as tweet word count be below 30.
All the needed measures for these requirements are created in the previous tasks and then added in the filter pane.

 
![Screenshot (359)](https://github.com/user-attachments/assets/6df023e1-0670-4c29-a4e6-fd7fbe4e3ac5)

### Skills and Competencies Developed
#### 1.	Technical Expertise:
   -	Mastery in Power BI visualization and DAX functions.
   -	Advanced filtering techniques for specific conditions.
#### 2.	Data Storytelling:
   -	Conveying insights effectively through visualizations and reports.
#### 3.	Problem-Solving:
   -	Tackling complex data transformations and filtering scenarios.

### Feedback and Evidence
-	Received positive feedback on the dashboard's interactivity and insightfulness.
-	Received appreciation for delivering a comprehensive and visually appealing analysis that effectively answered the problem statements
-	Included screenshots of all the charts that created.

 
### Challenges and Solutions

#### Task 1:
-	 Filter down data for tweets with more than 500 impressions( Task 1):  I used select column DAX function and inside that filter function.
-	Drill-Down Implementation:  From the table I have created Earlier created one more table with union and select columns to get a result as unpivoting table with tweets click type and click. Then I have taken the pie chart, dragged the “click type” in the legend and below that the “tweets” and “clicks” to the values.
```dax
‘ Click Types Table = 
UNION (
SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[tweets], "Click Type", "URL clicks","Clicks", 'clicks(>500)'[URL clicks]),
SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[Tweets], "Click Type", "User Profile Clicks", "Clicks", 'clicks(>500)'[user profile clicks]),
SELECTCOLUMNS('clicks(>500)', "Tweets", 'clicks(>500)'[Tweets], "Click Type", "Hashtag Clicks", "Clicks", 'clicks(>500)'[hashtag clicks])
) ‘
```

#### Task 2:
-	To enable the chart to show only between time 3pm and 6 pm: used the help of Youtube and Google, I have created 2 measures with DAX functions for “current hour” and “time between”, then I have added the “time between” measure in the filter pane of chart and set the filter.

#### Task 4:
-	creating conditional columns for categories, and how to differentiate categories: used “switch” function consist of “true” function instead of “if“ so that result is more accurate and fast.

#### Task 5:
-	DAX function for making new column: Tried chat GPT, done with if function.

#### Outcomes:
-	Resolved all challenges effectively, ensuring a functional and impactful dashboard.


 
### Outcomes and Impact
  -	Done all the tasks and subtasks inside it, with the use of Power Bi and DAX
-	Successfully built an advanced and interactive Twitter Analytics Dashboard.
-	Enhanced proficiency in Power BI, including advanced chart creation, DAX calculations, and interactive features like drill-downs and dual-axis visuals.
-	Provided actionable insights, such as identifying optimal posting times, tweet types driving higher engagement, and patterns in media interactions, enabling data-driven decisions.
-	Improved ability to handle complex data filtering and implement advanced visual storytelling techniques.

### Final Dashboard

![Screenshot (361)](https://github.com/user-attachments/assets/a175dff9-b588-49fa-aa23-6b22b5dec99a)
 
### Conclusion
The Twitter Analytics Dashboard project was a valuable learning experience that combined technical skills with creative problem-solving. Overcoming challenges along the way helped me grow and resulted in a professional dashboard with real-world applications in social media analytics. The project showed how powerful data visualization and analytics can be in understanding Twitter engagement trends. By answering key questions like engagement rates, top-performing tweets, and interaction breakdowns, the dashboard offered practical insights to improve social media strategies. Using advanced filtering and interactive features made the dashboard easy to use and explore, creating a strong base for future analytics work.
