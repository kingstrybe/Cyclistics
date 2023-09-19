# Google Data Analytics Capstone: Cyclistic Case Study
![divvy bikes](https://github.com/kingstrybe/Google-Data-Analytics-Certificate-Course-Capstone-/assets/124252840/630a1030-b4c7-460b-8b22-3eb331c364e3)
## About the company

In 2016, Cyclistic launched a successful bike-share offering, which currently holds more than 5,000 geo-tracked bicycles in a network of 692 stations across Chicago. The bicycles are unlockable in their respective station and can be returned to any other station in the system time.

Until now, Cyclistic’s marketing strategy relied on building general awareness and appealing to broad consumer segments. For example, the flexibility of its pricing plans is as follows: single-ride passes, full-day passes, and annual memberships. 
Customers who purchase single-ride or full-day passes are referred to as casual riders. Customers who purchase annual memberships are Cyclistic members.

## Scenario

Cyclistic is a fictional bike-sharing company located in Chicago. It operates a fleet of more than 5,800 bicycles which can be accessed from over 600 docking stations across the city. Bikes can be borrowed from one docking station, ridden, and then returned to any docking station in the system Over the years marketing campaigns have been broad and targeted a cross-section of potential users. Data analysis has shown that riders with an annual membership are more profitable than casual riders. The marketing team is interested in creating a campaign to encourage casual riders to convert to members.
The marketing analyst team would like to know how annual members and casual riders differ, why casual riders would buy a membership, and how Cyclistic can use digital media to influence casual riders to become members. The team is interested in analyzing the Cyclistic historical bike trip data to identify trends in the usage of bikes by casual and member riders

# I. Ask
## Business Objective 

To increase profitability by converting casual riders to annual members via a targeted marketing campaign.

## Business Task for Junior Analyst

The junior analyst has been tasked with answering this question: How do annual members and casual riders use Cyclistic bikes differently?

## Stakeholders

* **Cyclistic:** A bike-share program that features more than 5,800 bicycles and 600 docking stations. Cyclistic sets itself apart by also offering reclining bikes, hand tricycles, and cargo bikes, making bike-share more inclusive for people with disabilities and riders who can’t use a standard two-wheeled bike. Most riders opt for traditional bikes; about 8% of riders use assistive options. Cyclistic users are more likely to ride for leisure, but about 30% use them to commute to work each day.
* **Lily Moreno:** The director of marketing and your manager. Moreno is responsible for the development of campaigns and initiatives to promote the bike-share program. These may include email, social media, and other channels.
* **Cyclistic marketing analytics team:** A team of data analysts who are responsible for collecting, analyzing, and reporting data that helps guide Cyclistic marketing strategy.
* **Cyclistic executive team:** The notoriously detail-oriented executive team will decide whether to approve the recommended marketing program.

# II. PREPARE

## Where is Data Located?
The data used for this analysis were obtained from Motivate, a company employed by the City of Chicago to collect data on bike share usage.

## How is the Data Organized?
The data is organized in monthly CSV files. The most recent twelve months of data (August 2020 – July 2021)  were used for this project. The files consist of 13 columns containing information related to ride id, ridership type, ride time, start location and end location, geographic coordinates, etc.

## Credibility of the Data
The [data](https://divvy-tripdata.s3.amazonaws.com/index.html) is collected directly by Motivate, Inc., the company that runs the Cyclistic Bike Share program for the City of Chicago. The data is comprehensive in that it consists of data for all the rides taken on the system and is not just a sample of the data. The data is current. It is released monthly and, as of August 2021, was current to July 2021. The City of Chicago makes the data available to the public.

## Licensing, privacy, security, and accessibility
This data is anonymized as it has been stripped of all identifying information. This ensures privacy, but it limits the extent of the analysis possible. There is not enough data to determine if casual riders are repeat riders or if casual riders are residents of the Chicago area. The data is released under this [license](https://ride.divvybikes.com/data-license-agreement).

## Ability of Data to be used to answer Business Questions
One of the fields in the data records the type of rider; casual riders pay for individual or daily rides and member riders purchase annual subscriptions. This information is crucial to be able to determine the differences between how the two groups use the bike share program.

## Problems with the data
There are some problems with the data. Most of the problems (duplicate records, missing fields, etc.) can be dealt with by data cleaning, but a couple of issues require further explanation.

### Rideable-type Field
The rideable_type field contains one of three values – Electric bike, Classic bike, or Docked bike. Electric and Classic bikes seem self-explanatory, but what exactly a Docked bike is unclear. From a review of the data, it seems that electric bikes were available to both types of users for the entire 12-month period; classic bikes were available to both groups of users but only from December 2, 2020, to July 31, 2021; and Docked Bikes were available to members from August 1, 2021, to January 13, 2021, and to casual users for the entire 12 months. For the purpose of this analysis, these rideable types will not be used to segment the data or draw any conclusions about bike preferences as data collection for this variable is not consistent across the time period being analyzed.

### Latitude and Longitude
There is also a challenge with the latitude and longitude coordinates for the stations. Each station is represented by a range of lat/long coordinates. The start/end latitude and longitude seem to indicate the location of a particular bike. Creating a list of popular stations is not difficult, but mapping the stations is more problematic. This was remedied by averaging the lat and long values for the stations before mapping. This resulted in the ride counts for a station matching the ride count for one set of lat/long coordinates.

# III. PROCESS & CLEAN

## What tools are you choosing and why?
For this project, I chose to use RStudio Desktop to analyze and clean the data and Tableau to create the visualizations. The data set was too large to be processed in spreadsheets and RStudio Cloud.

## Review of Data
Data was reviewed to get an overall understanding of the content of fields, and data formats, and to ensure its integrity. The review of the data involved, checking column names across the 12 original files and checking for missing values, trailing white spaces, duplicate records, and other data anomalies.

The review of the data revealed several problems:
- Duplicate record ID numbers
- Records with missing start or end stations
- Records with very short or very long ride durations
- Records for trips starting or ending at an administrative station (repair or testing station)

Once the initial review was completed, all twelve files were loaded into four data frames (quarterly) and then into a single data frame. The resulting amalgamated file consisted of 4.731,081 rows with 13 columns of character and numeric data. This matched the number of records in the twelve-monthly files.

## Extraction of Data from Existing Fields
To allow for more granular analysis of the data and more insights, several new columns were created and populated with data from the started_at column of date and time. These new columns were day, month, year, time, and day of the week.

Another column was created to contain the trip duration (length of each trip). The data for this column was created by calculating the difference in time between the start and end time of the ride. Another version of this column was then created to contain the trip duration in minutes.

## Data Cleaning
Duplicate records (based on the RIDE ID field) were removed. (209 records removed)

![Screenshot (180)](https://github.com/kingstrybe/kingstrybe/assets/124252840/888a813c-9241-4d01-88ef-96cf27951a33)

Records for trips less than 60 seconds (false starts) or longer than 24 hours were removed. Bikes out longer than 24 hours are considered stolen and the rider is charged for a replacement. (82,282 records removed)

![Screenshot (181)](https://github.com/kingstrybe/kingstrybe/assets/124252840/a1b2e6cd-ae54-4f24-889f-b381948fa34e)

Records with missing fields start_station, end_station, start/end lat/long fields were removed. (544,204 records removed)

![Screenshot (182)](https://github.com/kingstrybe/kingstrybe/assets/124252840/89d9d265-2b7f-4aa1-82dd-042365c04cbe)


Initially, the data set contained 4,731,081 records. Once data was cleaned, 4.101,243 records remained. 13.3% of the records were removed.

# IV. ANALYZE
Once the data was cleaned, analysis of the data was undertaken in RStudio to determine the following:

- Mean, median, maximum, and minimum ride duration (by rider type)
- Average ride length by day and by rider type
- Count of trips by rider type
- Count of trips by bicycle type
- Count the number of start stations

The cleaned data set was used to create a CSV file that was exported from RStudio and imported into Tableau for further analysis and creation of visualizations.

Tableau was used to analyze the data further and determine the following:

- Ride duration
- Times of Day for rides
- Days of the week for rides
- Months of the year of the rides
- Top 20 start stations by user type
- Top 20 end stations by user type

# Summary of analysis
From the analysis, we can see that there are several key differences between casual and member riders.

From the analysis, we can see that there are several key differences between casual and member riders.

# Number and Length of Rides
Member riders take more trips than casual riders but casual riders take longer rides than member riders (Figure 1). Casual riders average 31.85 minutes per ride as opposed to 14.43 minutes for member riders (Figure 2). The total amount of time spent by casual riders is greater than by member riders (Figure 3).

![Total length of rides by rider types](https://github.com/kingstrybe/kingstrybe/assets/124252840/32eba7a7-5fd2-48dc-b816-0fd9c023a2ee)
Pie graph showing that member riders take about 56% of all rides.  Casual riders take 44%
Figure 1.

![Overview 1](https://github.com/kingstrybe/kingstrybe/assets/124252840/1ffc6827-9319-4974-98d1-d18f568db252)
Graph showing casual riders trips take an average of 131.85 minutes and member riders trips take an average of 14.43 minutes.
Figure 2.

![Overview 2](https://github.com/kingstrybe/kingstrybe/assets/124252840/058ea18d-ca63-42fb-9063-a764d0023210)
Figure 3.

# Trips by Time/Day/Month

![Screenshot (189)](https://github.com/kingstrybe/kingstrybe/assets/124252840/49215b47-5745-4532-8898-afa8ff0fe6dc)
Graph showing start hours for casual and member riders.  Member riders have three peak times - the highest at 5 p.m., another peak at lunchtime, and another peak at 8 a.m.  The number of casual rides increases during the day and peaks at 5 pm
Figure 4

The number of trips made by casual riders increases over the day, peaking at 5 p.m. Member trips also peak at 5 p.m. but there are two smaller peaks earlier in the day at 8 a.m. and lunchtime, which corresponds with the work day.

![Trips by day of the week](https://github.com/kingstrybe/kingstrybe/assets/124252840/0bd558e0-d3f5-4f67-8580-e0f5f0c82976)
Figure 5.
Figure 5 shows that weekend days are popular with casual riders whereas member rider trips are spread out more evenly throughout the week.

![Screenshot (186)](https://github.com/kingstrybe/kingstrybe/assets/124252840/d655b020-047c-4ea6-bfc1-e2ad13597c3d)
Figure 6.
The winter months (December, January, and February) see very few rides. The summer months are popular with both types of riders. July is the busiest month for casual riders.

# Station Usage
Member and casual riders also differ in the stations that are popular for starting and ending their rides.

Top starting and destination stations for casual riders cluster around tourist destinations within about 1 km of the lakefront from Lincoln Park in the north to the Field Museum in the south. Member riders' top stations are more spread out and reflect office locations.

![screen-shot-2021-09-15-at-5 16 20-pm](https://github.com/kingstrybe/kingstrybe/assets/124252840/b1947477-e4f2-465f-88f8-06bd0f695205)
Figure 7

![screen-shot-2021-09-15-at-7 47 38-pm](https://github.com/kingstrybe/kingstrybe/assets/124252840/db77b001-8205-4aec-976e-04181020ad8d)
Figure 8

## Recommendations

Without a user experience (UX) study, we cannot assess the customer's preferences and pain points. The following recommendations are limited to the assumptions above:

1. Develop a seasonal marketing strategy aiming at casual riders.
    * Because casual riders are usually interested in summer usage of the Cyclistic bike-share program, the marketing department could develop an appropriate marketing strategy around summer participation.
    *  Cyclistic could determine the suitability of stations and surrounding public space for deployment of promotional materials, focusing on proximity to local establishments and recreational areas that are popular during the summer.
    *  Since patrons may be active in recreational activities with others, Cyclistic could develop a referral program that pairs with the annual membership, with a generous discount for one or both parties. For example, first-time members receive a discounted membership if they apply a referral code during the purchase. Likewise, the associated member could receive a small discount on electric bicycles for the summer.
2. Partner with local institutions and establishments to sponsor the Cyclistic membership.
    * Through partnerships with local institutions and establishments, annual membership is promoted to employees and students. For example, a first-time subscription discount may be suitable because Cyclistic is developing a strategy to convert existing riders. 
    * Similar to above, Cyclistic could focus on local establishments and recreational areas, which are popular during the summer. However, instead of advertising around these areas, Cyclistic could partner with their corresponding organizations for marketing. 

However, there is one recommendation for Cyclistic if the company is interested in structural changes.

3. Develop a seasonal pass for casual riders during the summer.
    * Some demographics are specifically available in the summer, including but not limited to: tourists and student populations. Because of the expected decline in utility for annual memberships, a seasonal pass is a compromise between the options for casual riders and the membership.
