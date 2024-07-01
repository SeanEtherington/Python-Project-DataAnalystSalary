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