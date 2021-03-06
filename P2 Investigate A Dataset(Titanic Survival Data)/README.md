
# Titanic Survival Data Analysis

The data set which I am going to explore contains the information gathered from 891 of the 2224 passengers and crew on board the Titanic. The Data set has various columns like 

### Data Dictionary

- **Survival** : Survival(0 = No, 1 = Yes)
- **Pclass**   : Ticket class(1 = 1st, 2 = 2nd, 3 = 3rd)
- **Sex**
- **Age**
- **Sibsp** : # of siblings / spouses aboard the Titanic
- **Parch** : # of parents / children aboard the Titanic
- **Ticket** : Ticket Number
- **Fare**
- **Cabin** : Cabin Number
- **Embarked** : Port of Embarkation.(C = Cherbourg, Q = Queenstown, S = Southampton)


### Variable Notes

**Pclass:** A proxy for socio-economic status (SES)
- 1st = Upper
- 2nd = Middle
- 3rd = Lower

**Age:** Age is fractional if less than 1. If the age is estimated, is it in the form of xx.5

**Sibsp:**  The dataset defines family relations in this way...
- Sibling = brother, sister, stepbrother, stepsister
- Spouse = husband, wife (mistresses and fiancés were ignored)

**Parch:** The dataset defines family relations in this way...
- Parent = mother, father
- Child = daughter, son, stepdaughter, stepson
- Some children travelled only with a nanny, therefore parch=0 for them.

## Questions To Explore

- **Did Gender played any role in survival** ?
- **Did Pclass(A proxy for socio-economic status (SES)) played any role in survival ?**
- **Did Females of any particular Pclass receive special priority ?**
- **Did any particular age got priority over other age groups ?**
- **Did family relations(Parch value) played any role in survival ?**

## Loading Data From Csv

For importing data from csv, pandas has a built in function which reads the information into a dataframe. 


```python
import pandas as pd

titanic_data = pd.read_csv('titanic_data.csv')
titanic_data.describe()
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>714.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
      <td>891.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>446.000000</td>
      <td>0.383838</td>
      <td>2.308642</td>
      <td>29.699118</td>
      <td>0.523008</td>
      <td>0.381594</td>
      <td>32.204208</td>
    </tr>
    <tr>
      <th>std</th>
      <td>257.353842</td>
      <td>0.486592</td>
      <td>0.836071</td>
      <td>14.526497</td>
      <td>1.102743</td>
      <td>0.806057</td>
      <td>49.693429</td>
    </tr>
    <tr>
      <th>min</th>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>1.000000</td>
      <td>0.420000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>223.500000</td>
      <td>0.000000</td>
      <td>2.000000</td>
      <td>20.125000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>7.910400</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>446.000000</td>
      <td>0.000000</td>
      <td>3.000000</td>
      <td>28.000000</td>
      <td>0.000000</td>
      <td>0.000000</td>
      <td>14.454200</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>668.500000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>38.000000</td>
      <td>1.000000</td>
      <td>0.000000</td>
      <td>31.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>891.000000</td>
      <td>1.000000</td>
      <td>3.000000</td>
      <td>80.000000</td>
      <td>8.000000</td>
      <td>6.000000</td>
      <td>512.329200</td>
    </tr>
  </tbody>
</table>
</div>



- The above data shows that all the columns have data count as 891 except the age which we will handle in age data exploration part. 

## Data Cleaning

- As we see that cabin data mostly contains NaN and won't be much useful for exploration,
- And same lies with the Embarked column and Fare Column, this data too won't be much useful, so lets drop these columns from dataframe.


```python
del titanic_data['Cabin']
del titanic_data['Embarked']
titanic_data.head(3)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>3</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
      <td>1</td>
      <td>0</td>
      <td>A/5 21171</td>
      <td>7.2500</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
      <td>1</td>
      <td>0</td>
      <td>PC 17599</td>
      <td>71.2833</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>3</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
      <td>0</td>
      <td>0</td>
      <td>STON/O2. 3101282</td>
      <td>7.9250</td>
    </tr>
  </tbody>
</table>
</div>



## Overall Metrics

The information has a column called 'survived' whose values are either 0 or 1, using this column we can count the total number of casualties and survivors.


```python
total_no_passengers = len(titanic_data['PassengerId'])
survival_data = titanic_data['Survived']
survivied = (survival_data == 1).sum()
casualties = (survival_data == 0).sum()

print('------------------------------------------')
print('Total number of passengers on board: ',total_no_passengers)
print('Total number of survivors: ',survivied)
print('Total number of casualties: ',casualties)
print('------------------------------------------')
```

    ------------------------------------------
    Total number of passengers on board:  891
    Total number of survivors:  342
    Total number of casualties:  549
    ------------------------------------------
    

- The following pie chart shows the distribution of survivors and casulaties


