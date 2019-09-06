# 01-Data Analysis

__Case-01.__ Wine Quality Dataset
  - package: Pandas, Numpy, Matplotlib, Seaborn, 
  - func:
-------------------------------------------------------------------------------------------------------------------------------------  
__Data:__ Wine Quality Data Set (https://archive.ics.uci.edu/ml/datasets/Wine+Quality/)
2 datasets are included, related to red and white "vinho verde wine" samples, from the north of Portugal. Each wine sample comes with a quality rating from 1 to 10, and results from several physical chemical test. There are 11 columns describing chemical properties as follows. If we were the owner of a vineyard, how might we use it? 

Input variables (based on physicochemical tests): 
  - 1 - fixed acidity 
  - 2 - volatile acidity 
  - 3 - citric acid 
  - 4 - residual sugar 
  - 5 - chlorides 
  - 6 - free sulfur dioxide 
  - 7 - total sulfur dioxide 
  - 8 - density 
  - 9 - pH 
  - 10 - sulphates 
  - 11 - alcohol 

Output variable (based on sensory data): 
  - 12 - **quality** (score between 0 and 10)

## Investigation:__ What chemical charateristics are most important in predicting the quality of wine? 

### 1> import the dataset
```
df_red = pd.read_csv('C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L3/case01/data/winequality-red.csv', sep=';')
df_red.info()
df_red.head(10)
df_red.describe()
df_white = pd.read_csv('C:/Users/Minkun/Desktop/classes_1/NanoDeg/1.Data_AN/L3/case01/data/winequality-white.csv', sep=';')
df_white.info()
df_white.head(10)
df_white.describe()
```
<img src="https://user-images.githubusercontent.com/31917400/33953018-7e646d10-e02b-11e7-92a2-7404bf419d7a.jpg" />
<img src="https://user-images.githubusercontent.com/31917400/33953024-82897f48-e02b-11e7-9ba6-b98f69e09d52.jpg" />

### 2> wrangle the dataset

#### *How many duplicate rows are in the dataset ? 
```
sum(df_white.duplicated())
```
#### *How to drop the duplicate rows ?
```
df_white.drop_duplicates(inplace=True)
```
#### *How many rows with missing values?
```
df_white.isnull().sum(axis=0)
```
#### *How to drop the rows with missing values?
```
df_white.dropna(axis=0, how='any', inplace=True)
```
#### *How to fill the rows with missing values with 0 or mean?
```
df_white.fillna(0, inplace=True)
df_white['column'].fillna(df_white['column'].mean(), inplace=True)
df_white.apply(lambda x: x.fillna(x.mean(), inplace=True), axis=0)
```
#### *How many unique values in 'quality' (**categorical response variable**) column and what are they?
```
df_red['quality'].value_counts()

df_red['quality'].nunique()
df_white['quality'].nunique()
df_red['quality'].unique()
df_white['quality'].unique()
```
#### *Prepare to combine two datasets
 - First, add new column telling Red/White to preserve the characteristics.
 - Creating two arrays using numpy as long as the number of rows in the red and white dataframes that repeat the value “red” or “white” and add that as a column into each dataframe.
```
color_red = np.repeat('red', 1599)
color_white = np.repeat('white', 4898)
```
#### *Add arrays to the red and white dataframes by setting a new column called 'color' to the appropriate array.
```
df_red['color'] = color_red
df_white['color'] = color_white
```
#### *Combine DataFrames with 'append()'
```
df_wine = df_red.append(df_white, ignore_index=True) 
df_wine.tail()
```
<img src="https://user-images.githubusercontent.com/31917400/33956444-78317720-e036-11e7-8eb3-b2c5df8606a9.jpg" width="350" height="100" />

#### *Oops, Error! so we need to change col_name of 'df_white' as well.
```
df_white = df_white.rename(columns = {'total sulfur dioxide': 'total_sulfur_dioxide'})
```
#### *Save your newly combined dataframe as winequality_edited.csv. Remember, set index=False to avoid saving with an unnamed column!
```
df_wine.to_csv('~path~/winequality_edited.csv', index=False)
```

### 3> Explore the dataset

## (A) Univariate Plot
#### *Histogram -Fixed Acidity, Total Sulfur Dioxide, pH, Alcohol..How they look like?
```
df_wine['fixed acidity'].plot(kind='hist')
df_wine['total_sulfur_dioxide'].plot(kind='hist')
df_wine['pH'].plot(kind='hist')
df_wine['alcohol'].plot(kind='hist')
```
<img src="https://user-images.githubusercontent.com/31917400/33957203-e1433756-e038-11e7-8fd6-842ddcb08d4c.jpg" />

#### *Scatter plot - Quality(response) VS. Fixed Acidity, Total Sulfur Dioxide, pH, Alcohol. Alcohol seems most likely to have a positive impact on quality.
```
df_wine.plot(x='quality', y='fixed acidity', kind='scatter') 
df_wine.plot(x='quality', y='total_sulfur_dioxide', kind='scatter') 
df_wine.plot(x='quality', y='pH', kind='scatter')
df_wine.plot(x='quality', y='alcohol', kind='scatter') 
```
<img src="https://user-images.githubusercontent.com/31917400/33957833-a90b5754-e03a-11e7-926e-4c7aed1a73b1.jpg" />

#### *How to group the data and **aggregate information** about these groups or perform group-specific transformation ?
 - For example, we can find mean - fixed acidity, Total_Sulfur_Dioxide, pH, alcohol- for all samples.
 - but WHAT IF we need the mean for each quality rating? For example, mean pH level for all samples of the quality rating of 7? Here, 'groupby()' is used to designate those 'muti-category' variables! 
```
df_wine.mean()
df_wine.groupby('quality').mean() 
```
<img src="https://user-images.githubusercontent.com/31917400/33958321-f91f19a0-e03b-11e7-9deb-33d5175c4a1f.jpg" width="350" height="100" />

#### *We could even split it further with multiple columns by providing a list (of categorical variables).
```
df_wine.groupby(['quality','color']).mean()
```
<img src="https://user-images.githubusercontent.com/31917400/33958565-9fcb4a1c-e03c-11e7-9f09-20c6ae31e39f.jpg" width="350" height="180" />

## (B) Bivariate Plot
> Q1. Is a certain type of wine (red or white) associated with higher quality?** (Grouping by the Categorical)
 - Find the mean quality of each wine type (red and white) with groupby.
```
df_wine.groupby('color').mean()
```
<img src="https://user-images.githubusercontent.com/31917400/33965948-b0ab49d6-e055-11e7-9fe0-86a43d03bca6.jpg" width="350" height="50" />

> Hey, it gives a dataset !

#### *Plotting to display our findings regarding the associations b/w quality and some properties
 - Plotting the **mean-'quality'** by 'color'
```
df_wine.groupby('color')['quality'].mean().plot(kind='bar', title='Avg Quality by Color', color = ['red', 'white'] , alpha=0.7)
```
 - Improving the plot-I: Set xlabel, ylabel...=> plt is useful!
```
color_means = df_wine.groupby('color')['quality'].mean()

colors=['red', 'white'] 
color_means.plot(kind='bar', title='Avg Quality by Color', color = colors) #from the pd-plot

plt.xlabel('Colors', fontsize=18) #from the plt-plot
plt.ylabel('Quality', fontsize=18)
```
<img src="https://user-images.githubusercontent.com/31917400/33968417-ab0854b0-e05f-11e7-9893-b103b71683be.jpg" width="400" height="160" />

 - Improving the plot-II: Get more details on where this difference is coming from.
   - Note: Care the order! For example, ['quality','color'] differs from ['color','quality']
```
counts = df_wine.groupby(['quality', 'color']).count(); counts 
```
<img src="https://user-images.githubusercontent.com/31917400/33968607-bfd0f2fc-e060-11e7-985a-55b56f43759a.jpg" width="500" height="200" />

> Hey, it gives a dataset !

 - Plotting the **counts** by 'quality' and 'color'
```
counts = df_wine.groupby(['quality', 'color']).count()['pH']  # why pH? The values for all columns are the same...coz it's a count! 

colors=['red', 'white'] 
counts.plot(kind='bar', title='Avg Quality by Color_A', color=colors)

plt.xlabel('Quality + Colors', fontsize=18) 
plt.ylabel('Count', fontsize=18)
```
There are clearly more white samples than red samples. so it's hard to make a fair comparison. To balance this out, we can divide each count by the total count for that color to use "proportions" instead.
```
counts = df_wine.groupby(['quality', 'color']).count()['pH'] 
total = df_wine.groupby('color').count()['pH']
prop = counts / total

colors=['red', 'white'] 
prop.plot(kind='bar', title='Avg Quality by Color_B', color=colors)

plt.xlabel('Quality + Colors', fontsize=18) 
plt.ylabel('Proportion', fontsize=18)
```
As can be seen, for the lower ratings -3/4/5, 'red' shows higher proportion. and for the higher ratings, the reverse is true. 

<img src="https://user-images.githubusercontent.com/31917400/33968829-f01b2b8e-e061-11e7-99b5-9dd091118b89.jpg" width="600" height="200" />

We just plotted using pandas' `plot()`
#### *Further customization in Matplotlib. It gives us much more control over our visualizations. ####
> Creating a **Bar Chart** Using Matplotlib ##
 - 1) There are two required arguments in pyplot's bar function: the "x-coordinates" of the bars, and the "heights" of the bars.
   - >>> plt.bar([1, 2, 3], [224, 620, 425]) #######(x-range / height)
 - 2) Specify the x tick labels using "plt.xticks()" function, or by specifying another parameter in the bar function. 
   - >>> plt.bar([1, 2, 3], [224, 620, 425], align='center') #######(x-range / height / location)
   - >>> plt.xticks([1, 2, 3], ['a', 'b', 'c']) #######(x-range / labels)
   - >>> **plt.bar([1, 2, 3], [224, 620, 425], tick_label=['a', 'b', 'c'], rotation='vertical')**
 - 3) Basic plt collections...
   - >>> plt.title('blahblah')
   - >>> plt.xlabel('blah', fontsize=n)
   - >>> plt.ylabel('blah', fontsize=n)
