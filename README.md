# Data Jobs in Finland What Employers want and what they pay :An end-to-end data analysis of Finland's data job market, built with Python.

Author: Olufade Segun Sunday 

Skills shown: data cleaning, exploratory data analysis, data visualisation, analytical storytelling, written reporting.

Plain-language summaries are written for everyone. "Technical note" lines and collapsible code blocks are for analysts who want the detail.

Contents: Executive Summary · Introduction · Methodology · Findings & Insights · Recommendations · Conclusions
## Executive Summary

I analysed data-related job postings in Finland, taken from a dataset of roughly 2 million postings worldwide, to answer a practical question: what should someone learn, and where should they look, to get a data job in Finland? The answer is clearest on skills. SQL and Python are the foundation of the market: each appears in 43% of Data Analyst postings, and Python appears in 63% of both Data Engineer and Data Scientist postings. Power BI is the most requested analyst tool after them (26%). Opportunities are concentrated in the capital region, where Helsinki has nearly four times as many postings as Espoo. Median yearly pay runs from €87K for Data Analysts to €106K for Senior Data Analysts, and 45.9% of postings do not mention a degree at all.

My recommendation is to master SQL and Python first, add Power BI, and focus the search on Helsinki and Espoo, treating cloud skills such as Azure as the next step. One caution: the skill-versus-salary results for Data Analysts rest on only 9 postings that list a salary, so they show direction, not proof.

## Introduction

Background. Data roles such as Data Analyst, Data Scientist, Data Engineer and Business Analyst are among the most talked-about careers in tech, but the advice people get is often generic and not specific to their country.

Problem statement. Someone planning a data career in Finland has no easy way to see which skills employers ask for, which employers pay well, what the roles pay, and how these things differ by role. Without evidence, learners risk spending months on skills the local market rarely requests.

Objectives. This project answers six questions using real job postings:
- Where in Finland are data jobs located?
- Which companies pay the most?
- How much do the different data roles pay?
- Which skills does each role ask for most often?
- Does skill demand change through the year?
- Which skills pay the most for Data Analysts, and which offer the best balance of demand and pay?

A seventh, smaller question is whether postings mention degrees and benefits.

## Methodology

In plain English: I took a large collection of job postings, kept only the ones in Finland, cleaned the data, and then counted, compared and charted what the postings say.

Data source. A dataset of roughly 2 million data-related job postings made up of job_postings_fact.csv and company_dim.csv (used for company names), plus a flattened version, job_postings_flat.csv, used for the skills analysis. The postings were published between 2023 and 2026. The fields used were job title, country, location, company, list of required skills, average yearly salary, posting date, and the flags for work-from-home, no degree mentioned and health insurance. Salary figures are the dataset's yearly salary averages (salary_year_avg), shown in euros as labelled in the charts.

### Tools. 
**Python**, **Pandas**, **Seaborn**, **Matplotlib**, **adjustText**, **Jupyter notebooks**.**Git&Github**

### Techniques.

- Filtering: Finland only. The location, company-pay and role-pay analysis covers seven titles: Data Analyst, Data Scientist, Data Engineer, Business Analyst and the senior versions of the first three. Skill demand covers Data Analyst, Data Engineer and Data Scientist. The salary distribution covers the six roles with the most salary-listed postings. The skill-pay analysis covers Data Analysts only.
- Cleaning: converted posting dates to date format, converted the skills column from text into real Python lists with ast.literal_eval, removed "Anywhere" locations, and dropped rows with no salary for any salary analysis.
- Joining: matched each posting to its company name through company_id, removing postings with no company name.
- Reshaping: "exploded" each skills list so every skill gets its own row, then grouped and counted by role.
- Measures: skill demand as a percentage of postings for a role, medians instead of averages for salary (so a few extreme salaries do not distort results), and box plots to show salary spread.
- Monthly pattern: a pivot table of skill mentions by calendar month, converted to percentages, for the 6 most mentioned skills.
- Demand versus pay: a scatter plot of demand against median salary, limited to skills in more than 5% of Data Analyst postings, with adjustText to keep labels readable.

![project Notebook Overview](jobs_Project\project_images\notebook_overview.png) 