```python
%pylab inline
import matplotlib.pyplot as plt

def drawPieChart(labels,sizes,explode,startangle):
    '''
    Function to produce pie charts
    '''
    plt.pie(sizes, explode=explode, labels=labels,autopct='%1.2f%%', shadow=True, startangle=startangle) 
    plt.axis('equal')
    plt.show()
    
drawPieChart(['Survivors', 'casualties'],[survivied,casualties],(0.1, 0),105)    
```

    Populating the interactive namespace from numpy and matplotlib
    


![png](output_7_1.png)


## Overall Metrics By Gender

The above metrics show the total number of survivors and casualties, now let us explore these metrics based on the gender.
Using the gender data and survival data, we can count the total number of female and male survivors. when we calulate female survivors and male survivors in terms of percentages with respect to total survivors  , we find that female survivors constitute (68.13%) and male survivors constitute (31.87%). Using this information, we can say that females where given more preference in using life saving materials like life boats, etc.




```python
gender_data = titanic_data['Sex']

male_count = (gender_data == 'male').sum()
female_count = (gender_data == 'female').sum()

male_survivors = ((gender_data == 'male') & (survival_data == 1)).sum()
female_survivors = ((gender_data == 'female') & (survival_data == 1)).sum()

male_casualties = male_count - male_survivors
female_casualties = female_count - female_survivors

print('-------------------------------------')
print('Total number of males: ',male_count)
print('Total number of male survivors: ',male_survivors)
print('Total number of male casualties: ',male_casualties)
print('--------------------------------------')
print('Total number of females: ',female_count)
print('Total number of female survivors: ',female_survivors)
print('Total number of female casualties: ',female_casualties)
print('--------------------------------------')
```

    -------------------------------------
    Total number of males:  577
    Total number of male survivors:  109
    Total number of male casualties:  468
    --------------------------------------
    Total number of females:  314
    Total number of female survivors:  233
    Total number of female casualties:  81
    --------------------------------------
    

- The following pie chart shows the distribution of male and female survivors


```python
drawPieChart(['Female Survivors', 'Male Survivors'],[female_survivors,male_survivors],(0.1, 0),60)
```


![png](output_11_0.png)


## Explore Casualties based on Pclass

Let us exlpore the Casualties based on Pclass(A proxy for socio-economic status (SES), 1st = Upper, 2nd = Middle, 3rd = Lower) to see whether passengers were given priority based on their class. Now Lets explore some metrics related to Pclass.



```python
pclass = titanic_data['Pclass']
pclass_values = sort(pclass.unique())

def calculateSurvivalRate(condition=True):
    '''
    Function to calculate survival Rates
    '''
    print('----------------------------------') 
    for val in pclass_values:
        pclass_count = ((pclass == val) & (condition)).sum()
        pclass_survivors = ((pclass == val)&(survival_data == 1) & (condition)).sum()
        pclass_casualties = ((pclass == val)&(survival_data == 0) & (condition)).sum()
        
        print('Pclass {0} Metrics: '.format(val))
        print('Total Count : ',pclass_count)
        print('Total Number Of Survivors: ',pclass_survivors)
        print('Total Number Of casualties: ',pclass_casualties)
        print('Survival Rate: {0}%'.format(round((pclass_survivors/pclass_count)*100.0,2)))
        print('----------------------------------') 
```


```python
calculateSurvivalRate()
```

    ----------------------------------
    Pclass 1 Metrics: 
    Total Count :  216
    Total Number Of Survivors:  136
    Total Number Of casualties:  80
    Survival Rate: 62.96%
    ----------------------------------
    Pclass 2 Metrics: 
    Total Count :  184
    Total Number Of Survivors:  87
    Total Number Of casualties:  97
    Survival Rate: 47.28%
    ----------------------------------
    Pclass 3 Metrics: 
    Total Count :  491
    Total Number Of Survivors:  119
    Total Number Of casualties:  372
    Survival Rate: 24.24%
    ----------------------------------
    

- From the above listed metrics, we can say that people from upper class(pclass = 1) were given priority as they had the highest survival rate of (62.96%) which is greater than the survival rates of both middle class and lower class. Lets plot a histogram of casualties to get a more precise picture


```python
pclass_casualties = pclass[survival_data == 0]

def drawHistogram(data,title,xlabel,ylabel,xticks=None,yticks=None,bins=None,color=None):
    '''
    Function to produce histogram
    '''
    plt.title(title)
    plt.xlabel(xlabel)
    plt.ylabel(ylabel)
    if xticks != None :
        plt.xticks(xticks)
    if yticks != None :
        plt.yticks(yticks)
    plt.hist(data,bins=bins,color=color)
    plt.show()

drawHistogram(pclass_casualties,'Distribution of casualties based on Pclass',
              'Pclass value','Number of survivors',range(1,4),color='DarkRed')
```


![png](output_16_0.png)


- The Histogram above shows the distribution of casualties based on their pclass, as we can observe that lower class has the   most number of casualties when compared to upper class and middle class. So, Now we can say that lower class people got the   least priority for life saving materials like life boats, etc. 
- As we already know that females were given more priority then males, Lets explore the female survival based on Pclass to see whether females of any particular class got special priority.


