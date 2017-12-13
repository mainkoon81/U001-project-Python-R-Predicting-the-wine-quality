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
  - 12 - quality (score between 0 and 10)

__Investigation:__ What chemical charateristics are most important in predicting the quality of wine? 

1> import and wrangle the data set
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
























