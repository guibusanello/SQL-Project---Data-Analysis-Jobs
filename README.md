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

- **Wide Salary Range:** Top 10 paying data analyst roles span from $184k to $650k, indicating significant salary potential.
- **Diverse Employers:** Companies like SmartAsset, Meta and AT&T are among those offering high salaries, showing a broad interest across different industries.
- **Job title variety:** There is a high diversity in job titles, from Data Analyst to Director of Analytics, reflecting varied roles and specializations within data analytics.

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

- **SQL** is leading with a bold count of 8.
- **Python** follows closely with a bold count of 7.
- **Tableau** is also highly sought after, with a bold count of 6. Other skills like R, Snowflake, Pandas, and Excel show varying degrees of demand.

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

- **SQL** and **Excel** remains fundamental, emphasizing the need for strong foundational skills in data processing and spreadsheet manipulation.
- **Programming and Visualization Tools like Python, Tableau, and Power BI** are essential, pointing towards the increasing importance of technical skills in data storytelling and decision support.

![3_top_demanded_skills](https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/1a91c23e-e847-4a22-8402-dccd3fc78a94)


### Skills based on salary

Exploring the average salaries associated with different skills revealed which skills are the highest paying.

```sql
SELECT
    skills,
    ROUND(AVG(salary_year_avg), 0) AS avg_salary
FROM job_postings_fact
INNER JOIN skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = True
GROUP BY
    skills
ORDER BY
    avg_salary DESC
LIMIT 20;
```

Here's a breakdown of the results for top paying skills for Data Analysts:

- **High Demand for Big Data & ML Skills:** Top salaries are commanded by analysts skilled in big data technologies (PySpark, Couchbase), machine learning tools (DataRobot, Jupyter), and Python libraries (Pandas, NumPy), reflecting the industry's high valuation of data processing and predictive modeling capabilities.
- **Software Development & Deployment Proficiency:** Knowledge in development and deployment tools (GitLab, Kubernetes, Airflow) indicates a lucrative crossover between data analysis and engineering, with a premium on skills that facilitate automation and efficient data pipeline management.
- **Cloud Computing Expertise:** Familiarity with cloud and data engineering tools (Elasticsearch, Databricks, GCP) underscores the growing importance of cloud-based analytics environments, suggesting that cloud proficiency significantly boosts earning potential in data analytics.

![4_top_paying_skills](https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/cf4b3412-5bb6-467b-a2b8-ab5d5cfaf807)

### Most Optimal Skills to Learn

Combining insights from demand and salary data, this query aimed to pinpoint skills that are both in high demand and have high salaries, offering a strategic focus for skill development.

```sql
SELECT
    skills_dim.skill_id,
    skills_dim.skills,
    COUNT(skills_job_dim.job_id) AS demand_count,
    ROUND(AVG(job_postings_fact.salary_year_avg), 0) AS avg_salary
FROM
    job_postings_fact
INNER JOIN
    skills_job_dim ON job_postings_fact.job_id = skills_job_dim.job_id
INNER JOIN
    skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
WHERE
    job_title_short = 'Data Analyst'
    AND salary_year_avg IS NOT NULL
    AND job_work_from_home = TRUE
GROUP BY
    skills_dim.skill_id
HAVING
    COUNT(skills_job_dim.job_id) > 10
ORDER BY
    avg_salary DESC,
    demand_count DESC
LIMIT 25
```
Here's a breakdown of the most optimal skills for Data Analysts in 2023:

- **High-Demand Programming Languages:** Python and R stand out for their high demand, with demand counts of 236 and 148 respectively. Despite their high demand, their average salaries are around $101,397 for Python and $100,499 for R, indicating that proficiency in these languages is highly valued but also widely available.
- **Cloud Tools and Technologies:** Skills in specialized technologies such as Snowflake, Azure, AWS, and BigQuery show significant demand with relatively high average salaries, pointing towards the growing importance of cloud platforms and big data technologies in data analysis.
- **Business Intelligence and Visualization Tools:** Tableau and Looker, with demand counts of 230 and 49 respectively, and average salaries around $99,288 and $103,795, highlight the critical role of data visualization and business intelligence in deriving actionable insights from data.
- **Database Technologies:** The demand for skills in traditional and NoSQL databases (Oracle, SQL Server, NoSQL) with average salaries ranging from $97,786 to $104,534, reflects the enduring need for data storage, retrieval, and management expertise.

<img width="298" alt="5_most_optimal_skills" src="https://github.com/guibusanello/SQL-Project---Data-Analysis-Jobs/assets/157152963/f9787a03-1030-450c-95f9-9eda88901991">

## What I learned

Through this course, I was able to learn more about SQL tools, some complex querying, VSCode and develop my analytical thinking.

## Conclusions and Insights

From the analysis, several insights have been founded:

1) Top-paying data analyst jobs: The highest-paying jobs for data analysts is at $650,000.
2) Skills for top-paying jobs: High-paying data analyst jobs require advanced proficiency in SQL.
3) Most-in demand skills: SQL is also the most demanded skill in the data analyst job market.
4) Skills with higher salaries: Specialized skills, such as SVN and Solidity, are associated with the highest average salaries.
5) Optimal skills for Job Market Value: SQL leads in demand and offers for a high average salary, therefore it as one of the most optimal skills for data analysts.

This project enhanced my SQL skills and provided valuable insights into the data analyst job market. The findings from the analysis serve as a guide to prioritizing skill development and job search efforts. Aspiring data analysts can better position themselves in a competitive job market by focusing on high-demand, high-salary skills.



