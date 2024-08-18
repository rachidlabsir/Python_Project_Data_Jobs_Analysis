# Overview
Welcome to my analysis of the data job market, specifically focusing on data analyst roles. This project aims to provide a clearer understanding of the current job landscape, highlighting top-paying positions and in-demand skills. By delving into these areas, I hope to help data analysts identify the most promising job opportunities and navigate the market more effectively.

The data for this analysis is sourced from [Luke Barousse's webiste](https://huggingface.co/datasets/lukebarousse/data_jobs), which offers a comprehensive foundation, including detailed information on job titles, salaries, locations, and essential skills. Using a series of Python scripts, I examine critical questions such as the most sought-after skills, salary trends, and the relationship between demand and salary in the field of data analytics.

# The Questions
In this project, I aim to address the following key questions:

    1-What are the most in-demand skills for the top three most popular data roles?
    2-How are in-demand skills evolving for Data Analysts?
    3-How do salaries and job opportunities correlate with skills for Data Analysts?
    4-What are the optimal skills for Data Analysts to acquire in terms of both high demand and high pay?

# Tools I Used

Python: The core of my analysis, enabling me to extract critical insights from the data. Key Python libraries used include:

    Pandas: For data manipulation and analysis.
    Matplotlib: For creating basic data visualizations.
    Seaborn: For generating more advanced and aesthetically pleasing visuals.

Jupyter Notebooks: The platform I used to run Python scripts, which allowed me to seamlessly integrate notes and analysis.
Visual Studio Code: My preferred editor for executing Python scripts.
Git & GitHub: Essential tools for version control, project tracking, and collaboration, ensuring efficient code management and sharing.
# Import & Clean Up Data

I start by importing necessary libraries and loading the dataset, followed by initial data cleaning tasks to ensure data quality.
```python
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

# Filter US Jobs
To focus my analysis on the U.S. job market, I apply filters to the dataset, narrowing down to roles based in the United States.
```python
df_US = df[df['job_country'] == 'United States']
```
# The Analysis

Each Jupyter Notebook in this project was designed to investigate specific aspects of the data job market. Here’s how I 
approached each question:

## 1-What are the most demanded skills for the top 3 most popular data roles?

To determine the most demanded skills for the top 3 data roles, I first identified the most popular positions. Then, I extracted the top 5 skills associated with each of these roles. This analysis highlights the key skills for the most sought-after job titles, helping me focus on the skills that are crucial for the roles I am targeting.
View my notebook with detailed steps here: [2_Skill_Demand](https://github.com/rachidlabsir/project_data_jobs_analysis/blob/main/2_Skill_Demand.ipynb)
### Visualize Data
```python


fig, ax = plt.subplots(len(job_titles),1)
sns.set_theme(style = 'ticks')
for i ,job_title in enumerate(job_titles):
   df_plot = df_skills_perc[df_skills_perc['job_title_short'] == job_title].head(5)
   sns.barplot(data = df_plot , x='skills_perc',y='job_skills',hue = 'job_skills' ,ax = ax[i], palette = 'dark:b_r') 
   ax[i].set_title(job_title)
   ax[i].set_ylabel('')
   ax[i].set_xlabel('')
   ax[i].set_xlim(0, 78)
   if i == len(job_titles) -1:
     ax[i].set_xticks([])   
    
   for n, v in enumerate(df_plot['skills_perc']):
        ax[i].text(v + 1, n, f'{v:.0f}%', va='center')
fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=15)
fig.tight_layout(h_pad=0.5) # fix the overlap
plt.show()
```
![Untitled](https://github.com/user-attachments/assets/3522eb8a-6f46-41e8-96bd-0ff4fa92d8f4)
Bar graph visualizing the salary for the top 3 data roles and their top 5 skills associated with each.
### Insights:
-SQL is the most requested skill for both Data Analysts and Data Scientists, appearing in over half of job postings for these roles. In contrast, Python is the most sought-after skill for Data Engineers, appearing in 68% of job postings.

-Data Engineers require more specialized technical skills, such as AWS, Azure, and Spark, whereas Data Analysts and Data Scientists are expected to be proficient in more general data management and analysis tools, including Excel and Tableau.

-Python is a highly versatile skill, in demand across all three roles, with the highest prominence in Data Scientists (72%) and Data Engineers (65%).

## 2-How are in-demand skills evolving for Data Analysts?

To analyze how skills for Data Analysts have trended in 2023, I filtered job postings for Data Analyst positions and grouped the skills by month. This analysis identified the top 5 skills each month, revealing how the popularity of these skills varied throughout the year.
You can view my detailed analysis in the notebook here: [3_Skills_Trend](https://github.com/rachidlabsir/project_data_jobs_analysis/blob/main/3_Skills_Trend.ipynb)

### Visualize Data
```python