#######################################################################################################

> Q2. What level of acidity (pH value) receives the highest average rating?:** (Grouping by the Numerical)
 - This question is more tricky because unlike color, which has clear categories you can group by (red and white), pH is a quantitative variable without clear categories. 
 - However, there is a simple fix to this. You can create a categorical variable from a quantitative variable by creating your own categories. 
 - Pandas **'cut()'** function that let you "cut" data in groups. Using this, create a new column called 'acidity_levels' with these categories:
   - Acidity Levels:
     - High: Lowest 25% of pH values
     - Moderately High: 25% - 50% of pH values
     - Medium: 50% - 75% of pH values
     - Low: 75% - max pH value 
 - View the min, 25%, 50%, 75%, max pH values with Pandas describe(). Here, the data is able to be split at the 25th, 50th, and 75th percentile.After you create these four categories, you'll be able to use groupby to get the mean quality rating for each acidity level. 
```
df_wine['pH'].describe()
```
 - Adding new categorical column!
   - Bin edges that will be used to "cut" the data into groups
   - Labels for the four acidity level groups - min/25%/50%/75%/max
   - Creates new 'acidity_levels' column
   - Checks for successful creation of this column
   - The 'cut()' can be useful for going from a continuous variable to a categorical variable. For example, cut could convert ages
