# List of Python Codes to Data Cleaning with Pandas

## Content

 * **[Import Data](#Import-Data)**
 * **[Data Exploration](#Data-Exploration)**
 * **[Data Manipulation](#Data-Manipulation)**
 * **[Data Cleaning](#Data-Cleaning)**
 * **[Data Analysis](#Data-Analysis)**
 * **[Data Storage](#Data-Storage)**


  * **[Sources](#Sources)**


* Source 1. [Pythonic Data Cleaning With Pandas and NumPy](https://realpython.com/python-data-cleaning-numpy-pandas/)
* Source 2. [Data Wrangling with Pandas](https://www.pluralsight.com/guides/data-wrangling-pandas). The The code is available on [pandas-demo repo](https://github.com/fivethirtyeight/data/tree/master/daily-show-guests) for demonstration and practice.
* Source 3. [Complet list with string methods](https://docs.python.org/2.5/lib/string-methods.html)


## Import Data

```python
# Import Data

import numpy as np
import pandas as pd

 # Import Data
 df = pd.read_csv("text.csv")
 df = pd.read_csv('file2.txt', sep = '\t') # depending the character used to separate the values
 df = pd.read_sql("text.sql")
 df = read_json("text.")
 df = read_excel("text.xlsx")
 df = read_excel("text.xlsx", sheet_name= "Sheet1") # to select a particular sheet
```

* To open excel files pandas need "xlrd" module => ```pip install xlrd```
* SQL data access requires you to setup the connection using ```pyodbc.connect```  
* then use ```pd.read_sql(sql_query, connection_object)```to retrieve data.

---

## Data Exploration

**overview of the DataFrame**

```python
df.head()
df.shape()
df.info()

# check the unique values in the column
df['gender'].value_counts()
data['gender'].unique()


# check unique values in a set categorical columns

cols = ['home_ownership', 'grade','verification_status', 'emp_length', 'term', 'addr_state']
for name in cols:
    print(name,':')
    print(object_columns_df[name].value_counts(),'\n')

```

**Exploring and Standardizing Columns**

```python

# to see the columns
df.columns

# to identify a column with unique values
df['Identifier'].is_unique

 # to create a new row with columns names
column_names = file1.columns
data = pd.DataFrame(columns=column_names)

# to standard the headers. Option 1
cols = []
for i in range(len(data.columns)):
    cols.append(data.columns[i].lower())

data.columns = cols

# deleting columns
to_drop = ['Edition Statement','Corporate Author', 'Corporate Contributors'] # list of columns to drop

df.drop(to_drop, inplace=True, axis=1) # option 1
df.drop(columns=to_drop, inplace=True) # option 2
data = data.drop(['tcode', 'domain', 'dob'], axis =1) # Option 3


# rename 1
data = data.rename(columns={ 'controln':'id','hv1':'median_home_val', 'ic1':'median_household_income'}) #first the column_name: then new column_name

# rename 2


# Rearranging columns

data = data[['id', 'state', 'gender', 'median_home_val', 'median_household_income', 'ic2', 'ic3', 'ic4', 'ic5', 'avggift', 'target_d']] # write the column_names in new order


```




---

## Data Manipulation

### Filtering and Subsetting

**using conditions with dataframes**

```python
# lets say we are analyzing data of male donors here
data[data['gender']=='M']
data[data['gender'].isin(['M', 'F'])]
data[(data['gender']=='M') | (data['gender']=='F')] # | = or
data[(data['gender']=='M') & (data['state']=='FL')] # & = and
data[data['target_d']>100]
data[(data['target_d']<100) & (data['gender']=='F')]

```

**using index []**

```python
# to see a particular row with the number of the index i.e. 206
df.loc[206]

# using [[column_names]][index]
df[['gender', 'ic2', 'ic3']][0:10]

# filtering just on the indexes [row,columns]
df.iloc[1:3]
df.iloc[1:10,0:4]
filtered.iloc[[1,2,3,4],[0,2,4]] # to filter particular columns and rows

#reset the index
df = df.reset_index(drop=True)


```


**Data types**

```python
df.dtypes
df._get_numeric_data()
df._get_bool_data()
df.select_dtypes('object')

# to classify numerical and categorical dtypes
df._get_numeric_data()
df._get_bool_data()
df.select_dtypes('object')


# covert the type of a column with astype (float, object, int)
temp['median_home_val'] = temp['median_home_val'].astype('float', errors='ignore')  
# error will be ignored with no values but the no values will not be converted as NaNs
# int and datetime columns can be converted to object to be processed with ReExp

# covert the type of a column to numerics - should also works with categorical
data['ic5'] =  pd.to_numeric(data['ic5'], errors='coerce') # "coerse" does note return error and replaces nan values to NaNs

# changing the type of data


```

### Sorting

To sort the dataframe in ascending (default) or descending order, use ```sort_values``` function. It uses the algorithm ```quicksort``` by default for sorting, though it can be replaced with ```mergesort``` or ```heapsort``` using ```kind``` property.


```python
# example

sorted_guest_df = guest_list_df.sort_values("GoogleKnowlege_Occupation", # sort by column
                                 ascending=False, # enable descending order
                                 kind='heapsort', #sorting algorithm
                                 na_position='last') # keep NaN value at last

```

### Merge and Concatenation

Pandas has ```merge``` function which can be used to combine two dataframes, just like two SQL tables using joins as:


```python
# Merge
sorted_guest_df = pd.merge(guest_list_df.head(3),
                           guest_list_df.tail(3),
                           how='outer',
                           indicator = True)


```


*    ```head``` and ```tail``` will get the three rows from the top and bottom as dataframes.
*    ```outer``` is required to combine two dissimilar (no common rows) dataframes (tables).
*  Pandas also has the ```join``` function which uses ```merge``` internally, though it is more concise and visually specific: ```Left```, ```right``` and ```inner``` join can be passed as parameter.
*    Enabling ```indicator``` will provide information about the dataframe source of each row (left or right).



To combine both dataframes into a new one, use ```concat``` as:

```Python
top_df = guest_list_df.head(3)
bottom_df = guest_list_df.tail(3)
combined_guest_df = pd.concat( [top_df, bottom_df] )

```

* ```Concat``` combines dataframes based on common columns or indexes and can also support ```left``` and ```right``` join. ```concat``` only works with Iterable objects.


### Grouping

Grouping is used to aggregate the data into different categories. A common use case would be to create different types of users (paid/free) or to find the number of guests, having Acting as Group.

```python
guest_group = guest_list_df.groupby('Group')
print(guest_group.get_group('Acting'))
```

**Renaming**

Use ```.rename``` to change the column name Group to Occupation as:

```python
guest_list_df = guest_list_df.rename(columns={'Group':'Occupation'}, index={1:0001})
```


------------
## Data Cleaning

### Dealing with ```NaN``` values

**To verify nun values in the dataframe**

```python

df.isnull().values.any() # verify if any value is null
df.isnull().sum() # display summary of null values per column
df.isnull().sum() / len(df) # to find the percentage of nan values i.e. 0.11717147339205986

round(df.isna().sum()/len(df),4)*100 #shows the percentage of null values in a column


# notnull() function return True for existent values  
df.notnull()


# creating a new dataframe with null values
nulls_df = pd.DataFrame(round(data.isna().sum()/len(data),4)*100)
nulls_df = nulls_df.reset_index()
nulls_df.columns = ['header_name', 'percent_nulls']
columns_drop = nulls_df[nulls_df['percent_nulls']>3]['header_name']  # dummy case with 3
print(columns_drop.values)


```


**To verify and replace null values for categorical variables**

```python

# General Approaches:

# Ignore observation if it is possible
# Replace by most frequent value
# Replace using an algorithm like KNN using the neighbours.
# Predict the observation using a multiclass predictor.
# Treat missing data as just another category

data['gender'].value_counts()
len(data[data['gender'].isna()==True])  # number of missing values
data['gender'] = data['gender'].fillna('F')

```

**To import the data without ```NaN``` values**

While reading data from files, use the ```na_values``` to take a list of values to replace with ```NaN``` as:

```python
guest_list_df = pd.read_csv("daily_show_guests.csv", na_values = ['NA', 'N/A','-']])
```


**To replace NA and NaN values with desired value with ```fillna```**

```Python
product_data={'product': ['E-book', 'Plants'], 'unit_sold': [12, np.NaN]} # need to import numpy as np
product_df = pd.DataFrame(data=product_data)
product_df.fillna(0, inplace=True)

# filling a null values in categorical feature using fillna()
df["Gender"].fillna("No Gender", inplace = True)

# will replace  Nan value in dataframe with value -99  
df.replace(to_replace = np.nan, value = -99)

# to interpolate the missing values
df.interpolate(method ='linear', limit_direction ='forward')

# using dropna() function to delete all columns with nan values  
df.dropna()

# using dropna() to drop rows whose all data is missing or contain null values
df.dropna(how = 'all')

# Dropping columns with at least 1 null value.
df.dropna(axis = 1)

```
* ```dropna()``` function can also be used to remove rows or columns (via axis).
* Dataframe also has functions like ```replace``` to replace specific values or ```rename``` to change the column names.



### Dealing with duplicated values

**drop_duplicates**

Row duplication can be removed using ```drop_duplicates``` function which will remove duplicate values based on column as:

```python

df = pd.DataFrame({'Name':['Pavneet','Pavneet']}, index=[1,2])
df.drop_duplicates(subset='Name',keep='last') # conserving the last duplicated item
temp = temp.drop_duplicates(subset=['state','gender', 'ic2', 'ic3']) # removing duplicated in a subset of  specific columns


```
### Dealing with unneeded characters

**clean a column with RegEx**
```python

extr = df['Date of Publication'].str.extract(r'^(\d{4})', expand=False)

```
* RegEx pattern regex = r'^(\d{4})'
* The \d represents any digit, and {4} repeats this rule four times. The ^ character matches the start of a string, and the parentheses denote a capturing group, which signals to Pandas that we want to extract that part of the regex. (We want ^ to avoid cases where [ starts off the string.)
* Here the string method ```str.extract``` to extract the needed characters.


### Dealing with outliers
```python
# This is done using a box plot which we will cover later
# After identifying the upper limit and the lower limit values for a numerical column
# we can use filters to remove those rows from the dataframe
```

------------
## Data Analysis


**Data Description**

* To calculate various measures like mean, median, standard deviation, min, max, etc, use the ```describe()``` function.
* To describe the information in one row use the ```axis```
* ```axis = 1``` will perform the operation on the values of rows and, by default, it is ```axis=0``` which means column-wide calculations.

```python
product_df = pd.DataFrame(data={'product': ['E-book', 'Plants'],
                                'unit_sold': [2000, 5000],
                                'types': [800, 200]})
product_df.describe()
product_df['unit_sold'].mean()
product_df.mean(axis=1)

# to present the info from DataFrame into a print statement
print('My number is: {one}, and my name is: {two}'.format(one=num,two=name))
print('My number is: {}, and my name is: {}'.format(num,name))
print('data Total Claim Amount Mean: {}'.format(data['total_cla'].mean()))

```





-----------------

## Data Storage

* To export the dataframe to external files use ```to_csv```
* To export to other formats use ```to_parquet```, ```to_sql```, ```to_hdf```, ```to_excel```, ```to_json```, ```to_html```, etc.


```python
product_df = pd.DataFrame(data={'product': ['E-book', 'Plants'],
                        'unit_sold': [2000, 5000],
                        'types': [800, 200]})
product_df.to_csv('aa.csv',
                  index=False, # otherwise will add extra comma at start
                  sep=',',
                  encoding='utf-8)
```

------------
##  
-------------

```python
#
```
-----------------

# Sources

* Source 1. [Pythonic Data Cleaning With Pandas and NumPy](https://realpython.com/python-data-cleaning-numpy-pandas/)
* Source 2. [Data Wrangling with Pandas](https://www.pluralsight.com/guides/data-wrangling-pandas). The The code is available on [pandas-demo repo](https://github.com/fivethirtyeight/data/tree/master/daily-show-guests) for demonstration and practice.
* Source 3. [Complet list with string methods](https://docs.python.org/2.5/lib/string-methods.html)
* Source 4. [Working with Missing Data in Pandas](https://www.geeksforgeeks.org/working-with-missing-data-in-pandas/)
-----------
# To Add

Filter
