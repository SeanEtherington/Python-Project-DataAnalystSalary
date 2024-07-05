# The Analysis

## 1. What are the most in demand skills for the top 3 most popular data roles?

To find the most in demand skills for the top 3 data roles. I filtered out those positions by which were the most pupular and then got the top 5 skills for these roles. This query highlights the most popular job titles and their top skills, showing which skills I should pay attention to depending on the role I am targeting.

You can view my notebook with detailed steps here:

[2_Project_Skill_demand.ipynb](Portfolio_project\2_Project_Skill_demand.ipynb)

### Visualising data

```python 
fig, ax = plt.subplots(len(job_titles), 1)


for i, job_title in enumerate(job_titles):
    df_plot = df_skills_percent[df_skills_percent['job_title_short']==job_title].head(5)
    sns.barplot(data=df_plot, x='skill_percent', y='job_skills', ax=ax[i], hue='skill_count', palette='dark:b_r', dodge=False, legend=False)

    plt.show()

```

### Results

![Visualisations of Top Job Skills - Data Analyst, Data Engineer, Senior Data Engineer](Portfolio_project\Jobskillsdemand.png)

### Insights

- SQL is an extremely versatile skill and highly in demand for all three roles, most prominently in Data Engineer (63%), Senior Data Engineer (64%) and lastly, Data Analysts (47%). 
- For Data Analysts, python, Power Bi, Excel and Tabeleay all had similar skill requirement percentages.
- Data Engineers and Senior Data Engineers require more specialised technical skills (azure, aws and spark) compared to Data Analysts who are expected to be more proficient in general data management and analysis tools (Excel, Power Bi).

## 2. How are in-demand skills trending for Data Analysts, Data Engineers, and Senior Data Engineers?

```python


# Plot for Data Analyst, Data Engineer, and Senior Data Engineer roles

sns.lineplot(data=df_plot,dashes = False, palette='tab10', ax=axes[0])
axes[0].set_title('Trending Top Skills for Data Analyst, Data Engineer, and Senior Data Engineer roles in Australia', fontsize=20)
axes[0].set_ylabel('Likelihood in Job Posting', fontsize=14)
axes[0].legend(df_plot.columns, loc='upper right')


```

### Results

![Trending Top Skills for Data Analysts, Data Engineers and Senior Data Engineers](Advanced_project\Trendingskills.png)

*Line graph visualising the trendng top skills for Data Analysts, Data Engineers and Senior Data Engineers in 2023.*

### Insights

- SQL consistently remains the top skill, with Python following closely, indicating their crucial role in data-related job postings throughout the year.
- Fluctuating Cloud Skills: Azure and AWS show variable demand, with noticeable dips and recoveries, highlighting the dynamic nature of cloud technology requirements in job postings.

*Note:*
The total number of Data Analyst roles in United States were 14086 compared to 456 in Australia. 
Having Data Analyst, Data Engineer, and Senior Data Engineer roles all in the same graph is why there is a high percentage of cloud skills such as Azure and AWS present in the graph and not fundamental data tools such as Tableau and Excel. 

```python

# Print the counts
print(f"Total number of Data Analyst roles in United States: {count_DA_US}")
print(f"Total number of Data Analyst roles in Australia: {count_DA_AUS}")
print(f"Total number of Data Analyst, Data Engineer, and Senior Data Engineer roles in Australia: {count_DA_DE_SDE_AUS}")


```

## 3. How well do jobs and skills pay for Data roles in the United States?

*Note:* Graphing salaries for Australian roles was too inaccurate due to the sample size. After dropping N/A salary values, there were only 50 entries for the top 5 data roles in Australia compared to 12000 entries in the United States.

```python


job_counts = df_US_top5['job_title_short'].value_counts()

print("Number of jobs for each role:")
print(job_counts)

job_counts_AU = df_AUS_top5['job_title_short'].value_counts()

# Print the number of jobs for each role
print("Number of jobs for each role:")
print(job_counts_AU)


Australia:

Number of jobs for each role:

Data Engineer                30
Senior Data Engineer          9
Data Scientist                7
Machine Learning Engineer     7
Software Engineer             6


United States:

Number of jobs for each role:

Data Scientist           4553
Data Analyst             4350
Data Engineer            2915
Senior Data Scientist    1241
Senior Data Engineer     1058 

```

### Salary Analysis 
#### Visualising the Data
```python
sns.boxplot(data=df_US_top5, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title('Salary Distributions in United States')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0, 500000)
```

![Salary Distributions of Data Jobs in the United States](Portfolio_project\salary.png)

## Results
- There's a significant variation in salary ranges across job titles. Senior Data Scientist posisionts tend to have the highest salary potential, up to 500k, indicating the high value placed on advanced data skills and experience in the industry. 

- Senior Data Engineer and Senior Data Scientist roles show a considerable number of outliers on the higher end of the salary range, suggesting that exceptional skills or working circumstances cnal lead to high pay in these roles. In contrast, Data Analyst roles demonstrate more consistent in salary with fewer outliers.

- The median salaries increase with the seniority and specialisation of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only have higher median salaries but also larger ranges, reflecting greater variance in compensation as responsibilities increase. 

### Highest Paid and Most in Demand Skills for Data Analysts
#### Visualising the data

```python

# Top 10 most Highest paying Skills for Data Analysts 
sns.barplot(data = df_DA_top_pay, x = 'median', y = df_DA_top_pay.index, ax=ax[0], hue = 'median', palette = 'dark:b')
ax[0].legend().remove()


# Top 10 most in Demand Skills for Data Analysts 
sns.barplot(data = df_DA_skills, x = 'median', y = df_DA_skills.index, ax=ax[1], hue = 'median', palette = 'light:b') 
ax[1].legend().remove()

plt.show()
```
![The Highest Paid and Most In-Demand Skills for Data Analysts in the United States](Portfolio_project\skillsindemandandpay.png)

#### Insights:

- The top graph shows specialised technical skills like 'dplyr', 'Bitbucket', and 'Github' being associated with higher salaries, some reaching up to 200k, suggesting that advanced technical proficiency can increase earning potential.

- The second graph highlights that foundational skills like 'Excel', 'Powerpoint', and 'SQL' are the most in demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analyt roles. 
- There is a clear distinction between skills that are the highest paid and that are most in-demand. Data analysts aiming to maximise their career potential should consider developing a diverse skill set that includes both higher paying specialised skills and highly demanded foundational skills. 