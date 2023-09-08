# Messi's Goals Analysis
![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/intro_image.jpg)
## Introduction
This is my first data analytics project and I decided to analyse Messi's goals throughout his career. The dataset can be found here: [Messi's Goals](https://www.kaggle.com/datasets/azminetoushikwasi/-lionel-messi-all-club-goals). The aim of this project was to familiarise myself with the pandas, seaborn and matplotlib libraries. Note this is an overview of the project and the relevant Jupyter notebook is attached.
## Relevant libraries
Python - pandas, seaborn, matplotlib and numpy
## Problem
1) What was Messi's top performing season?
2) In every season, which league did Messi score the most goals in?
3) What position does Messi excel at the most?
## Data Analysis
**Null Values** \
\
Firstly, I try to get an idea of any missing values present in the dataset.
```python
sns.heatmap(df.isnull(), cmap = 'viridis', cbar = False)
```
![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/null_heatmap.png)

Fortunately most of the data is present.

**Imputation**\
\
I noticed that some cells weren't formatted correctly hence I had to reformat some cells.

```python
df['Season'].replace(["11-Dec","Dec-13"],["11/12","12/13"], inplace = True)
df['Competition'].replace(['Troph�e des Champions','Champions League'],['Trophée des Champions','UEFA Champions League'], inplace = True)
```

## Data Visualisation

To answer the first question, I created a new dataframe to count the goals scored per season.

```python
goals_df = df['Season'].value_counts(sort = False).reset_index()
goals_df.columns = ['Season','Goals'] 
```
Now using this dataframe I made a visual to find out Messi's top performing season.
```python
plt.figure(figsize=(14,6))
plt.title('Goals Per Season')
sns.barplot(x = goals_df['Season'], y = goals_df['Goals'])
```

![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/goals_per_season.png)

Similarly, I created a dataframe called comp_df to use for the second question.

```python
comp_df = df.groupby(['Season','Competition']).agg(Goals = ('Season','count')).reset_index()
```

With this new dataframe I then made this stacked visual to show the goals per competition.

```python
pivot_df = comp_df.pivot_table(index='Season', columns='Competition', values='Goals')
pivot_df.plot(kind='bar', stacked=True, figsize = (14, 7), title = 'Goals Per Season Per Competition')
```

![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/stacked_bar.png)

I also created a pie chart to get a different perspective.

```python
def func(pct, allvalues):
    absolute = int(pct / 100.*np.sum(allvalues))
    return "{:.1f}%\n({:d} Goals)".format(pct, absolute)

plt.figure(figsize = (14, 7))
competition = df['Competition'].value_counts().reset_index()
competition.columns = ['Competition','Goals']
plt.pie(competition['Goals'], explode = (0,0.1,0.2,0.3,0.4,0.6,0.8,1),labels = competition['Competition'], autopct = lambda pct: func(pct, competition['Goals']),shadow = True)
plt.title('Goal Distribution Across Competitions')
plt.show()
```
![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/pie.png)

To answer the last question, the following visual was created.

```python
sns.set(style="darkgrid")
sns.countplot(x = 'Playing_Position', data = df, palette = 'cividis', order = df['Playing_Position'].value_counts().index)
```

![](https://github.com/chekebh/Messi-Goals-Analysis/blob/main/position.png)

## Conclusion
Using the first visual, it can be concluded that the **2011-2012 season** was Messi's top performing season.

The next two visuals shows that **LaLiga** was Messi's top scoring league.

Lastly, Messi's favoured position was **Centre Forward**.