```python
calculateSurvivalRate(gender_data == 'female')
```

    ----------------------------------
    Pclass 1 Metrics: 
    Total Count :  94
    Total Number Of Survivors:  91
    Total Number Of casualties:  3
    Survival Rate: 96.81%
    ----------------------------------
    Pclass 2 Metrics: 
    Total Count :  76
    Total Number Of Survivors:  70
    Total Number Of casualties:  6
    Survival Rate: 92.11%
    ----------------------------------
    Pclass 3 Metrics: 
    Total Count :  144
    Total Number Of Survivors:  72
    Total Number Of casualties:  72
    Survival Rate: 50.0%
    ----------------------------------
    

- The above data shows that females of upper class and middle class got priority, as they had the highest survival rates of (96.81%) and  (92.11%) when compared to the females of lower class who had a survival rate of only (50%). 
- The below Histogram shows the distribution of females casulaties based on Pclass.


```python
pclass_female_casualties = pclass[(survival_data == 0) & (gender_data == 'female')]
drawHistogram(pclass_female_casualties,'Distribution of female casualties based on Pclass',
              'Pclass value','Number of female casualties',xticks=range(1,4),color='DarkRed')
```


![png](output_20_0.png)


- Now lets explore male survival based on pclass to see whether males of any particular class got special priority.


```python
calculateSurvivalRate(gender_data == 'male')
```

    ----------------------------------
    Pclass 1 Metrics: 
    Total Count :  122
    Total Number Of Survivors:  45
    Total Number Of casualties:  77
    Survival Rate: 36.89%
    ----------------------------------
    Pclass 2 Metrics: 
    Total Count :  108
    Total Number Of Survivors:  17
    Total Number Of casualties:  91
    Survival Rate: 15.74%
    ----------------------------------
    Pclass 3 Metrics: 
    Total Count :  347
    Total Number Of Survivors:  47
    Total Number Of casualties:  300
    Survival Rate: 13.54%
    ----------------------------------
    

- The above metrics show that males of any particular class didn't receive special priority as every class has low survival rates. Another point to note is that Pclass 3 has the most male deaths which account to 300. 
- The below histogram shows the distribution of male casualties based on Pclass.


```python
pclass_male_casualties = pclass[(survival_data == 0) & (gender_data == 'male')]
drawHistogram(pclass_male_casualties,'Distribution of male casualties based on Pclass',
              'Pclass value','Number of male casualties',xticks=range(1,4),color='DarkRed')
```


![png](output_24_0.png)


## Explore Survivors And Casualties Based On Age

- In this part of data exploration, I want to check whether did age played any role in survival.
- As a first step towards the Age data exploration, Lets take a look at age data for any anomalies.


```python
titanic_data[['PassengerId','Survived','Name','Sex','Age']].head(7)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1</td>
      <td>0</td>
      <td>Braund, Mr. Owen Harris</td>
      <td>male</td>
      <td>22.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>1</td>
      <td>Cumings, Mrs. John Bradley (Florence Briggs Th...</td>
      <td>female</td>
      <td>38.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3</td>
      <td>1</td>
      <td>Heikkinen, Miss. Laina</td>
      <td>female</td>
      <td>26.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>4</td>
      <td>1</td>
      <td>Futrelle, Mrs. Jacques Heath (Lily May Peel)</td>
      <td>female</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>4</th>
      <td>5</td>
      <td>0</td>
      <td>Allen, Mr. William Henry</td>
      <td>male</td>
      <td>35.0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>6</td>
      <td>0</td>
      <td>Moran, Mr. James</td>
      <td>male</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>6</th>
      <td>7</td>
      <td>0</td>
      <td>McCarthy, Mr. Timothy J</td>
      <td>male</td>
      <td>54.0</td>
    </tr>
  </tbody>
</table>
</div>



- As we see that row number 5 contains NaN for Age column data,We need to drop all these values before taking the age data into consideration 
- Now lets take a look at some metrics for Age data.


```python
age_data = titanic_data['Age']
survived_age_data = age_data[survival_data==1].dropna()
casualties_age_data = age_data[survival_data==0].dropna()

oldest_to_survive = survived_age_data.max()
youngest_to_survive = survived_age_data.min()

oldest_to_die = casualties_age_data.max()
youngest_to_die = casualties_age_data.min()

mean_age_survivors = survived_age_data.mean()
mean_age_casualties = casualties_age_data.mean()

