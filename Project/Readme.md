# The Analysis

This project focuses on utilizing python libraries to create visualizations and analysis showcasing facets of the 2023 job market for the data science profession. It focuses on demand and skill representation in job listings to contextualize professions and differences in skill aspects. 

## 1.  What are the most demanded skills for the top 3 most popular data roles?

This analysis filtered for the top three data science rolls in the US job market to examine the demand and likelihood of each skill across job postings. In doing so this query focuses on the most popular job titles and the most requested skills for each roll.

#### Methodology 
1. Filter for roles and skills in the US job market.
2. Aggregate skill counts and skill percents for each roll.
3. Plot skill count and relative skill demand.

### Visualize Data

```python
fig, ax = plt.subplots(len(job_titles), 1)
for i, job_title in enumerate(job_titles):
    df_plot_percent=df_skills_percent[df_skills_percent['job_title_short']==job_title].head()
    sns.barplot(data=df_plot_percent, y='job_skills', x='skill_percent', ax=ax[i], hue='skill_count', palette='dark:b_r')

plt.show()
```

```python
fig, ax = plt.subplots(len(job_titles), 1)
for i, job_title in enumerate(job_titles):
    df_plot_percent=df_skills_percent[df_skills_percent['job_title_short']==job_title].head()
    sns.barplot(data=df_plot_percent, y='job_skills', x='skill_percent', ax=ax[i], hue='skill_count', palette='dark:b_r')
    ax[i].set_title(job_title)
    ax[i].set_ylabel('')
    ax[i].set_xlabel('')
    ax[i].set_xlim(0,78)
    ax[i].legend().remove()

    for n, v in enumerate (df_plot_percent['skill_percent']):
        ax[i].text(v+1,n,f'{v:.0f}%', va='center')


    if i !=len(job_titles)-1:
        ax[i].set_xticks([])

fig.suptitle('Likelihood of Skills Requested in US Job Postings', fontsize=16)
fig.tight_layout(h_pad=0.5)
plt.show()
```
### Results

![alt text](<Images/2_1_Demand for Top Skills in Job Postings by Title.png>)


![alt text](<Images/2_2_Likelihood of Skills Requested in US Job Postings.png>)

### Insights

- SQL leads as the net most desirable data science skill by demand with high requests across all three roles, returning as the most requested skill for Data Analysts (51%) and Data Engineers (68%), and the second most requested skill for Data Scientists (51%). 
- Python is a highly desirable skill, noted as the second most requested skill for Data Engineers (65%) and the most requested skill for Data Scientist (72%) across all job postings.
- Data Analyst skills appear more heterogenous with the most popular request (SQL) appearing in 51% of listings vs. Data Engineer's top two skills representing 68% and 65% of all listings and Data Scientist's top two demands returning at 72% and 51% of listings.

## 2. How are in-demand skills Trending for Data Analysts?

This query looks at the 2023 monthly trend of skill demand over time for Data Analysts in the United States job market. It further shows the relative demand of of each skill as a percentage of all listed skills during each time period to further highlight the strength of each skill's demand as a reflection of the demand pool.

#### Methodology 
1. Aggregate skills counts monthly
2. Re-analyze based on percentage of total jobs
3. Plot the monthly skill demand

### Visualize Data

```python
from matplotlib.ticker import PercentFormatter 
sns.lineplot(data=df_us_plot, dashes=False, palette='tab10')
sns.despine()
plt.title(f'Top {count} Trending Skills for {job4}s in {country4}')
plt.xlabel('2023')
plt.ylabel('Likelihood of Skill in Job Posting')
plt.legend().remove()


ax=plt.gca()
ax.yaxis.set_major_formatter(PercentFormatter(decimals=0))


for i in range(count):
    plt.text(11, df_us_plot.iloc[9, i], df_us_plot.columns[i])
plt.show()
```
### Results

![alt text](<Images/3_Likelihood of Skill in Job Posting.png>)

### Insights
- For Data Analysts in the US job market, SQL consistently remains as the highest in demand skill throughout the year, although as the year progresses, SQL shows a gradual demand decrease.
- Excel settles in as the second most sought skill with a gradual downward trend until August. However it remains at a significant threshold of demand above both Python and Tableau. 
- Both Python and Tableau show minor fluctuations as the year progresses but remain closely related in terms of overall demand.

