# Notes Python Udemy
## Pandas

## Series


* series work with list of strings, list of numbers and arrays
* with dictionaries series takes the key as index and the value in the first column
* series also accept operations as parameters
* Information in series can be get with Index


```python
labels = ['a','b','c']
my_list = [10,20,30]
arr = np.array([10,20,30])
d = {'a':10,'b':20,'c':30}

# make a series with default Index
pd.Series(data=my_list)
#Result
0    10
1    20
2    30
dtype: int64

# make a series with specific index
pd.Series(data=my_list,index=labels)
# Result
a    10
b    20
c    30
dtype: int64

# other option. Here the variables are in the order of the parameters, therefore we do need to specified the parameters
pd.Series(my_list,labels)


#dictionaries
pd.Series(d)
# Result
a    10
b    20
c    30
dtype: int64

# Operations
pd.Series(np.arange(0,10)*2)
# Result
0    0
1    2
2    4
3    6
4    8
dtype: int64


# getting info from series with Index

ser1 = pd.Series([1,2,3,4],index = ['USA', 'Germany','USSR', 'Japan'])      

ser2 = pd.Series([1,2,5,4],index = ['USA', 'Germany','Italy', 'Japan'])        

ser1['USA']
#Result
1

# Operation among series
ser1 + ser2
# result
Germany    4.0
Italy      NaN
Japan      8.0
USA        2.0
USSR       NaN
dtype: float64

ser1*ser2
# Result
Germany     4.0
Italy       NaN
Japan      16.0
USA         1.0
USSR        NaN
dtype: float64

```


## DataFrames

* DataFrame Columns are just Series


```python
df = pd.DataFrame(randn(5,4),index='A B C D E'.split(),columns='W X Y Z'.split())

# Pass a column name to get one column
df['W']

# Pass a list of column names to get more than one column
df[['W','Z']]

# creating a new column
df['new'] = df['W'] + df['Y']

# dropping columns directly in the DataFrame. Inplace has to be specified
df.drop('new',axis=1,inplace=True)


# dropping columns directly in the DataFrame. Inplace has to be specified
df1 = df.drop("new", axis=1)

# Can also drop rows specified the row Index and axis=0

df.drop('E',axis=0)


```

### Selecting Rows
* two methods=
  * loc[]
  * iloc[]



```python
# loc[] return all the values in the row with index "A"
df.loc['C']

# iloc[] can receive the int index value even though the row index is a string
df.iloc[2]


```



### Selecting subset of rows and columns

```python
# loc[] can receive the value of [row,column] like the index

df.loc['C',"Y"] # to select the values in df[C,Y]

# loc[] to get a row or a subset of rows passing a list rows and them columns
df.loc[['A','B'],['W','Y']]


```

### Conditional Selection

* Conditional selection using bracket notation in pandas, very similar to numpy:


```python
# boolean to check everything bigger than 0  
df>0

 # return a DataFrame with the values that have the Condition. Where the condition is not present it returns nan values
df[df>0]

# returns a DataFrame without the rows that does not have the Conditional
df[df['W']>0]


# return the column Y with the values that has the condition in W
# first the condition in [] than the column name in []

df[df['W']>0]['Y']
df[df['W']>0][['Y','X']]

# columns names can be also stored in a variable and then pass the variable as the columns

boolser = df['W']>0
result = df[boolser] # new DataFrame with the boolean condition
my_columns = ["X", "Y"]
result = [my_columns] # pass the boolean condition to the columns in my_columns


# For two/multiple conditions and / or operators do not work.
# you have to use | and & with parenthesis:
df[(df['W']>0) & (df['Y'] > 1)]

```

### More Index Details


```python

#  Reset to default 0,1...n index
# the new index makes the actual index to a column
# to make it works in the df inplace = True
df.reset_index()

# change index having a list
# make a list
newind = 'CA NY WY OR CO'.split()
# new column with the index values
df['States'] = newind
# set the index to the new columns. The default index is deleted.
# to make it permanent inplace = True
df.set_index("States",  inplace = True)

```

### Multi-Index and Index Hierarchy¶