print('------------------------------')
print('Oldest To Survive: ',oldest_to_survive)
print('Youngest To Survive: ',youngest_to_survive)
print('------------------------------')
print('Oldest To die: ',oldest_to_die)
print('Youngest To die: ',youngest_to_die)
print('------------------------------')
print('Mean Age Of Survivors: ',round(mean_age_survivors,2))
print('Mean Age Of Casualties: ',round(mean_age_casualties,2))
print('------------------------------')
```

    ------------------------------
    Oldest To Survive:  80.0
    Youngest To Survive:  0.42
    ------------------------------
    Oldest To die:  74.0
    Youngest To die:  1.0
    ------------------------------
    Mean Age Of Survivors:  28.34
    Mean Age Of Casualties:  30.63
    ------------------------------
    

- Now lets plot the Age data of both the survivors and casualties to see whether any particular age group was given             preference. 


```python
drawHistogram(survived_age_data,'Distribution of Survivors based on Age',
              'Age value','Number of survivors',yticks=range(0,120,20),bins=8)
```


![png](output_30_0.png)



```python
drawHistogram(casualties_age_data,'Distribution of casualties based on Age',
              'Age value','Number of casualties',bins=8)
```


![png](output_31_0.png)


- When we observe distribution of age data, Even though we see a slight bump in data at age [0 - 10] of survivors when compared to the age data of casualties, We cannot say or conclude that children might have got some preference because there is not much difference between the total no of survivors and casulaties who were of age less than 10. 

- Lets explore the data of few passengers who were of age less than 10 and did not survive.



```python
child_casualties_data = titanic_data[(age_data < 10) & (survival_data == 0)]
child_casualties_data[['PassengerId','Survived','Name','Sex','Age','Ticket','Parch','Pclass']].head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Ticket</th>
      <th>Parch</th>
      <th>Pclass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>349909</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>0</td>
      <td>Rice, Master. Eugene</td>
      <td>male</td>
      <td>2.0</td>
      <td>382652</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>0</td>
      <td>Palsson, Miss. Torborg Danira</td>
      <td>female</td>
      <td>8.0</td>
      <td>349909</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>50</th>
      <td>51</td>
      <td>0</td>
      <td>Panula, Master. Juha Niilo</td>
      <td>male</td>
      <td>7.0</td>
      <td>3101295</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>63</th>
      <td>64</td>
      <td>0</td>
      <td>Skoog, Master. Harald</td>
      <td>male</td>
      <td>4.0</td>
      <td>347088</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>119</th>
      <td>120</td>
      <td>0</td>
      <td>Andersson, Miss. Ellis Anna Maria</td>
      <td>female</td>
      <td>2.0</td>
      <td>347082</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>147</th>
      <td>148</td>
      <td>0</td>
      <td>Ford, Miss. Robina Maggie "Ruby"</td>
      <td>female</td>
      <td>9.0</td>
      <td>W./C. 6608</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>164</th>
      <td>165</td>
      <td>0</td>
      <td>Panula, Master. Eino Viljami</td>
      <td>male</td>
      <td>1.0</td>
      <td>3101295</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>171</th>
      <td>172</td>
      <td>0</td>
      <td>Rice, Master. Arthur</td>
      <td>male</td>
      <td>4.0</td>
      <td>382652</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>182</th>
      <td>183</td>
      <td>0</td>
      <td>Asplund, Master. Clarence Gustaf Hugo</td>
      <td>male</td>
      <td>9.0</td>
      <td>347077</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



- Based upon above data, Lets explore the data of family members of these children. To explore the family data we need to take ticket value into consideration, as people might have booked tickets under same ticket value for all their family members.


