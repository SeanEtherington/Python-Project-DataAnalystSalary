# Overview

Welcome to my analysis of the data job market, which focused on primarily on data analyst roles but also Data Engineer and Senior Data Engineer positions. This project was created out of a desire to better navigate and understand the job market more effectively. Specifically, I looked into the top-paying and in-demand skills to help find optimal job opportunities for data analysts.

The data was sourced from Luke Barousse's Python course, which provided a foundation for my analysis and contained information on job titles, salaries, locations, and essential skills. Through a series of Python scripts, I explored key questions such as the most in-demand job skills, salary trends, and the relationship between demand and salary in data analytics.

# The Questions
Below are the questions I want to answer in my project:

1. What are the skills most in demand for the top 3 most popular data roles?
2. How are in-demand skills trending for Data Analysts?
3. How well do jobs and skills pay for Data Analysts?
4. Wha are the optimal skills for data analysts to learn?

*Note:* For questions 3 and 4, I decided to focus my analysis on the United States instead of Australia due to the larger sample size. This approach allowed me to gather more descriptive and detailed insights. While I could have grouped job titles together as I did for questions 1 and 2, I found the results too misleading because the skills required for different job titles varied significantly.

# Tools I Used
For this project, I harnessed the power of several key tools:

**Python:** The backbone of my analysis, allowing me to analyze the data and find critical insights. I also used the following Python libraries:
- **Pandas Library:** This was used to analyze the data.
- **Matplotlib Library:** Helped with visualising the data.
- **Seaborn Library:** Helped create more advanced visuals.

**Jupyter Notebooks:** The tool I used to run my Python scripts which let me easily include my notes and analysis.

**Visual Studio Code:** My go-to for executing my Python scripts.

**Git & GitHub:** Essential for version control and sharing my Python code and analysis, ensuring collaboration and project tracking.

# Data Preparation and Cleanup
This section outlines the steps taken to prepare the data for analysis, ensuring accuracy and usability.

## Import and Clean Up Data 
I started by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
``` python
# Importing Libraries
import ast
import pandas as pd
import seaborn as sns
from datasets import load_dataset
import matplotlib.pyplot as plt  

# Loading Data
dataset = load_dataset('lukebarousse/data_jobs')
df = dataset['train'].to_pandas()

# Data Cleanup
df['job_posted_date'] = pd.to_datetime(df['job_posted_date'])
df['job_skills'] = df['job_skills'].apply(lambda x: ast.literal_eval(x) if pd.notna(x) else x)
```
## Filter United States/Australian jobs

# The Analysis

To focus my analysis on the United States job market, I applied filters to the dataset, narrowing down to roles based in the United States/Australia.

```python
df_US = df[df['job_country'] == 'United States']
#OR
df_AUS = df[df['job_country'] == 'Australia']

```

## 1. What are the most in demand skills for the top 3 most popular data roles in Australia?

To find the most in-demand skills for the top three data roles, I filtered out the positions that were the most popular and then identified the top five skills for these roles. This query highlighted the most popular job titles and their top skills, showing which skills I should have focused on depending on the role I was targeting.

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

- SQL was an extremely versatile skill and highly in demand for all three roles, most prominently in Data Engineer (63%), Senior Data Engineer (64%), and lastly, Data Analysts (47%).
- For Data Analysts, Python, Power BI, Excel, and Tableau all had similar skill requirement percentages.
- Data Engineers and Senior Data Engineers required more specialized technical skills (Azure, AWS, and Spark) compared to Data Analysts, who were expected to be more proficient in general data management and analysis tools (Excel, Power BI).

## 2. How are in-demand skills trending for Data Analysts, Data Engineers, and Senior Data Engineers in Australia?

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
- There was a significant variation in salary ranges across job titles. Senior Data Scientist positions tended to have the highest salary potential, up to 500k, indicating the high value placed on advanced data skills and experience in the industry.

- Senior Data Engineer and Senior Data Scientist roles showed a considerable number of outliers on the higher end of the salary range, suggesting that exceptional skills or working circumstances could lead to high pay in these roles. In contrast, Data Analyst roles demonstrated more consistency in salary with fewer outliers.

