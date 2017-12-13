# Python-project-00-DA

### [Contents] 

__Case-01.__ Wine Quality Data Set
  - package: Pandas, Numpy, Matplotlib, Seaborn, 
  - func:

__Case-02.__ 
  - package:
  - func: 
-------------------------------------------------------------------------------------------------------------------------------------  
### Case-01. Wine Quality Data Set
  
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

__Investigation:__ What chemical charateristics are most important in predicting the quality of wine? 

__1> import the dataset__
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

__2> wrangle the dataset__

#How many duplicate rows are in the dataset ? 
```
sum(df_white.duplicated())
```
#How to drop the rows ?
```
df_white.drop_duplicates(inplace=True)
```
#How many unique values in 'quality' (**categorical response variable**) column ?
```
df_red['quality'].nunique()
df_white['quality'].nunique()
```
#Combine two data set
 - First, add new column telling Red/White to preserve the characteristics.
 - Creating two arrays using numpy as long as the number of rows in the red and white dataframes that repeat the value “red” or “white” and add that as a column into each dataframe.
```
color_red = np.repeat('red', 1599)
color_white = np.repeat('white', 4898)
```
#Add arrays to the red and white dataframes by setting a new column called 'color' to the appropriate array.
```
df_red['color'] = color_red
df_white['color'] = color_white
```
#Combine DataFrames with 'append()'
```
df_wine = df_red.append(df_white, ignore_index=True) 
df_wine.tail()
```
<img src="https://user-images.githubusercontent.com/31917400/33956444-78317720-e036-11e7-8eb3-b2c5df8606a9.jpg" width="350" height="100" />

#Oops, Error! so we need to change col_name of 'df_white' as well.
```
df_white = df_white.rename(columns = {'total sulfur dioxide': 'total_sulfur_dioxide'})
```
#Save your newly combined dataframe as winequality_edited.csv. Remember, set index=False to avoid saving with an unnamed column!
```
df_wine.to_csv('winequality_edited.csv', index=False)
```

__3> Visual Exploration__#df_wine['fixed acidity'].plot(kind='hist')
#df_wine['total_sulfur_dioxide'].plot(kind='hist')
#df_wine['pH'].plot(kind='hist')
df_wine['alcohol'].plot(kind='hist')

#Histogram -Fixed Acidity, Total Sulfur Dioxide, pH, Alcohol
```
df_wine['fixed acidity'].plot(kind='hist')
df_wine['total_sulfur_dioxide'].plot(kind='hist')
df_wine['pH'].plot(kind='hist')
df_wine['alcohol'].plot(kind='hist')
```
<img src="https://user-images.githubusercontent.com/31917400/33957203-e1433756-e038-11e7-8fd6-842ddcb08d4c.jpg" width="350" height="100" />