### Limitations to keep in mind.
- Job postings show what employers advertise, not who was hired.
Only Data Analyst postings with a listed salary were used for skill pay, which is just 9 postings, and each skill appears in only 1 or 2 of them.
- Company and role medians come only from postings that list a salary, and the number per company is small. For example, Senior Data Scientists show a lower median than Data Scientists, which is a sign of small samples and not a real pattern.
- The monthly analysis combines all years into one calendar, so it shows a typical year, not a true time series. It measures each skill's share of all skill mentions, not its share of postings.
- The work-from-home and health-insurance flags were False for every Finland posting, which most likely means the data does not capture them.

## Findings and Insight

Insight 1: The capital region is where the jobs are

Helsinki has about 1,700 data-role postings, far ahead of Espoo with about 445 and Tampere with about 105. That makes Helsinki nearly four times the size of Espoo, and Espoo about four times the size of Tampere. Another 480 or so postings list only "Finland" with no city, so their true location is unknown. For job seekers, this means that being able to work in or near Helsinki gives access to a much larger share of the market than any other single factor in this dataset.

Top 10 locations for data jobs in Finland

Technical note: EDA_intro.ipynb

<details> <summary>View the visualisation code</summary>
Technical note:
[EDA_intro.ipynb](jobs_Project\EDA_intro.ipynb)

### visualization data 
```python
sns.barplot(
    data=location_counts,
    x='job_count',
    y='job_location',
    palette='Blues_r',
    hue='job_location',
    legend=False
)
plt.title('Top 10 Locations for Data Jobs in Finland')
plt.xlabel('Number of Job Postings')
plt.ylabel('Location')
plt.tight_layout()
plt.show()
```
![job_location_image](jobs_Project\project_images\job_location_image.png)

plt.show()
</details>

### Insight 2: 
Insight 2: The best-paying employers are industrial and tech firms, and a bank closes the top 10

Wärtsilä leads with a median of €136K, followed by Nortal (€131K) and Nitor (€121K), then Wolt (€111K) and Oura Health (€110K). At the other end of the top 10, ICEYE pays €88K and Nordea €86K. The top spot therefore pays about €50K, or more than 1.5 times, what the tenth-placed company pays. For a job seeker, a target list of well-paying employers is a good complement to simply applying wherever there is an opening, though each figure comes from a small number of salary-listed postings and should be checked against current ads.

Top 10 paying companies for analytics roles in Finland

Technical note: EDA_intro.ipynb

<details> <summary>View the visualisation code</summary>
[EDA_intro.ipynb](jobs_Project\EDA_intro.ipynb)

### visualization data 
```python
# Plot
plt.figure(figsize=(12, 8))

ax = sns.barplot(
    data=top_companies,
    x='median_salary',
    y='name',
    hue='name',
    palette='Blues_r',
    legend=False
)

# Add labels on bars
for container in ax.containers:
    labels = [f'${v/1000:.0f}K' for v in container.datavalues]

    ax.bar_label(
        container,
        labels=labels,
        padding=3
    )

# Title and labels
plt.title('Top 10 Paying Companies for Analytics Roles in Finland')
plt.xlabel('Median Salary (EURO)')
plt.ylabel('Company')

# Format x-axis
formatter = FuncFormatter(lambda x, pos: f'€{x/1000:.0f}K')
ax.xaxis.set_major_formatter(formatter)

plt.tight_layout()
plt.show()

```
![visualization of top paying companies](jobs_Project\project_images\company_pay_image.png)
</details>

### Insight 3: Seniority and specialisation both lift pay by about a fifth

Median yearly salary is €87K for Data Analysts and €106K for Senior Data Analysts, a step up of about €19K (22%) for experience in the same role. Moving sideways into data science brings a similar jump: Data Scientists earn €105K, about €18K (21%) more than Data Analysts. Business Analysts sit at €94K and Data Engineers at €97K.

![salary for roles](jobs_Project\project_images\median_yearly_salary_data_roles.png)

Median salary by data role in Finland

The spread chart shows Machine Learning Engineers highest, with a median near 140K, while Data Analyst salaries range widely, from about 45K to 125K with one outlier near 165K. For someone starting as an analyst, this shows both the entry point and two clear ways up. The one odd result, Senior Data Scientists at €88K, is most likely a small-sample effect.

Salary distributions of data jobs in Finland

Technical notes: EDA_intro.ipynb (role medians) and 3_salary_analysis.ipynb (salary distribution)

<details> <summary>View the visualisation code</summary>
[EDA_intro.ipynb](jobs_Project\EDA_intro.ipynb)