from matplotlib.ticker import FuncFormatter
df_plot = df_DA_US_percent.iloc[:,:5]
sns.lineplot(data=df_plot, dashes=False, legend='full', palette='tab10')
sns.set_theme(style='ticks')
sns.despine() # remove top and right spines

plt.title('Trending Top Skills for Data Analysts in the US')
plt.ylabel('Likelihood in Job Posting')
plt.xlabel('2023')
plt.legend().remove()
ax =plt.gca()
ax.yaxis.set_major_formatter(FuncFormatter(lambda y, _: f'{y:.0f}%'))
for i in range(5):
  plt.text(11.2,df_plot.iloc[-1,i],df_plot.columns[i],color = "black")

```
![Untitled](https://github.com/user-attachments/assets/49485fdb-83e9-4f38-bfa7-7442e697a065)
Bar graph visualizing the trending top skills for data analysts in the US in 2023.
### Insights:
-SQL remains the most consistently demanded skill throughout the year, though it exhibits a gradual decrease in demand.

-Excel saw a significant rise in demand starting around September, ultimately surpassing both Python and Tableau by the end of the year.

-Python and Tableau demonstrated relatively stable demand throughout the year, with some fluctuations, but continued to be essential skills for data analysts.

-Power BI, while less in demand compared to the other skills, showed a slight upward trend toward the end of the year.

## 3-How do salaries and job opportunities correlate with skills for Data Analysts?
To identify the highest-paying roles and skills, I focused on job postings in the United States and analyzed their median salaries. Initially, I examined the salary distributions for common data roles such as Data Scientist, Data Engineer, and Data Analyst to determine which positions command the highest pay.

You can view the detailed analysis in my notebook here: [4_Salary_Analysis](https://github.com/rachidlabsir/project_data_jobs_analysis/blob/main/4_Salary_Analysis.ipynb)

### Visualize Data:
```python


