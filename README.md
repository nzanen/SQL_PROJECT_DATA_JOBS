# Introduction

This is a small SQL project made along the tutorial of Luke Barousse's [SQL for Data Analytics](https://www.youtube.com/watch?v=7mz73uXD9DA). The project uses a data set of Data Science job postings worldwide in 2023. Focusing on data analyst roles, this project explores top-paying jobs, in-demand skills and where high demand meets high salary in data analytics.

### Database ERD

![Project ERD](/ERD.png)

You can find the SQL queries that were used here: [project_sql folder](/project_sql/)

# Tools used

- **PostgreSQL:** for hosting the job database locally
- **SQL:** for querying the the local database
- **Visual Studio Code:** for connecting to the local database, writing and submitting SQL queries
- **Git & Github:** for version control and content sharing

# The Analysis

### 1. Top 10 Paying Data Analyst Jobs in The Netherlands

Determine what are the top 10 best paying jobs for the role of Data Analyst in The Netherlands

```sql
SELECT
    job_id,
    job_title,
    name AS company_name,
    job_location,
    job_schedule_type,
    salary_year_avg,
    job_posted_date
FROM
    job_postings_fact
LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
WHERE
    job_title_short = 'Data Analyst'
    AND job_location LIKE '%Netherlands%'
    AND salary_year_avg IS NOT NULL
ORDER BY salary_year_avg DESC
LIMIT 10
```

The data shows:

- **Wide salary range**: the top 10 data analyst roles in The Netherlands show a wide anual salary range, ranging from \$98,500 up to \$177,283.
- **Location:** the vast majority of the highest paying jobs are located in Amsterdam
- **International companies**: most of the top paying jobs are at internation companies with a local office in The Netherlands, typically in Amsterdam as mentioned above.

### 2. What are the skills required for the top paying jobs?

Diving deeper into the top 10 paying jobs, what are the skills required for these jobs?

```sql
WITH top_paying_jobs AS (
    SELECT
        job_id,
        job_title,
        name AS company_name,
        job_location,
        salary_year_avg
    FROM
        job_postings_fact
    LEFT JOIN company_dim ON job_postings_fact.company_id = company_dim.company_id
    WHERE
        job_title_short = 'Data Analyst'
        AND job_location LIKE '%Netherlands%'
        AND salary_year_avg IS NOT NULL
    ORDER BY salary_year_avg DESC)

SELECT
    top_paying_jobs.*,
    skills_dim.skills
FROM top_paying_jobs
INNER JOIN skills_job_dim ON top_paying_jobs.job_id = skills_job_dim.job_id
INNER JOIN skills_dim ON skills_job_dim.skill_id = skills_dim.skill_id
ORDER BY salary_year_avg DESC
```

# What I Learned

# Conclusions