* Let us go over how to work with Multi-Index, first we'll create a quick example of what a Multi-Indexed DataFrame would look like:
*  Multi-Indexed DataFrame can be indexed with 2 methods
  * loc[]
  * xs()

```python

# Index Levels
outside = ['G1','G1','G1','G2','G2','G2']
inside = [1,2,3,1,2,3]
hier_index = list(zip(outside,inside)) # makes tuples with two values each one
hier_index = pd.MultiIndex.from_tuples(hier_index) # males different levels of index
# Result of MultiIndex
MultiIndex(levels=[['G1', 'G2'], [1, 2, 3]],
           labels=[[0, 0, 0, 1, 1, 1], [0, 1, 2, 0, 1, 2]])


# to get values in a particular Index

df.loc["G1"] # returns the tree values in "G1" with subindex 1,2,3

# I can indexing after a selection
df.loc['G1'].loc[1] # returns the values at "G1" located in [1]


# To name the index use method index.names
df.index.names = ['Group','Num']



# Indexing MultiIndex DataFrame with cross section from the outside to the inside.
df.xs('G1') # returns only "G1"
df.xs(['G1',1]) # returns the values in external index "G1" and inner index 1

# to get all values with particular inner indexed unsing the name of the subindex. Eje. inner index = 1
df.xs(1, level=Num)

```

##  Missing Data

```python
# drop nan of all columns with one or more nan
#  dropna() returns only the rows without nan values
df.dropna()


#  dropna(axis=1) returns only the columns without nan values
df.dropna(axis=1)

# dropna() with threshold returns the rows that contain at least 2 values that are not nan values
df.dropna(thresh=2)

# to fill the nan values with a particular value
df.fillna(values= "filled values")


# to fill the nan values with the mean() of the column "A"
df.fillna(values= df.["A"].mean())
# the same but afecting only column "A"
df["A"].fillna(values= df.["A"].mean())

```

## GroupBy

* groupby method allows you to group rows of data together and call aggregate functions
* use the .groupby() method to group rows together based off of a column name.


```python
import pandas as pd
# Create dataframe
data = {'Company':['GOOG','GOOG','MSFT','MSFT','FB','FB'],
       'Person':['Sam','Charlie','Amy','Vanessa','Carl','Sarah'],
       'Sales':[200,120,340,124,243,350]}

df = pd.DataFrame(data)

# Groupby the company names
#this is only a broupby object, it has to be store in a variable and operated as normal
df.groupby("Company")

bycomp = df.groupby("Company")

# we can pass aggregate methods to the variable with the groupby
# returns the mean of the variable with the previous groupby
bycomp.mean()
bycomp.std()
bycomp.max()
bycomp.min()
bycomp.count() # count the values in each partition of the groupby
bycomp.describe() # describe info in aech partition of the GroupBy
bycomp.describe().transpose() # transpose present the describe info in horizontal format like a pivot table!!
bycomp.describe().transpose()['GOOG'] # to get only the describe info of one column of the variable with the bycomp. filtering!!


#  the average (mean) BasePay of all employees per year? (2011-2014)
# first the mean of all columns in DataFrame, then the column needed
df.groupby('Year').mean()['BasePay']
```


## Merging,Joining,and Concatenating


### Concatenating

* Concatenation basically glues together DataFrames.
* Keep in mind that **dimensions should match along the axis** you are concatenating on.
* use pd.concat and **pass in a list of DataFrames** to concatenate together

```python
# to concatenate one df behind another
pd.concat([df1,df2,df3])

# to concatenate one df next to another
pd.concat([df1,df2,df3], axis=1)

```


### Merge

* merge DataFrame based on **a column or set of columns** of reference like the key column in SQL

```python
# inner join is default Joining
pd.merge(left,right,how='inner',on='key')

# merging in two different keys
pd.merge(left, right, on=['key1', 'key2'])

# outher, right & left merge
pd.merge(left, right, how= "outer" , on= ["key1", "key2"])
pd.merge(left, right, how='right', on=['key1', 'key2'])
pd.merge(left, right, how='left', on=['key1', 'key2'])

```
### Join method on DataFrame