```python
ticket_data = titanic_data['Ticket']
ticket_bool = (ticket_data == '349909')|(ticket_data == '382652')|(ticket_data == '347088')|(ticket_data == '3101295')
titanic_data[(age_data > 10) &(ticket_bool) ][['PassengerId','Survived','Name','Sex','Age','Ticket','Parch']].head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Ticket</th>
      <th>Parch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>167</th>
      <td>168</td>
      <td>0</td>
      <td>Skoog, Mrs. William (Anna Bernhardina Karlsson)</td>
      <td>female</td>
      <td>45.0</td>
      <td>347088</td>
      <td>4</td>
    </tr>
    <tr>
      <th>266</th>
      <td>267</td>
      <td>0</td>
      <td>Panula, Mr. Ernesti Arvid</td>
      <td>male</td>
      <td>16.0</td>
      <td>3101295</td>
      <td>1</td>
    </tr>
    <tr>
      <th>360</th>
      <td>361</td>
      <td>0</td>
      <td>Skoog, Mr. Wilhelm</td>
      <td>male</td>
      <td>40.0</td>
      <td>347088</td>
      <td>4</td>
    </tr>
    <tr>
      <th>567</th>
      <td>568</td>
      <td>0</td>
      <td>Palsson, Mrs. Nils (Alma Cornelia Berglund)</td>
      <td>female</td>
      <td>29.0</td>
      <td>349909</td>
      <td>4</td>
    </tr>
    <tr>
      <th>638</th>
      <td>639</td>
      <td>0</td>
      <td>Panula, Mrs. Juha (Maria Emilia Ojala)</td>
      <td>female</td>
      <td>41.0</td>
      <td>3101295</td>
      <td>5</td>
    </tr>
    <tr>
      <th>686</th>
      <td>687</td>
      <td>0</td>
      <td>Panula, Mr. Jaako Arnold</td>
      <td>male</td>
      <td>14.0</td>
      <td>3101295</td>
      <td>1</td>
    </tr>
    <tr>
      <th>885</th>
      <td>886</td>
      <td>0</td>
      <td>Rice, Mrs. William (Margaret Norton)</td>
      <td>female</td>
      <td>39.0</td>
      <td>382652</td>
      <td>5</td>
    </tr>
  </tbody>
</table>
</div>



- The above data shows that none of their family members survived.
- Now lets explore the data related to children who survived and were of age less than 10.


```python
child_survivors_data = titanic_data[(age_data < 10) & (survival_data == 1)]
child_survivors_data[['PassengerId','Survived','Name','Sex','Age','Ticket','Parch','Pclass']].head(10)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Ticket</th>
      <th>Parch</th>
      <th>Pclass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>10</th>
      <td>11</td>
      <td>1</td>
      <td>Sandstrom, Miss. Marguerite Rut</td>
      <td>female</td>
      <td>4.00</td>
      <td>PP 9549</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>43</th>
      <td>44</td>
      <td>1</td>
      <td>Laroche, Miss. Simonne Marie Anne Andree</td>
      <td>female</td>
      <td>3.00</td>
      <td>SC/Paris 2123</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>58</th>
      <td>59</td>
      <td>1</td>
      <td>West, Miss. Constance Mirium</td>
      <td>female</td>
      <td>5.00</td>
      <td>C.A. 34651</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>78</th>
      <td>79</td>
      <td>1</td>
      <td>Caldwell, Master. Alden Gates</td>
      <td>male</td>
      <td>0.83</td>
      <td>248738</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>165</th>
      <td>166</td>
      <td>1</td>
      <td>Goldsmith, Master. Frank John William "Frankie"</td>
      <td>male</td>
      <td>9.00</td>
      <td>363291</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>172</th>
      <td>173</td>
      <td>1</td>
      <td>Johnson, Miss. Eleanor Ileen</td>
      <td>female</td>
      <td>1.00</td>
      <td>347742</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>183</th>
      <td>184</td>
      <td>1</td>
      <td>Becker, Master. Richard F</td>
      <td>male</td>
      <td>1.00</td>
      <td>230136</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>184</th>
      <td>185</td>
      <td>1</td>
      <td>Kink-Heilmann, Miss. Luise Gretchen</td>
      <td>female</td>
      <td>4.00</td>
      <td>315153</td>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>193</th>
      <td>194</td>
      <td>1</td>
      <td>Navratil, Master. Michel M</td>
      <td>male</td>
      <td>3.00</td>
      <td>230080</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>233</th>
      <td>234</td>
      <td>1</td>
      <td>Asplund, Miss. Lillian Gertrud</td>
      <td>female</td>
      <td>5.00</td>
      <td>347077</td>
      <td>2</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



- Now lets explore the data of fmaily members related to these children.


