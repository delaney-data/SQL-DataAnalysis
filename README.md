# SQL Data Analysis
 Answering questions about data using SQL.

## Overview
Using a dataset from Kaggle, we will provide insights on using SQL. 

<strong>Language and Utilities Used:</strong>
- Language: `SQL`
- DBMS: [PostgreSQL 14](https://www.postgresql.org/)
- GUI: [PgAdmin 4](https://www.pgadmin.org/) on Windows 10
- Data Source: [USA People without Internet Access in 2016](https://www.kaggle.com/datasets/madaha/people-without-internet/)
- Notes on Source: Data was only collected in US counties where the population was greater than 65K


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

Notes: 
- I have already set up my tables and inserted values from the dataset into my database. You can check out this prerequisite step over here:
- Data was only collected in US counties where the population was greater than 65K.

### Question 1: What is the number of people without internet per US State?
Approach: To answer this question, I will need to use some math to find the <b>number</b> (integer) of the population without internet. The data set only provided a <b>percentage</b> column/values for those without internet. I will add a new column to the table which calculates the number using the total population number and percantage. 

  SQL statements/commands we'll need to use to solve the question:
- `ALTER TABLE`,`ADD COLUMN`
- `UPDATE`, `SET`
  - Math Operators and Subqueries
- `SUM`, `GROUP BY`

Solution:

### Question 2: Which state has the highest percentage of those without internet? The Lowest?

### Question 2.1: Of the highest and lowest, what is the number of those living below the poverty line?
