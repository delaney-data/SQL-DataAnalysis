# SQL Data Analysis
A demonstration of creating SQL queries to provide insights into a data set. 

### Table of Contents
1) [Context of the Dataset](https://github.com/delaney-data/SQL-DataAnalysis#the-problem--the-digital-divide-)
2) [Database Schema](https://github.com/delaney-data/SQL-DataAnalysis/blob/main/README.md#database-schema)
3) Data Analysis - Question #1
	- [What are the top 10 states with the highest number of people without internet?](https://github.com/delaney-data/SQL-DataAnalysis#question-1-what-are-the-top-10-states-with-the-highest-number-of-people-without-internet)
		- [Part 1: Calculate the value of a Percentage](https://github.com/delaney-data/SQL-DataAnalysis/blob/main/README.md#q1-solution-first-step) 
		- [Part 2: Solution](https://github.com/delaney-data/SQL-DataAnalysis/blob/main/README.md#q1-solution-second-step)
		- [Results & Insights](https://github.com/delaney-data/SQL-DataAnalysis#q1-data-insights)
4) Data Analysis - Question #2
	- [In Progess]



<strong>Language and Utilities Used:</strong>
- Language: `SQL`
- DBMS: [PostgreSQL 14](https://www.postgresql.org/)
- GUI: [PgAdmin 4](https://www.pgadmin.org/) on Windows 10
- Data Source: [USA People without Internet Access in 2016](https://www.kaggle.com/datasets/madaha/people-without-internet/)



## The Problem [ The Digital Divide ]
As the technology in the world continues to advance and more services are exclusively accessed via the Internet, the impact of the Digital Divide (a gap between those who have access to digital technology and those who do not) can be felt harder to those on the receiving end.

<p align="center"><img src="https://uspirg.org/sites/pirg/files/Digital%20Divide%20Blog.png"></p align>

The Digital Divide issue is complex and is a culmination of a Lack of Technology, a Lack of Internet Access and a Lack of Digital Literacy.
The coronavirus crisis has shown that not being digitally connected, or not having internet access in the household, can result in negative affects that range from: 

:technologist: Lack of job opportunities & employment 
   - Remote or Hybrid work
   - Applying to Jobs
   - Lack of Job-ready Skills (navigating the internet, programs, etc)
 
 :books: Lack of educational resources 
   - Online Schooling
   - Online Education resources
  
  :no_entry: Restricted from internet-only services
   - Account Activation / Cancellation
   - Paying Bills Online
   - Paperless Documentation
   - Ordering Services (Food, Supplies, etc)
   - Smart Devices that require internet to fully function
 

## Notes & Prerequisites
I will be analyzing data focusing specifically on the <b>Lack of Internet</b>.

Prior to this chapter, I have completed the prerequisite step of importing the dataset into PostgreSQL.
  - View my tutorial here: [Create Tables & Import Data with SQL repository](https://github.com/delaney-data/SQL-CreateTablesImport)
  
According to the [source data](https://www.kaggle.com/datasets/madaha/people-without-internet), data was only collected in US counties where the population was greater than 65K in the year 2016.
>Content: This dataset contains data for counties with population over 65000, compiled from the 2016 ACS 1-year estimate. ACS 1-year estimates only summarize data for large geographic areas over 65000 population. It provides sufficient data for us to gain insight into internet use.

## Database Schema

### `County_Pop` Table
| Column Name   |  Data Type, Constraint |Description |
|----------|:-------------:|:------|
|id_pop| integer, primary key | Used as primary key for the county population table.|
|county| varchar| Each county per state|
|state| varchar| Each state|
|p_total| integer| Total Population in numbers per county |
|p_white| integer| Population grouped by race: white|
| p_black |integer| Population grouped by race: black|
|p_asian |integer|Population grouped by race: asian|
|p_native |integer|Population grouped by race: native|
|p_hawaiian |integer|Population grouped by race: hawaiian|
|p_others |integer|Population grouped by race: other|
|percent_no_internet| numeric| Percentage without Internet|
|**pop_no_internet |decimal | Population in numbers without Internet|

**<sub> I manually added new this column (as it did not exist in the dataset) and inserted values into the rows in order to solve our [Analysis Question #1](https://github.com/delaney-data/SQL-DataAnalysis#q1-solution).</sub>


### `Education_Income` Table
| Column Name   |  Data Type, Constraint |Description |
|----------|:-------------:|:------|
|id | integer, primary key | Used as primary key for the education and income table.|
|county| varchar| Each county per state|
|state| varchar| Each state|
|p_below_middle_school| integer| Education level per population: Below Middle School|
|p_some_high_school| integer| Education level per population: Some High School|
|p_high_school_equivalent| integer| Education level per population: High School Equivalent|
|p_some_college integer| integer | Education level per population: Some College |
|p_bachelor_and_above |integer| Education level per population: Bachelors and above|
|p_below_poverty| integer| Income Level per population: Below Poverty|
|median_age |numeric| Median age of household per county|
|median_household_income| numeric | Median Household Income per county|
|median_rent_per_income |numeric | Median Rental Income per county|



# Data Analysis

### Question 1: What are the <b>top 10 states</b> with the highest <b>number</b> of people without internet?

SQL statements/commands we'll need to use to solve the question:
- `ALTER TABLE`,`ADD COLUMN`
- `UPDATE`, `SET`
  - Math Operators and Subqueries
- `SUM`, `GROUP BY`, `ORDER BY`, `LIMIT`

### Q1 Solution (calculate the value of a percentage first):

First, we'll need to add a new column to the `county_pop` table, as the question calls for values we do not have yet (number without internet). We can find those values with the percentage_no_internet values and p_total values. 

The new column will be a decimal type so I can run mathmatical operations/aggregates with it. I will name the column pop_no_internet.

```sql
ALTER TABLE county_pop
 ADD COLUMN pop_no_internet DECIMAL;
```

Next, let's insert the data into the new column using math operators. The SET command will add your desired values. 

```sql
UPDATE county_pop
 SET pop_no_internet = (percent_no_internet * 0.01) * p_total;

-- SQL executes in order of operations, starting with anything in parathesis first 
-- Percent_no_internet column has decimal value percentage
-- So we'll need to multiply (*) by 0.01 first, then multiply the result by p_total which has the total number of the population
```
View the results:

```sql

SELECT 
	county,
	state,
	ROUND(percent_no_internet) AS percentage_no_internet,
	ROUND(pop_no_internet) AS population_no_internet,
	p_total as total_population

FROM county_pop;

-- I enclosed the columns with a ROUND statement to reduce decimal points and used AS to create an ALIAS for clarity
```
<img src="https://i.imgur.com/VJAy8Nz.png" height = "50%" widge= "50%" alt= "Q1 SQL Results">

### Q1 Solution (second step):
We can now use begin using the new population_no_internet column with `SUM` and `GROUP BY` to answer the original question of:

<i>"What are the top 10 states with the highest <b>number</b> of people without internet?"</i>

```sql
 SELECT county_pop.state AS "State",
    SUM(ROUND(county_pop.pop_no_internet)) AS "Population without Internet" -- Adding all the values to get the TOTAL per county

FROM county_pop
GROUP BY county_pop.state -- Grouping by state, which will roll up all the counties
ORDER BY SUM(pop_no_internet) DESC -- Organizing by the largest to the smallest integers with DESC
LIMIT 10; -- Limiting to the top 10 results

```
### Q1 Data Insights: 

Top 10 States where there's the highest amount of people without internet accces. 

| State | Population without Internet | 
| ------------- | ------------- |
|CA|	 4,615,690| 
|TX|	 3,684,136| 
|FL|	 2,864,638| 
|NY|	 2,737,094| 
|PA|	 1,901,010| 
|IL|	 1,494,638| 
|OH|	 1,480,742| 
|MI|	 1,267,995| 
|NC|	 1,216,635| 
|NJ|	 1,114,282| 

Overlapping with total population vs total without internet access. 
![image](https://user-images.githubusercontent.com/66498659/180867482-0fa5e263-cf11-43f3-84a7-2043d0ee400d.png)


<hr>


### Question 2: Which state has the highest percentage of those without internet? The Lowest?
[In Progress]

### Question 2.1: Of the highest and lowest, what is the number of those living below the poverty line?
[In Progess]
