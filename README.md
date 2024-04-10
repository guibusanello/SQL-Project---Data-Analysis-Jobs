# SQL-Project---Data-Analysis-Jobs
## Introduction

Focusing on data analyst job roles, this project explores top-paying jobs, in-demand skills and where high demand meets high salary in data analytics.

This project was build for my course SQL for Data Analytics (https://www.youtube.com/watch?v=7mz73uXD9DA) by Luke Barousse.

Thank you so much Luke!

## Background

Driven by a quest to navigate the data analyst job market more effectively, this project was born from a desire to pinpoint top-paid and in-demand skills, streamlining other to work to find optimal jobs.

Link to Dataset: https://lukebarousse.com/sql

### Questions I wanted to answer in this project

1) What are the top-paying data analyst jobs?
2) What skills are required for these top-paying jobs?
3) What skills are most in demand for Data Analysts?
4) Which skills are associated with higher salaries?
5) What are the most optimal skills to learn?

## Tools I used

For deep diving into data, I used this tools

1) SQL: used to query database and find valuable insights.
2) PostgreSQL: database management system.
3) VSCode: database management and executing SQL Queries.
4) Git & Github: version control, sharing SQL scripts and analysis and project tracking.
5) Excel: create some graphics

## The analysis

Each query for this project aimed at investigation specifics aspects of data analyst job market.
Here's how I approached each question:

### What are the top-paying data analyst jobs?

To identify the highest paying roles, I filtered data analyst positions by average yearly salary and location and focusing on remote jobs. This query highlits the high paying opportunities in the field.

```sql
-- Identifying top 10 highest-paying Data Analyst roles that are avaliable remotely

SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10

-- Identifying which companies these jobs belongs to

SELECT
    job_id,
    job_title,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date,
    name AS company_name
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst' AND 
    job_location = 'Anywhere' AND
    salary_year_avg IS NOT NULL
ORDER BY
    salary_year_avg DESC
LIMIT 10
```

Here's the breakdown of the top data analyst jobs in 2023:

**Wide Salary Range:** Top 10 paying data analyst roles span from $184k to $650k, indicating significant salary potential.
**Diverse Employers:** Companies like SmartAsset, Meta and AT&T are among those offering high salaries, showing a broad interest across different industries.
**Job title variety:** There is a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

![1_top_paying_roles](https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/3b9b37f0-a95c-4001-b20e-62b359656c08)

### What are the top-paying data analyst jobs?

To understand what skills are required for the top-paying jobs, I joined the job postings with the skills data, providing insights into what employers value for high-compensation roles.

```sql
-- creating a CTE to organize our query

WITH top_paying_jobs AS (

    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT *
FROM top_paying_jobs

-- joining tables

WITH top_paying_jobs AS (

    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id

-- ordering by salary

WITH top_paying_jobs AS (

    SELECT
        job_id,
        job_title,
        salary_year_avg,
        name AS company_name
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst' AND 
        job_location = 'Anywhere' AND
        salary_year_avg IS NOT NULL
    ORDER BY
        salary_year_avg DESC
    LIMIT 10
)

SELECT
    top_paying_jobs.*,
    skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY
    salary_year_avg DESC
```

Here's the breakdown of the most demanded skills for the top 10 highest paying data analyst jobs in 2023:

**SQL** is leading with a bold count of 8.
**Python** follows closely with a bold count of 7.
**Tableau** is also highly sought after, with a bold count of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.

![2_top_paying_job_skills](https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/7c00e60d-c992-4381-a252-29504eedd817)

### In-Demand Skills for Data Analysts

This query helped identify the skills most frequently requested in job postings, directing focus to areas with high demand.

```sql
-- JOINING TABLES

SELECT *
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
LIMIT 5

-- SHOWING TOP 5 IN-DEMAND SKILLS FOR DATA ANALYST

SELECT
    skills,
    COUNT(skills_job_dim.job_id) AS demand_count
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
GROUP BY
    skills
ORDER BY
    demand_count DESC
LIMIT 5
```

Here's the breakdown of the most demanded skills for data analysts in 2023

**SQL** and **Excel** remains fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
**Programming and Visualization Tools like Python, Tableau, and Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

![3_top_demanded_skills](https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/1a91c23e-e847-4a22-8402-dccd3fc78a94)



## What I learned


## Conclusions
