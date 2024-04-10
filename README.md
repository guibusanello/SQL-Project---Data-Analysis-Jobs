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

![Top-paying jobs] (sql_project_data_analyst_job/all_folders/assets/1_top_paying_roles.png)

## What I learned


## Conclusions