- The median salaries increased with the seniority and specialization of the roles. Senior roles (Senior Data Scientist, Senior Data Engineer) not only had higher median salaries but also larger ranges, reflecting greater variance in compensation as responsibilities increased.

### Highest Paid and Most in Demand Skills for Data Analysts
#### Visualising the Data

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

- The top graph shows specialised technical skills like 'dplyr', 'Bitbucket', and 'Gitlab' being associated with higher salaries, some reaching up to 200k, suggesting that advanced technical proficiency can increase earning potential.

- The second graph highlights that foundational skills like 'Excel', 'Powerpoint', and 'SQL' are the most in demand, even though they may not offer the highest salaries. This demonstrates the importance of these core skills for employability in data analyt roles. 
- There is a clear distinction between skills that are the highest paid and that are most in-demand. Data analysts aiming to maximise their career potential should consider developing a diverse skill set that includes both higher paying specialised skills and highly demanded foundational skills. 

## 4. What is the most optimal skill to learn for Data Analysts in the United States?

#### Visualising the data
``` python
sns.scatterplot(
    data=df_DA_skills_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')
```
![Most Optimal Skills for Data Analysts in the United States](Portfolio_project\Optimalskills.png)

*A scatter plot visualising the most optinal skills (high paying and high demand) for data analysts in the United States*

#### Insights:
- The scatter plot shows the most of the 'programming' skills (coloured blue) tended to cluster at higher salary levels compared to other categories, indicating that programming expertise might offer greater salary ranges within the data analytics field.
- Analyst tools (coloured green), including Tableau and Power Bi, are prevalent in job postings and offer competitive salaries, showing that visualising and data analysis software are crucial for current data roles. This category not only has a good salary range but is also versatile across different types of data tasks.
- The database skills (coloured orange), such as Oracle and SQL server, are associated with some of the highest salaries among data analyst tools. This indicates a significant demand and valuation for data management and manipulation expertise in the industry. 

# What I Learned


Throughout this project, I deepened my understanding of the data analyst job market and honed my technical skills in Python, particularly in data manipulation and visualization. Here are several key insights I gained:

- **Advanced Python Usage**: I leveraged libraries like Pandas for proficient data manipulation and Seaborn and Matplotlib for creating insightful visualizations, enabling more sophisticated data analysis.
- **Data Cleaning Importance:** I learned the critical importance of meticulous data cleaning and preparation. This foundational step ensures the accuracy and reliability of insights derived from the data.
- **Holistic Skill Assessment:** The project underscored the strategic value of aligning skillsets with market demand. By analyzing the interplay between skill requirements, salary trends, and job opportunities, I gained valuable insights for strategic career planning in the tech industry.
- **Effective Communication of Findings:** I improved my ability to communicate complex data findings to diverse stakeholders, enhancing the impact of my analytical work within organizational contexts.
# Insights
This project provided several general insights into the data job market for analysts:

- **Skill Demand and Salary Correlation:** There is a clear correlation between the demand for specific skills and the salaries these skills command. Advanced and specialized skills like Python and Oracle often lead to higher salaries.
- **Market Trends:** There are changing trends in skill demand, highlighting the dynamic nature of the data job market. Keeping up with these trends is essential for career growth in data analytics.
- **Economic Value of Skills:** Understanding which skills are both in-demand and well-compensated can guide data analysts in prioritizing learning to maximize their economic returns.
# Challenges I Faced
This project was not without its challenges, but it provided good learning opportunities:

- **Data Inconsistencies:** Handling missing or inconsistent data entries requires careful consideration and thorough data-cleaning techniques to ensure the integrity of the analysis.
- **Complex Data Visualization:** Designing effective visual representations of complex datasets was challenging but critical for conveying insights clearly and compellingly.
- **Balancing Breadth and Depth:** Deciding how deeply to dive into each analysis while maintaining a broad overview of the data landscape required constant balancing to ensure comprehensive coverage without getting lost in details.
# Conclusion
This exploration of the data analyst job market was highly informative, revealing the essential skills and trends that shaped this dynamic field. The insights I gained deepened my understanding and offered practical guidance for those seeking to advance their careers in data analytics. As the market continued to evolve, ongoing analysis was crucial to staying ahead in the field. This project provided a solid foundation for future inquiries and emphasized the importance of continuous learning and adaptation in the data industry.