```python
ticket_bool = ((ticket_data == '248738')|(ticket_data == 'SC/Paris 2123')|(ticket_data == '363291')|(ticket_data == '347077')
               |(ticket_data == 'C.A. 34651')|(ticket_data == 'F.C.C. 13529'))
data = titanic_data[(age_data > 10)&(ticket_bool)][['PassengerId','Survived','Name','Sex','Age','Ticket','Parch','Pclass']]

def highlight_max(s):
    '''
    To Highlight male rows in data
    '''
    is_max = s == 0
    if is_max['Survived'] :
        is_max = is_max | True  
         
    return ['background-color: #ff9999' if v else '' for v in is_max]

data.style.apply(highlight_max,axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Ticket</th>
      <th>Parch</th>
      <th>Pclass</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>25</th>
      <td>26</td>
      <td>1</td>
      <td>Asplund, Mrs. Carl Oscar (Selma Augusta Emilia...</td>
      <td>female</td>
      <td>38.0</td>
      <td>347077</td>
      <td>5</td>
      <td>3</td>
    </tr>
    <tr>
      <th>314</th>
      <td>315</td>
      <td>0</td>
      <td>Hart, Mr. Benjamin</td>
      <td>male</td>
      <td>43.0</td>
      <td>F.C.C. 13529</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>323</th>
      <td>324</td>
      <td>1</td>
      <td>Caldwell, Mrs. Albert Francis (Sylvia Mae Harb...</td>
      <td>female</td>
      <td>22.0</td>
      <td>248738</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>328</th>
      <td>329</td>
      <td>1</td>
      <td>Goldsmith, Mrs. Frank John (Emily Alice Brown)</td>
      <td>female</td>
      <td>31.0</td>
      <td>363291</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>440</th>
      <td>441</td>
      <td>1</td>
      <td>Hart, Mrs. Benjamin (Esther Ada Bloomfield)</td>
      <td>female</td>
      <td>45.0</td>
      <td>F.C.C. 13529</td>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>450</th>
      <td>451</td>
      <td>0</td>
      <td>West, Mr. Edwy Arthur</td>
      <td>male</td>
      <td>36.0</td>
      <td>C.A. 34651</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>472</th>
      <td>473</td>
      <td>1</td>
      <td>West, Mrs. Edwy Arthur (Ada Mary Worth)</td>
      <td>female</td>
      <td>33.0</td>
      <td>C.A. 34651</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>548</th>
      <td>549</td>
      <td>0</td>
      <td>Goldsmith, Mr. Frank John</td>
      <td>male</td>
      <td>33.0</td>
      <td>363291</td>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>608</th>
      <td>609</td>
      <td>1</td>
      <td>Laroche, Mrs. Joseph (Juliette Marie Louise La...</td>
      <td>female</td>
      <td>22.0</td>
      <td>SC/Paris 2123</td>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>685</th>
      <td>686</td>
      <td>0</td>
      <td>Laroche, Mr. Joseph Philippe Lemercier</td>
      <td>male</td>
      <td>25.0</td>
      <td>SC/Paris 2123</td>
      <td>2</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



- The above data shows the data related to family members of the childeren who survived the diaster. The data shows that males (rows which are red in color) didn't survive the disaster. Probably now we can relate to the fact that female and children were given preference.
- The others facts that we can produce from the data are like, if family didn't get a chance to survive, then in that case whole family died including children 
- If in case some how family got a chance to survive then females and children of that family were given preference. Though there might be some outliers with respect to above mentioned two scenarios, but both the two scenarios hold true in most of the cases.  

## Explore Survivors And Casualties Based On Parch
- Lets explore the Parch data see whether family relations played any role in passenger survival.
- Now lets a take a look at parch data of the passengers.


```python
parch_data = titanic_data['Parch']
survived_parch_data = parch_data[survival_data==1]
casualties_parch_data = parch_data[survival_data==0]

drawHistogram(parch_data,'Distribution of Parch Values','Parch value','Number of Passengers',bins=6,color='k')
```


![png](output_42_0.png)


- The above Histogram shows that most of the passengers had parch value as 'Zero', i.e, they travelled alone and din't had any dependents.  
- As the Parch value indicates both the number of parents/child abroad the titanic, We wont be able to answear the questions like female survival Vs female with children survival and most of the passengers parch value is zero, therefore there is not much to explore with respect to parch value.

## Some Misc Facts 
- All the Children who were of age less than one survived the disaster.


```python
misc_data = titanic_data[(age_data < 1)][['PassengerId','Survived','Pclass','Name','Sex','Age','Parch',]]

def highlight_age(s):
    '''
    highlight the age in a Series Red.
    '''
    s['Age'] = math.floor(s['Age'])
    is_max = s == 0
    return ['background-color: #ff9999' if v else '' for v in is_max]