sns.boxplot(data=df_US_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')
sns.despine()

# this is all the same
plt.title('Salary Distributions of Data Jobs in the US')
plt.xlabel('Yearly Salary (USD)')
plt.ylabel('')
plt.xlim(0, 600000) 
ticks_x = plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
![Untitled](https://github.com/user-attachments/assets/32e900e7-b611-4b3b-9232-d40ef2de12be)

Box plot visualizing the salary distributions for the top 6 data job titles.

-There is significant variation in salary ranges across different job titles. Senior Data Scientist positions have the highest salary potential, reaching up to $600K, highlighting the premium placed on advanced data skills and extensive experience.

-Senior Data Engineer and Senior Data Scientist roles show a considerable number of high-end salary outliers, suggesting that exceptional skills or unique circumstances can lead to substantial compensation. In contrast, Data Analyst roles exhibit more consistent salaries, with fewer outliers.

-Median salaries increase with seniority and specialization. Senior roles (Senior Data Scientist, Senior Data Engineer) not only command higher median salaries but also exhibit a greater range in typical salaries, reflecting the increased variance in compensation as responsibilities grow.

## Highest Paid & Most Demanded Skills for Data Analysts
Next, I narrowed my analysis and focused only on data analyst roles. I looked at the highest-paid skills and the most in-demand skills. I used two bar charts to showcase these.
### Visualize Data
```python

fig, ax = plt.subplots(2, 1)  

# Top 10 Highest Paid Skills for Data Analysts
sns.barplot(data=df_DA_top_pay, x='median', y=df_DA_top_pay.index, hue='median', ax=ax[0], palette='dark:b_r')
ax[0].legend().remove()
# original code:
# df_DA_top_pay[::-1].plot(kind='barh', y='median', ax=ax[0], legend=False) 
ax[0].set_title('Highest Paid Skills for Data Analysts in the US')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))


# Top 10 Most In-Demand Skills for Data Analysts')
sns.barplot(data=df_DA_skills, x='median', y=df_DA_skills.index, hue='median', ax=ax[1], palette='light:b')
ax[1].legend().remove()
# original code:
# df_DA_skills[::-1].plot(kind='barh', y='median', ax=ax[1], legend=False)
ax[1].set_title('Most In-Demand Skills for Data Analysts in the US')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary (USD)')
ax[1].set_xlim(ax[0].get_xlim())  # Set the same x-axis limits as the first plot
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))

sns.set_theme(style='ticks')
plt.tight_layout()
plt.show()
```
Here's the breakdown of the highest-paid & most in-demand skills for data analysts in the US:
![Untitled](https://github.com/user-attachments/assets/99e037f7-3103-4595-b83f-27e8655525c6)
highest paid skills and most in-demand skills for data analysts in the US

## Insights:

-The top graph indicates that specialized technical skills, such as dplyr, Bitbucket, and Gitlab, are linked to higher salaries, with some reaching up to $200K. This suggests that advanced technical expertise can significantly enhance earning potential.

-The bottom graph shows that foundational skills like Excel, PowerPoint, and SQL are the most in-demand, though they may not offer the highest salaries. This underscores the importance of these core skills for securing employment in data analysis roles.

-There is a clear distinction between the highest-paying skills and the most in-demand skills. Data analysts aiming to maximize their career potential should focus on developing a diverse skill set that includes both high-paying specialized skills and essential foundational skills.

##  4-What are the optimal skills for Data Analysts to acquire in terms of both high demand and high pay?
To identify the most optimal skills to learn—those that are both the highest paid and most in demand—I calculated the percentage of demand and the median salary for these skills. This analysis helps pinpoint which skills offer the best balance of compensation and job market relevance.

You can view the detailed steps in my notebook here: [5_Optimal_Skills](https://github.com/rachidlabsir/project_data_jobs_analysis/blob/main/5_Optimal_Skills.ipynb)

### Visualize Data:
```python

from adjustText import adjust_text

plt.scatter(df_DA_skills_high_demand['skill_percent'], df_DA_skills_high_demand['median_salary'])
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Salary ($USD)')  # Assuming this is the label you want for y-axis
plt.title('Most Optimal Skills for Data Analysts in the US')

# Get current axes, set limits, and format axes
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))  # Example formatting y-axis

# Add labels to points and collect them in a list
texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i], " " + txt))

# Adjust text to avoid overlap and add arrows
adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))

plt.show()
```
![Untitled](https://github.com/user-attachments/assets/28c351e1-c116-4908-a3c1-035253696b19)
 A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US.

### Insights:

-The skill Oracle has the highest median salary at nearly $97K, despite its lower frequency in job postings. This indicates a high value placed on specialized database skills in the data analyst field.

-More commonly required skills like Excel and SQL appear frequently in job listings but offer lower median salaries compared to specialized skills such as Python and Tableau, which command higher salaries and are moderately prevalent.

-Skills like Python, Tableau, and SQL Server are at the higher end of the salary spectrum and are relatively common in job listings. This suggests that proficiency in these tools can lead to favorable opportunities in data analytics.

## Visualizing Different Technologies

Let's visualize the various technologies using a graph. We'll incorporate color labels to distinguish between different technologies (e.g., Programming: Python).

## Visualize Data:
```python
from adjustText import adjust_text
plt.figure(figsize=(12, 6))
sns.scatterplot(
    data=df_DA_skills_tech_high_demand,
    x='skill_percent',
    y='median_salary',
    hue='technology'
)

sns.despine()
sns.set_theme(style='ticks')

texts = []
for i, txt in enumerate(df_DA_skills_high_demand.index):
    texts.append(plt.text(df_DA_skills_high_demand['skill_percent'].iloc[i], df_DA_skills_high_demand['median_salary'].iloc[i]," "+ txt))

adjust_text(texts, arrowprops=dict(arrowstyle='->', color='gray'))
plt.tight_layout()


# Set axis labels, title, and legend
plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary')
plt.title('Most Optimal Skills for Data Analysts in the US')
plt.legend(title='Technology')

from matplotlib.ticker import PercentFormatter
ax = plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K'))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))

plt.tight_layout()
```
![Untitled](https://github.com/user-attachments/assets/cbad3a29-cbf3-44fb-b463-858acc06008e)
 A scatter plot visualizing the most optimal skills (high paying & high demand) for data analysts in the US with color labels for technology.
 
### Insights:
-The scatter plot reveals that most programming skills (colored blue) tend to cluster at higher salary levels compared to other categories. This suggests that expertise in programming may offer greater salary benefits within the data analytics field.

-Database skills (colored orange), such as Oracle and SQL Server, are associated with some of the highest salaries among data analyst tools. This indicates strong demand and high value for data management and manipulation expertise in the industry.

-Analyst tools (colored green), including Tableau and Power BI, are prevalent in job postings and offer competitive salaries. This highlights the importance of visualization and data analysis software in current data roles. This category not only provides good salary prospects but is also versatile across various data tasks.

# What I Learned

-Throughout this project, I gained a deeper understanding of the data analyst job market and enhanced my technical skills in Python, particularly in data manipulation and visualization. Here are some key takeaways:

-Advanced Python Usage: I utilized libraries such as Pandas for data manipulation and Seaborn and Matplotlib for data visualization. These tools enabled me to perform complex data analysis tasks more efficiently.

-Data Cleaning Importance: I learned that thorough data cleaning and preparation are essential before conducting any analysis. Proper data cleaning ensures the accuracy of the insights derived from the data.

-Strategic Skill Analysis: The project highlighted the importance of aligning one's skills with market demand. Understanding the relationship between skill demand, salary, and job availability facilitates more strategic career planning in the tech industry.

#Insights

This project revealed several key insights into the data job market for analysts:

-Skill Demand and Salary Correlation: There is a clear correlation between the demand for specific skills and the salaries they command. Advanced and specialized skills, such as Python and Oracle, often lead to higher salaries.

-Market Trends: Skill demand trends are continually evolving, reflecting the dynamic nature of the data job market. Staying updated with these trends is crucial for career advancement in data analytics.

-Economic Value of Skills: Identifying skills that are both in-demand and well-compensated helps guide data analysts in prioritizing their learning to maximize economic returns.

# Challenges I Faced

This project presented several challenges, each offering valuable learning opportunities:

-Data Inconsistencies: Managing missing or inconsistent data entries required careful handling and thorough data-cleaning techniques to maintain the integrity of the analysis.

-Complex Data Visualization: Creating effective visual representations of complex datasets was challenging but essential for clearly and compellingly conveying insights.

-Balancing Breadth and Depth: Striking the right balance between diving deeply into each analysis and maintaining a broad overview of the data landscape required constant adjustment to ensure comprehensive coverage without losing focus on details.

# Conclusion
This exploration of the data analyst job market has been highly informative, shedding light on the critical skills and trends that define this evolving field. The insights gained enhance my understanding and offer actionable guidance for advancing a career in data analytics. As the market continues to evolve, ongoing analysis will be crucial for staying ahead. This project provides a solid foundation for future investigations and emphasizes the importance of continuous learning and adaptation in the data field.





'