* Joining is based on **the index** of the DataFrames
* It combines the columns of two potentially differently-indexed DataFrames into a single result DataFrame.
* per default it joins in inner join

```python
# name of the DataFrame.join(name of the second DataFrame)
left.join(right)

# when the join is specified
left.join(right, how='outer')

```


## Operations

### finding unique values
* .unique()
* .nunique()
* .value_counts()

```python

# to get the unique values
df["col2"].unique()

# to get number of unique values
df["col2"].nunique()

# to return how many times a value appears in a column
df["col2"].value_counts()


```



### Selecting Data

* filtering info by passing a conditional selection statement (boolean)
* use **&** or **|** to select from DataFrame combining boolean statements from multiple columns

```python
#Select from DataFrame using a boolean in a column
df[(df['col1']>2)]

#Select and store a DataFrame combining boolean statements from multiple columns with & and |
newdf = df[(df['col1']>2) & (df['col2']==444)]


```

### Apply method
* to apply a function as a method  
* apply() receives function's names, other parameters different than functions (len) and lambda expressions.

```python

def times2(x):
    return x*2
# apply with a function's name
df["col2"].apply(times2)

# apply also receives other parameters different than functions
df["col2"].apply(len)

# apply to pass lambda expressions
df["col2"].apply(lambda x: x*2)


```

### Permanently Removing a Column

* del delete a column, faster than .drop()
```python

del df["col1"]

```


### Index and Columns names

* del delete a column, faster than .drop()

```python
#  Get column and index names
df.columns

# get the index
df.index

```

### sort values having a column as reference

* to sort values ascending is per default

```python
# inplace is false per default  
df.sort_values(by="col2")

```


### Pivot table


```python
data = {'A':['foo','foo','foo','bar','bar','bar'],
     'B':['one','one','two','two','one','one'],
       'C':['x','y','x','y','x','y'],
       'D':[1,3,2,5,4,1]}

df = pd.DataFrame(data)



#
df.pivot_table(values="D", index=["A", "B"], columns=["C"])


```


## Data Input and Output

### csv
  * pd.read_ + shit tap shows all the formats you can import
  * pd.to_ + shit tap shows all the formats you can export

### Excel
```
conda install -c anaconda xlrd
conda install -c anaconda openpyxl
```
  * Pandas can read and write excel files, keep in mind, this only imports data.
  * Not formulas or images, having images or macros may cause this read_excel method to crash.


```python
# csv input
df = pd.read_csv("example")

# csv output.
# Index false to avoid saving the index as a column
df.to_csv('example',index=False)

# Excel:  file, specific sheet
pd.read_excel('Excel_Sample.xlsx',sheet_name='Sheet1', index_col=0)
#take care with the index
df.to_excel("Excel_Sample.xlxs", sheetname= "Sheet1")

```


### HTML
* intall
    ```
conda install -c anaconda lxml
conda install -c anaconda html5lib
conda install -c anaconda BeautifulSoup4
```
* HTML files as read as a list that can be search with index methods  


```python
df = pd.read_html('http://www.fdic.gov/bank/individual/failed/banklist.html')
# to call a table in  the first position of the html file
df[0]
```


### SQL
* Database abstraction is provided by SQLAlchemy if installed.
* In addition you will need a driver library for your database.
* Examples of such drivers are **psycopg2** for **PostgreSQL** or **pymysql** for **MySQL**.
* For SQLite this is included in Python’s standard library by default. You can find an overview of supported drivers for each SQL dialect in the **SQLAlchemy** docs.

The key functions of **SQLAlchemy** are:
* Read SQL database table into a DataFrame.
**read_sql_table(table_name, con[, schema, ...])**
* Read SQL query into a DataFrame.
  **read_sql_query(sql, con[, index_col, ...])**
* Read SQL query or database table into a DataFrame.
  **read_sql(sql, con[, index_col, ...])**
* Write records stored in a DataFrame to a SQL database.
  **DataFrame.to_sql(name, con[, flavor, ...])**


```python
#
```