to groups of age ranges. Any NA values will be NA in the result. Out of bounds values will be NA in the resulting Categorical 
object. >>> pd.cut(np.array([.2, 1.4, 2.5, 6.2, 9.7, 2.1]), 3, labels=["good", "medium", "bad"])
```
bin_edges = [2.72,3.11,3.21,3.32,4.01] 
bin_names = ['very_high','high','medium','low'] 
df_wine['acidity_levels'] = pd.cut(df_wine['pH'], bin_edges, labels=bin_names)
df_wine.head()
```
<img src="https://user-images.githubusercontent.com/31917400/33966541-dc013436-e057-11e7-91c1-3001effcd373.jpg" width="350" height="80" />

 - Find the mean quality of each acidity level with groupby
```
df_wine.groupby('acidity_levels')['quality'].mean()
```
<img src="https://user-images.githubusercontent.com/31917400/33966902-33563dca-e059-11e7-8520-3b3acbfea0d5.jpg" width="350" height="60" />

#### *Plotting to display our findings regarding the associations b/w quality and some properties
 - Create a bar chart of the mean quality with a bar for each of the 4 acidity levels.
```
df_wine['pH'].describe()
bin_edges = [2.72,3.11,3.21,3.32,4.01] 
bin_names = ['very_high','high','medium','low'] 
df_wine['acidity_levels'] = pd.cut(df_wine['pH'], bin_edges, labels=bin_names)


levels = df_wine.groupby('acidity_levels')['quality'].mean()
mean_qual_low =levels[3]
mean_qual_medium = levels[2]
mean_qual_high = levels[1]
mean_qual_veryhigh = levels[0]


rangeis = [1, 2, 3, 4]
heights = [mean_qual_low, mean_qual_medium, mean_qual_high, mean_qual_veryhigh]
labels = ['Low','medium','high','very_high']
plt.bar(rangeis, heights, tick_label=labels)

plt.title('Average Quality Ratings by Acidity level')
plt.xlabel('Acidity level')
plt.ylabel('Average Quality Rating')
```
<img src="https://user-images.githubusercontent.com/31917400/33997727-358992d4-e0dd-11e7-98a1-b68186901437.jpg" width="300" height="200" />