misc_data.style.apply(highlight_age,axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Parch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>78</th>
      <td>79</td>
      <td>1</td>
      <td>2</td>
      <td>Caldwell, Master. Alden Gates</td>
      <td>male</td>
      <td>0.83</td>
      <td>2</td>
    </tr>
    <tr>
      <th>305</th>
      <td>306</td>
      <td>1</td>
      <td>1</td>
      <td>Allison, Master. Hudson Trevor</td>
      <td>male</td>
      <td>0.92</td>
      <td>2</td>
    </tr>
    <tr>
      <th>469</th>
      <td>470</td>
      <td>1</td>
      <td>3</td>
      <td>Baclini, Miss. Helene Barbara</td>
      <td>female</td>
      <td>0.75</td>
      <td>1</td>
    </tr>
    <tr>
      <th>644</th>
      <td>645</td>
      <td>1</td>
      <td>3</td>
      <td>Baclini, Miss. Eugenie</td>
      <td>female</td>
      <td>0.75</td>
      <td>1</td>
    </tr>
    <tr>
      <th>755</th>
      <td>756</td>
      <td>1</td>
      <td>2</td>
      <td>Hamalainen, Master. Viljo</td>
      <td>male</td>
      <td>0.67</td>
      <td>1</td>
    </tr>
    <tr>
      <th>803</th>
      <td>804</td>
      <td>1</td>
      <td>3</td>
      <td>Thomas, Master. Assad Alexander</td>
      <td>male</td>
      <td>0.42</td>
      <td>1</td>
    </tr>
    <tr>
      <th>831</th>
      <td>832</td>
      <td>1</td>
      <td>2</td>
      <td>Richards, Master. George Sibley</td>
      <td>male</td>
      <td>0.83</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- Most of the children who were of age less than 10 and didn't survive the disaster belong to plcass 3.


```python
misc_data = child_casualties_data[['PassengerId','Survived','Pclass','Name','Sex','Age','Parch']]
def highlight_class(s):
    '''
    highlight the class in a Series Red.
    '''
    is_max = s == 3
    is_max['Age'] = False 
    return ['background-color: #ff9999' if v else '' for v in is_max]

misc_data.style.apply(highlight_class,axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Parch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>7</th>
      <td>8</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Master. Gosta Leonard</td>
      <td>male</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>17</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Master. Eugene</td>
      <td>male</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>24</th>
      <td>25</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Miss. Torborg Danira</td>
      <td>female</td>
      <td>8.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>50</th>
      <td>51</td>
      <td>0</td>
      <td>3</td>
      <td>Panula, Master. Juha Niilo</td>
      <td>male</td>
      <td>7.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>63</th>
      <td>64</td>
      <td>0</td>
      <td>3</td>
      <td>Skoog, Master. Harald</td>
      <td>male</td>
      <td>4.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>119</th>
      <td>120</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Miss. Ellis Anna Maria</td>
      <td>female</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>147</th>
      <td>148</td>
      <td>0</td>
      <td>3</td>
      <td>Ford, Miss. Robina Maggie "Ruby"</td>
      <td>female</td>
      <td>9.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>164</th>
      <td>165</td>
      <td>0</td>
      <td>3</td>
      <td>Panula, Master. Eino Viljami</td>
      <td>male</td>
      <td>1.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>171</th>
      <td>172</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Master. Arthur</td>
      <td>male</td>
      <td>4.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>182</th>
      <td>183</td>
      <td>0</td>
      <td>3</td>
      <td>Asplund, Master. Clarence Gustaf Hugo</td>
      <td>male</td>
      <td>9.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>205</th>
      <td>206</td>
      <td>0</td>
      <td>3</td>
      <td>Strom, Miss. Telma Matilda</td>
      <td>female</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>278</th>
      <td>279</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Master. Eric</td>
      <td>male</td>
      <td>7.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>297</th>
      <td>298</td>
      <td>0</td>
      <td>1</td>
      <td>Allison, Miss. Helen Loraine</td>
      <td>female</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>374</th>
      <td>375</td>
      <td>0</td>
      <td>3</td>
      <td>Palsson, Miss. Stina Viola</td>
      <td>female</td>
      <td>3.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>386</th>
      <td>387</td>
      <td>0</td>
      <td>3</td>
      <td>Goodwin, Master. Sidney Leonard</td>
      <td>male</td>
      <td>1.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>480</th>
      <td>481</td>
      <td>0</td>
      <td>3</td>
      <td>Goodwin, Master. Harold Victor</td>
      <td>male</td>
      <td>9.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>541</th>
      <td>542</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Miss. Ingeborg Constanzia</td>
      <td>female</td>
      <td>9.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>634</th>
      <td>635</td>
      <td>0</td>
      <td>3</td>
      <td>Skoog, Miss. Mabel</td>
      <td>female</td>
      <td>9.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>642</th>
      <td>643</td>
      <td>0</td>
      <td>3</td>
      <td>Skoog, Miss. Margit Elizabeth</td>
      <td>female</td>
      <td>2.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>787</th>
      <td>788</td>
      <td>0</td>
      <td>3</td>
      <td>Rice, Master. George Hugh</td>
      <td>male</td>
      <td>8.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>813</th>
      <td>814</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Miss. Ebba Iris Alfrida</td>
      <td>female</td>
      <td>6.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>824</th>
      <td>825</td>
      <td>0</td>
      <td>3</td>
      <td>Panula, Master. Urho Abraham</td>
      <td>male</td>
      <td>2.0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>850</th>
      <td>851</td>
      <td>0</td>
      <td>3</td>
      <td>Andersson, Master. Sigvard Harald Elias</td>
      <td>male</td>
      <td>4.0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>852</th>
      <td>853</td>
      <td>0</td>
      <td>3</td>
      <td>Boulos, Miss. Nourelain</td>
      <td>female</td>
      <td>9.0</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



- The People who paid the highest fair belonged to Pclass and survived the titanic disaster.


```python
index = titanic_data['Fare'].argmax()
ticket_max = titanic_data.iloc[index]['Ticket']
titanic_data[titanic_data['Ticket'] == ticket_max]
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>SibSp</th>
      <th>Parch</th>
      <th>Ticket</th>
      <th>Fare</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>258</th>
      <td>259</td>
      <td>1</td>
      <td>1</td>
      <td>Ward, Miss. Anna</td>
      <td>female</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17755</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th>679</th>
      <td>680</td>
      <td>1</td>
      <td>1</td>
      <td>Cardeza, Mr. Thomas Drake Martinez</td>
      <td>male</td>
      <td>36.0</td>
      <td>0</td>
      <td>1</td>
      <td>PC 17755</td>
      <td>512.3292</td>
    </tr>
    <tr>
      <th>737</th>
      <td>738</td>
      <td>1</td>
      <td>1</td>
      <td>Lesurer, Mr. Gustave J</td>
      <td>male</td>
      <td>35.0</td>
      <td>0</td>
      <td>0</td>
      <td>PC 17755</td>
      <td>512.3292</td>
    </tr>
  </tbody>
</table>
</div>



- Around 50% of Pclass 2 males survivors where of age less than 10.


```python
misc_data = (titanic_data[(pclass == 2)&(gender_data == 'male') & (survival_data == 1)]
             [['PassengerId','Survived','Pclass','Name','Sex','Age','Parch']])

def highlight_age_lessThan10(s):
    '''
    highlight the age in a Series Red.
    '''
    is_max = [False,False,False,False,False,False,False]
    if s['Age'] < 10.0:
        is_max[5] = True 
          
    return ['background-color: #ff9999' if v else '' for v in is_max]

misc_data.style.apply(highlight_age_lessThan10,axis=1)
```




<div>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>PassengerId</th>
      <th>Survived</th>
      <th>Pclass</th>
      <th>Name</th>
      <th>Sex</th>
      <th>Age</th>
      <th>Parch</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>17</th>
      <td>18</td>
      <td>1</td>
      <td>2</td>
      <td>Williams, Mr. Charles Eugene</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>21</th>
      <td>22</td>
      <td>1</td>
      <td>2</td>
      <td>Beesley, Mr. Lawrence</td>
      <td>male</td>
      <td>34.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>78</th>
      <td>79</td>
      <td>1</td>
      <td>2</td>
      <td>Caldwell, Master. Alden Gates</td>
      <td>male</td>
      <td>0.83</td>
      <td>2</td>
    </tr>
    <tr>
      <th>183</th>
      <td>184</td>
      <td>1</td>
      <td>2</td>
      <td>Becker, Master. Richard F</td>
      <td>male</td>
      <td>1.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>193</th>
      <td>194</td>
      <td>1</td>
      <td>2</td>
      <td>Navratil, Master. Michel M</td>
      <td>male</td>
      <td>3.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>226</th>
      <td>227</td>
      <td>1</td>
      <td>2</td>
      <td>Mellors, Mr. William John</td>
      <td>male</td>
      <td>19.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>288</th>
      <td>289</td>
      <td>1</td>
      <td>2</td>
      <td>Hosono, Mr. Masabumi</td>
      <td>male</td>
      <td>42.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>340</th>
      <td>341</td>
      <td>1</td>
      <td>2</td>
      <td>Navratil, Master. Edmond Roger</td>
      <td>male</td>
      <td>2.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>407</th>
      <td>408</td>
      <td>1</td>
      <td>2</td>
      <td>Richards, Master. William Rowe</td>
      <td>male</td>
      <td>3.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>543</th>
      <td>544</td>
      <td>1</td>
      <td>2</td>
      <td>Beane, Mr. Edward</td>
      <td>male</td>
      <td>32.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>547</th>
      <td>548</td>
      <td>1</td>
      <td>2</td>
      <td>Padro y Manent, Mr. Julian</td>
      <td>male</td>
      <td>NaN</td>
      <td>0</td>
    </tr>
    <tr>
      <th>549</th>
      <td>550</td>
      <td>1</td>
      <td>2</td>
      <td>Davies, Master. John Morgan Jr</td>
      <td>male</td>
      <td>8.00</td>
      <td>1</td>
    </tr>
    <tr>
      <th>570</th>
      <td>571</td>
      <td>1</td>
      <td>2</td>
      <td>Harris, Mr. George</td>
      <td>male</td>
      <td>62.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>673</th>
      <td>674</td>
      <td>1</td>
      <td>2</td>
      <td>Wilhelms, Mr. Charles</td>
      <td>male</td>
      <td>31.00</td>
      <td>0</td>
    </tr>
    <tr>
      <th>755</th>
      <td>756</td>
      <td>1</td>
      <td>2</td>
      <td>Hamalainen, Master. Viljo</td>
      <td>male</td>
      <td>0.67</td>
      <td>1</td>
    </tr>
    <tr>
      <th>827</th>
      <td>828</td>
      <td>1</td>
      <td>2</td>
      <td>Mallet, Master. Andre</td>
      <td>male</td>
      <td>1.00</td>
      <td>2</td>
    </tr>
    <tr>
      <th>831</th>
      <td>832</td>
      <td>1</td>
      <td>2</td>
      <td>Richards, Master. George Sibley</td>
      <td>male</td>
      <td>0.83</td>
      <td>1</td>
    </tr>
  </tbody>
</table>
</div>



## The Conclusion :

From the above exploration of data, we can conclude the following 
- Females were given more priority than males for life saving materials like life boats because gender based exploration shows that females had a survival rate of 68.13% which is lot higher than males.
- Females of upper class and middle class received the highest priority because Pclass exploration shows that females of upper and middle class had a survival rates of more than 90%.
- Under the Age data exploration we found out that if family didn't get chance to survive than whole family died including children and if in case family got a chance to survive then females and children were given priority.  
 