```python
# plot
plt.figure(figsize=(12, 8))

ax = sns.barplot(
    data=role_salary,
    x='salary_year_avg',
    y='job_title_short',
    palette='Greens_r',
    hue='job_title_short',
    legend=False
)

# Add value labels
for container in ax.containers:
    labels = [f'€{v/1000:.0f}K' for v in container.datavalues]

    ax.bar_label(
        container,
        labels=labels,
        padding=3
    )

plt.title('Top Paying Data Roles in Finland')
plt.xlabel('Median Salary (Euro)')
plt.ylabel('Job Title')

formatter = FuncFormatter(lambda x, pos: f'€{x/1000:.0f}K')
plt.gca().xaxis.set_major_formatter(formatter)

plt.tight_layout()
plt.show()
```

![visualization of roles salary](jobs_Project\project_images\role_pay_image.png)

### Insight 4: SQL and Python are everywhere, and each role adds its own specialty

SQL and Python are in the top two skills for all three roles. For Data Analysts they each appear in 43% of postings. For Data Engineers Python leads at 63% with SQL at 61%, and for Data Scientists Python reaches 63% while SQL sits at 44%. Beyond that foundation the roles split: analyst postings ask for business intelligence tools (Power BI at 26%, ahead of Tableau at 19%), engineer postings ask for cloud platforms (Azure 52%, AWS 33%, Databricks 24%), and scientist postings ask for R (28%). The practical message is to build the shared foundation first and choose a specialty second.

![Top skills for role](jobs_Project\project_images\top_skills_by_data_role.png)

Likelihood of skills requested in Finland job postings

Counts of skills requested in Finland job postings

Technical note: 1_skill_demand.ipynb

<details> <summary>View the visualisation code</summary>
salary distribution
Technical note::
[salary_analysis.ipynb](jobs_Project\3_salary_analysis.ipynb)

### visualization data 
```python
# plot
sns.boxplot(data=df_fin_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs Finland')
plt.xlabel('Yearly Salary (Euro)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'€{int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
![salary_distribution](jobs_Project/project_images/salary_distribution_image.png)


### Insight 5: 
The core skills do not change with the seasons

Python and SQL stay the two most mentioned skills in every month of the year. Python takes between about 26% and 35% of skill mentions, peaking in September, and SQL between about 19% and 29%, peaking in August. Azure holds steady at roughly 14% to 16.5%, while AWS is more volatile, falling from about 15% in January to under 8% in September and then jumping to about 18% in November, when it passes Azure. Power BI stays in a narrow band of about 7% to 10%. The impact is reassuring for learners: there is no best month to learn a skill, because the leaders do not move.

Top 6 skills in Finland job postings by month

Technical note: 2_skill_trend.ipynb, pivot table of calendar month × skill, normalised per month. Months combine all years in the data.

Skill trend
Technical note::
[2_skill_trend,ipynb](jobs_Project\2_skill_trend.ipynb)

### visualization data 
```python

# Plot
plt.figure(figsize=(12, 6))

sns.lineplot(
    data=df_plot_pct,
    linewidth=2
)

plt.title('Top 6 Skills in Finland Job Postings')
plt.xlabel('Month')
plt.ylabel('Percentage of Skill Mentions (%)')

plt.xticks(rotation=45)

plt.legend(
    title='Skills',
    bbox_to_anchor=(1.05, 1),
    loc='upper left'
)