> Q3. Do wines with higher alcoholic content receive better ratings?** (Groupying by splitting the dataset)
 - To answer this question, use **'query()'** function to create two groups of wine samples: 
   - Low alcohol (samples with an alcohol content less than the median)
   - High alcohol (samples with an alcohol content greater than or equal to the median)
   - Then, find the mean quality rating of each group.
   - query() cannot deal with the name with space..so ('residual sugar' -> 'residual_sugar') 
 - Get the median amount of alcohol content
```
df_wine['alcohol'].median()
```
 - Select samples with alcohol content less than the median and / greater than or equal to the median
 - If Y has n rows and m columns, then Y.shape = (n,m), Y.shape[0] = n.
```
#low_alcohol = df_wine[df_wine['alcohol'] < 10.3]
low_alcohol = df_wine.query('alcohol < 10.3')

#high_alcohol = df_wine[df_wine['alcohol'] >= 10.3]
high_alcohol = df_wine.query('alcohol >= 10.3')
```
 - Ensure these queries included each sample exactly once
```
num_samples = df_wine.shape[0]   #total num of rows? 
num_samples == low_alcohol['quality'].count() + high_alcohol['quality'].count()   #should be True
```
 - Get the mean quality rating for the low alcohol and high alcohol groups
```
low_alcohol['quality'].mean() #5.48 rating
high_alcohol['quality'].mean() #6.15 rating
```
#### *Plotting to display our findings regarding the associations b/w quality and some properties
 - Create a bar chart of the mean quality with one bar for low alcohol and one bar for high alcohol wine samples.
```
a_median = df_wine['alcohol'].median()

a_low = df_wine.query('alcohol < {}'.format(a_median))
a_high = df_wine.query('alcohol >= {}'.format(a_median))

mean_qual_low = a_low['quality'].mean()
mean_qual_high = a_high['quality'].mean()

rangeis = [1, 2]
heights = [mean_qual_low, mean_qual_high]
labels = ['Low', 'High']
plt.bar(rangeis, heights, tick_label=labels)

plt.title('Average Quality Ratings by Alcohol Content')
plt.xlabel('Alcohol Content')
plt.ylabel('Average Quality Rating')
```
<img src="https://user-images.githubusercontent.com/31917400/33994734-9bbf2c30-e0d3-11e7-9757-701755745dc6.jpg" width="300" height="200" />



## (C) Multivariate Plot
 - How to plot by 'color' and by 'quality' at the same time? (Not using Pandas' plot, but using 'plt') 
```
sns.set_style('darkgrid')
```
> Create arrays for red bar heights white bar heights. There's a bar for each combination of 'color' and 'quality rating'. Each bar's height is based on the proportion of samples of that color with that quality rating.
 - For example:
   - 1. Red bar proportions = counts for each quality rating / total # of red samples
   - 2. White bar proportions = counts for each quality rating / total # of white samples
 
 - Get counts for each rating and color
``` 
color_counts = df_wine.groupby(['color', 'quality']).count()['pH']; color_counts
```
 - Get total counts for each color
```
color_totals = df_wine.groupby('color').count()['pH']; color_totals
```
 - Get proportions by dividing red rating counts by total # of red samples
``` 
red_proportions = color_counts['red'] / color_totals['red']; red_proportions
```
 - Get proportions by dividing white rating counts by total # of white samples
```
white_proportions = color_counts['white'] / color_totals['white']; white_proportions
```
> Plot proportions on a bar chart.
 - Set the x coordinate location for each rating group and and width of each bar.
```
ind = np.arange(len(red_proportions))  # the x locations for the groups
width = 0.35       # the width of the bars
```
 - Plot bars
``` 
red_bars = plt.bar(ind, red_proportions, width, color='r', alpha=.7, label='Red Wine')
white_bars = plt.bar(ind + width, white_proportions, width, color='w', alpha=.7, label='White Wine')
```
 - title and labels and legend
``` 
plt.ylabel('Proportion')
plt.xlabel('Quality')
plt.title('Proportion by Wine Color and Quality')
locations = ind + width / 2  # xtick locations
labels = ['3', '4', '5', '6', '7', '8', '9']  # xtick labels
plt.xticks(locations, labels)

plt.legend()
```
 - Oh, that didn't work because we're missing a red wine value for a the 9 rating. Even though this number is a 0, we need it for our plot. Plot again!
```
red_proportions['9'] = 0; red_proportions
```
<img src="https://user-images.githubusercontent.com/31917400/34003736-f1a0c87e-e0ed-11e7-99c3-b929da3f7242.jpg" width="600" height="200" />