## 3. How well do jobs and skills pay for Data Analysts?

This analysis further explores the market for Data Analyst but first explicates the salary distribution for the top six data science roles. Subsequently, the focus pivots to highlight the highest rewarded and most in demand skills for Data Analysts in the US job market sorted by median yearly salary. 

#### Methodology 
1. Evaluate median salary for top 6 data jobs
2. Find median salary per skill for Data Analysts
3. Visualize for highest paying skills and most demanded skills

### Salary Analysis for Data Nerds

### Visualize Data

```python
sns.boxplot(data=df_top6, x='salary_year_avg', y='job_title_short', order=job_order)
sns.set_theme(style='ticks')

plt.title(f'Salary Distribution in the {country5} ')
plt.xlabel('Yearly Salary ($USD)')
plt.ylabel('')
plt.xlim(0,600000)
ticks_x= plt.FuncFormatter(lambda y, pos: f'${int(y/1000)}K')
plt.gca().xaxis.set_major_formatter(ticks_x)
plt.show()
```
```python
fig, ax = plt.subplots(2,1, figsize=(7,6))
sns.set_theme(style='ticks')

sns.barplot(data=df_data_pay, x='median', y=df_data_pay.index, ax=ax[0], hue='median', palette='dark:b_r')
ax[0].set_title('Top 10 Highest Paid Skills for Data Analysts')
ax[0].set_ylabel('')
ax[0].set_xlabel('')
ax[0].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
ax[0].legend().remove()

sns.barplot(data=df_data_count, x='median', y=df_data_count.index, ax=ax[1], hue='median', palette='dark:b_r')
ax[1].set_title('Top 10 Most In-Demand Skills for Data Analysts')
ax[1].set_ylabel('')
ax[1].set_xlabel('Median Salary ($USD)')
ax[1].set_xlim(ax[0].get_xlim())
ax[1].xaxis.set_major_formatter(plt.FuncFormatter(lambda x, _: f'${int(x/1000)}K'))
ax[1].legend().remove()

fig.suptitle('Top Skills by Pay and Demand', fontsize=16)
fig.tight_layout()
```

### Results

![alt text](<Images/4_1_Salary Distribution in the {country5}.png>)

![alt text](<Images/4_2_Top Skills by Compensation and Demand.png>)

### Insights
- All senior rolls returned a higher median salary than their Jr. variant however Data Engineers and Data Scientists have a higher annual salary than Senior Data Analysts.
- 


## 4. What is the most optimal skill to learn for Data Analysts?

For this analysis, the focus is again on Data Analyst skills in the US job market. Using a categorical breakdown, the visualization highlights compensation vs. the relative percent strength of each skill against the skill market as a whole. 

#### Methodology
1. Group Skills to determine median salary and likelihood of being in posting
2. Visualize median salary vs percent skill demand
3. (Optional) Determine if certain technologies are more prevalent

### Visualize Data
```python
from adjustText import adjust_text
from matplotlib.ticker import PercentFormatter
sns.scatterplot(data=df_br, x='skill_percent', y='median_salary', hue='technology')
texts=[]
for i, txt in enumerate(df_skills_high_demand.index):
    
    texts.append(plt.text(df_skills_high_demand['skill_percent'].iloc[i], df_skills_high_demand['median_salary'].iloc[i], txt))
adjust_text(texts,  expand=(1.2, 2), arrowprops=dict(arrowstyle='->', color='grey'))


plt.xlabel('Percent of Data Analyst Jobs')
plt.ylabel('Median Yearly Salary ($USD)')
plt.title('Most Optimal Skills for Data Analysts in the US', fontsize=16)
ax=plt.gca()
ax.yaxis.set_major_formatter(plt.FuncFormatter(lambda y, pos:f'${int(y/1000)}K' ))
ax.xaxis.set_major_formatter(PercentFormatter(decimals=0))
plt.tight_layout()
plt.show()
```
### Results

![alt text](<Images/5_Most Optimal Skills for Data Analysts in the US.png>)

### Insights
- Python is a highly marketable skill being both financially rewarding and representing a significant volume of skill requests.
- Overall programming skills appear to yield a higher return and greater volume of skill requests compared to other technological subcategories.
- More specialized domains such as cloud and database skills while representing a smaller portion of the market net a greater fiscal compensation. 