# SQL Data Analysis
A demonstration of creating SQL queries to provide insights into data. 

## Table of Contents
1) [Context of the Dataset](https://github.com/delaney-data/SQL-DataAnalysis#the-problem--the-digital-divide-)
2) Data Analysis
	- [Q1 What are the top 10 states with the highest number of people without internet?](https://github.com/delaney-data/SQL-DataAnalysis#question-1-what-are-the-top-10-states-with-the-highest-number-of-people-without-internet)
		- [Solution](https://github.com/delaney-data/SQL-DataAnalysis#q1-solution)
		- [Results & Insights](https://github.com/delaney-data/SQL-DataAnalysis#q1-data-insights)
	- Q2 [In Progess]
	- Q3 [In Progress]
<hr>

<strong>Language and Utilities Used:</strong>
- Language: `SQL`
- DBMS: [PostgreSQL 14](https://www.postgresql.org/)
- GUI: [PgAdmin 4](https://www.pgadmin.org/) on Windows 10
- Data Source: [USA People without Internet Access in 2016](https://www.kaggle.com/datasets/madaha/people-without-internet/)



## The Problem [ The Digital Divide ]
As the technology in the world continues to advance and more services are exclusively accessed via the Internet, the impact of the Digital Divide (a gap between those who have access to digital technology and those who do not) can be felt harder to those on the receiving end.

<img src="https://uspirg.org/sites/pirg/files/Digital%20Divide%20Blog.png">

The Digital Divide issue is complex and is a culmination of: 
- Lack of Technology
- Lack of Internet Access
- Lack of Digital Literacy

The coronavirus crisis has shown that not being digitally connected, or not having internet access, can result in negative consequences that range from: 
- Lack of job opportunities & employment :technologist:
    - Remote or Hybrid work
    - Applying to Jobs
    - Lack of Job-ready Skills (navigating the internet, programs, etc)
- Lack of educational resources :books:
    - Online Schooling
    - Online Education resources
- Restricted from internet-only services :no_entry:
    - Account Activation / Cancellation
    - Paying Bills Online
    - Paperless Documentation
    - Ordering Services (Food, Supplies, etc)
    - Smart Devices that require internet to fully function
 
 I will be analyzing data focusing specifically on the <b>Lack of Internet</b>.
 
# Analysis

Note: 
- I have completed the prerequisite step of importing the dataset into PostgreSQL.
  - To view this go to my [Create Tables & Import Data with SQL repository](https://github.com/delaney-data/SQL-CreateTablesImport)
- According to the [source data](https://www.kaggle.com/datasets/madaha/people-without-internet), data was only collected in US counties where the population was greater than 65K in the year 2016.
>Content: This dataset contains data for counties with population over 65000, compiled from the 2016 ACS 1-year estimate. ACS 1-year estimates only summarize data for large geographic areas over 65000 population. It provides sufficient data for us to gain insight into internet use.


### Question 1: What are the <b>top 10 states</b> with the highest <b>number</b> of people without internet?

SQL statements/commands we'll need to use to solve the question:
- `ALTER TABLE`,`ADD COLUMN`
- `UPDATE`, `SET`
  - Math Operators and Subqueries
- `SUM`, `GROUP BY`, `ORDER BY`, `LIMIT`

### Q1 Solution:

First, we'll need to add a new column to the `county_pop` table. I need this column to be a decimal type so I can run mathmatical operations/aggregates with it. I will name the column pop_no_internet.

```sql
ALTER TABLE county_pop
 ADD COLUMN pop_no_internet DECIMAL;
```

Next, let's insert the data into the new column using math operators. The SET command will add your desired values. 

```sql
UPDATE county_pop
 SET pop_no_internet = (percent_no_internet * 0.01) * p_total;

--- percent_no_internet column has decimal value percentage, we'll need to multiply (*) by 0.01 first, then multiply the result by p_total which has the total number of the population
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

--- Used the ROUND statment to reduce decimal points and using the AS to create an Alias for clarity
```
<img src="https://i.imgur.com/VJAy8Nz.png" height = "50%" widge= "50%" alt= "Q1 SQL Results">

We can now use begin using the new population_no_internet column with `SUM` and `GROUP BY` to answer the original question of:

<i>"What are the top 10 states with the highest <b>number</b> of people without internet?"</i>

```sql
 SELECT county_pop.state AS "State",
    SUM(ROUND(county_pop.pop_no_internet)) AS "Population without Internet" --- Adding all the values to get the TOTAL per county

FROM county_pop
GROUP BY county_pop.state --- Grouping the counties per state
ORDER BY SUM(pop_no_internet) DESC --- Organizing by the largest to the smallest
LIMIT 10; ---Limiting to the top 10 results

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