plt.tight_layout()
plt.show()
```

![visualization skill trend](jobs_Project\project_images\skill_trend_images.png)

# Insight 6: 
High-paying skills exist, but the evidence is thin

Among the 9 Data Analyst postings with a salary, Azure (€165K), Go (€145K), Alteryx (€125K) and Looker (€115K) have the highest median pay. Azure and Alteryx each appear in only one posting, so their figures could simply reflect one well-paid job. Go and Looker each appear in two of the nine (22%) and are the only high-paying skills seen more than once, which makes them the most notable in the demand-versus-pay chart. Python, Power BI and Linux sit near €87K and Excel at €80K, below SQL and Tableau at €106K, but with one or two postings each these gaps should not be over-read. The honest takeaway is that tools beyond the basics (cloud, Alteryx, Looker) may carry a pay premium, and a larger salary sample is needed to confirm it.

Skill trend
Technical note::
[4_optimal_skill.ipynb](jobs_Project\4_optimal_skill.ipynb)

### visualization data 
```python
from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in Finland')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'€{int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```
![visualization optimal skill](jobs_Project\project_images\optimal_skill_image.png)


### visualization data 
```python
# plot
fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in the Finland')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'€{int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Most In-Demand Skills for Data Analysts in Finland')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (Euro)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'€{int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```
![visualization optimal skill salary](jobs_Project\project_images\optimal_skill_salary.png)

## Insight 7: 
Nearly half of postings do not mention a degree

In 45.9% of Finland data-role postings no degree is mentioned, while 54.1% do mention one. A mention is not the same as a strict requirement, but it suggests that for almost half of these roles, demonstrated skills and projects can matter as much as a diploma. The work-from-home and health-insurance flags were False for every posting, which looks like missing data, so this dataset cannot say anything reliable about those benefits.

Work from home, degree mention and health insurance in Finland postings

Skill trend
Technical note:
[EDA_intro.ipynb](jobs_Project\EDA_intro.ipynb)

### visualization data 
```python
# EDA 2 - Benefits & Requirements Analysis

# Count True and False values
work_from_home = df_fin['job_work_from_home'].value_counts()
job_no_degree = df_fin['job_no_degree_mention'].value_counts()
job_insurance = df_fin['job_health_insurance'].value_counts()

# Create figure
fig, ax = plt.subplots(1, 3, figsize=(12, 5))

# Work From Home
plt.subplot(1, 3, 1)
plt.pie(
    work_from_home,
    labels=work_from_home.index,
    autopct='%1.1f%%',
    startangle=90
)
plt.title('Work From Home Offered')

# No Degree Mention
plt.subplot(1, 3, 2)
plt.pie(
    job_no_degree,
    labels=job_no_degree.index,
    autopct='%1.1f%%',
    startangle=90
)
plt.title('Degree Requirement')

# Health Insurance
plt.subplot(1, 3, 3)
plt.pie(
    job_insurance,
    labels=job_insurance.index,
    autopct='%1.1f%%',
    startangle=90
)
plt.title('Health Insurance Offered')

plt.tight_layout()
plt.show()
```
![Benefits and requirement](jobs_Project\project_images\benefit_image.png)

## Recommendations
For job seekers and career changers
- Learn SQL and Python first. They are the top two skills for every role studied.
- Add Power BI next if you are aiming at analyst roles, since it is requested more often than Tableau (26% vs 19%).
- Consider Azure as a next step, particularly for engineering paths, where it appears in 52% of postings.
- Search in Helsinki and Espoo first, and open the roughly 480 postings that list only "Finland", since some may be remote or multi-location.
- Plan for progression: moving to a senior analyst role or into data science each lifts median pay by about a fifth.
- Build a portfolio of projects. With 45.9% of postings not mentioning a degree, visible proof of skills carries real weight.
- Use the top-paying company list as a starting point for targeted applications, and confirm pay in current postings.

### For students, bootcamps and training providers

- Make SQL and Python the core of any data curriculum, then offer role-based tracks: BI tools for analysts, cloud platforms for engineers, and R and statistics for scientists.

### For recruiters and employers

- Posting salary ranges and benefits clearly would help candidates compare roles. Very few postings here list a salary, and the benefit flags were empty.

### For the next stage of this project

-Collect a larger salary sample before drawing conclusions about skill pay.
- Add a hiring-volume view by company next to the pay view.
- Analyse each year separately (the data covers 2023 to 2026) to turn the monthly view into a true trend.
## Conclusions
- Where are the jobs? Mostly in the capital region. Helsinki leads (about 1,700 postings), then Espoo (about 445) and Tampere (about 105).
- Who pays the most? Wärtsilä (€136K), Nortal (€131K) and Nitor (€121K) lead on median salary, based on salary-listed postings.
- What do the roles pay? Median yearly pay is €87K for Data Analysts, €94K for Business Analysts, €97K for Data Engineers, €105K for Data Scientists and €106K for Senior Data Analysts.
- Which skills are asked for? SQL and Python lead for every role. Analysts add Power BI, engineers add Azure and AWS, and scientists add R.
- Does demand change over the year? No. Python and SQL are first and second in every month, so core skills are stable.
- Which skills pay and balance best for analysts? Azure, Go, Alteryx and Looker show the highest medians, with Go and Looker the strongest on demand and pay together. The sample is only 9 postings, so these are leads to investigate, not final answers.
- Degrees and benefits? 45.9% of postings do not mention a degree, and benefit data was not captured.

In one sentence: for a data career in Finland, a strong foundation in SQL and Python, plus Power BI for analyst roles, is the most evidence-backed place to start.
