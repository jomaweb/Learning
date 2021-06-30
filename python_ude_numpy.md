# Notes Python Udemy
## NumPy

Every method can be import with Numpy and thus be called

```python
from numpy.random import randint
np.random.randint(1,100) # can be called only
randint(1,100)

```

## Arrays

* the main difference between one dimensional and two dimensional arrays is the [].
  * array([to one dimension]) --> []
  * array([[to two dimensions]]) --> [[]]

```python
import numpy as np

# convert a list in array one dimension
list = [1,2,3]
arr = np.array(list)


# convert a list in array of two dimensions
list2 = [[1,2,3], [4,5,6],[7,8,9]]
arr = np.array(list2)


# to create an array starting in 0 to 9 containing only even numbers
np.arange(0,10,2)


# to create arrays of zeros
# option 1
np.zeros(3) # --> array([0.,0.,0.])
# option 2
np.zeros((3,3)) # --> a two dimensional array containing 3 columns and rows of zeros


# to create arrays of ones
np.ones(3)

```

### linspace & identity matrix
* linspace() returns an one dimensional array with values having the same distance among them
* identity matrix: a two dimensional array with the same number of columns and rows and the diagonal has value 1 while the rest has value 0

```python
# create an array with 100 values in a range from 0 to 5
np.linspace(0,5,100)

# identity matrix only receive one parameter
np.eye(4)


```


### random
  * rand(x) = an one dimensional array with x random values from 0 to 1.
  * rand(x,y) = a two dimensional array with x and y random values from 0 to 1.
  * randn(x) =  an one dimensional array with x random values having a normal distribution.
  * randn(x,y) = for two dimensional arrays  
  * randint(A,B,C) = return an C amount of random number between A and B, but not containing B. Without C return one value per default.

```python

# rand(x) & rand(x,y)
np.random.rand(5)
np.random.rand(5,5)

# randint(A,B)
np.random.randint(1,100) # --> 60
np.random.randint(1,100, 10) # --> 10 random values between 1 & 99
```


### reshape, min, max, shape, dtype & copy
* to reshape the distribution of a variable with an one dimensional array of values x into a two dimensional array  
```python

arr = np.arange(25) # 25 values in on e dimensional arrays
arr.reshape(5,5) # return a two dimensional array with 5 columns and 5 rows. The parameters should fill out the number of digits in the original array

arr2 = np.random.randint(1,100, 10)
arr2.min() # show the minimum value in the array
arr2.argmin() # show the index where the minimum value is placed in the array

arr2.max() # show the maximum value in the array
arr2.argmax() # show the index where the maximum value is placed in the array

arr2.shape # to return the shape of the array

arr2.dtype # return the data type of the array

#To get a copy, need to be explicit
arr_copy = arr.copy()
arr_copy

```



## Indexing and Selection
### Broadcasting
* A way to overcome this is to duplicate the smaller array so that it is the dimensionality and size as the larger array. This is called array broadcasting and is available in NumPy when performing array arithmetic, which can greatly reduce and simplify your code. [Broadcasting](https://machinelearningmastery.com/broadcasting-with-numpy-arrays/)
* Broadcast affect the original array changing the values or a subset of them to a particular digit.
* If I set a new variable with a particular broadcast of an array. The array will not be modified as in the variable. The array is not copied, it's a view of the original array! This avoids memory problems
arr[0:5] = 5



```python
#Creating sample array
arr = np.arange(0,11) # --> array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10])

# from 0 to 4 will be convert to 100.  
arr[0:5] = 100  # --> array([100, 100, 100, 100, 100,   5,   6,   7,   8,   9,  10])

```



### Indexing a 2D array (matrices)

* To get a int in a "D array the general format is arr_2d[row][col] or arr_2d[row,col]. I recommend usually using the comma notation for clarity.


```python
arr_2d = np.array(([5,10,15],[20,25,30],[35,40,45]))
# Result
# Index Col 0   1   2  | Index Row    
# array([[ 5, 10, 15],   0
#        [20, 25, 30],   1
#        [35, 40, 45]])  2

# to get the 30
arr_2d[1,2]

# to get a slice of the matrix
arr_2d[:2,1:] # slice the first row index (exclude the second row index) starting from the first column index   
# result
# array([[10, 15],
#        [25, 30]])

```

### Boolean Selection

* Selection based off of comparison operators can be defined with <, >, =, !


```python
arr = np.arange(1,11)
# array([ 1,  2,  3,  4,  5,  6,  7,  8,  9, 10])

# This is a condition to select info in the array
arr > 4
# array([False, False, False, False,  True,  True,  True,  True,  True,  True], dtype=bool)

# store the selection in a variable
bool_arr = arr>4

# apply the selection to the original arrays
arr[bool_arr]
# array([ 5,  6,  7,  8,  9, 10])

# the condition can be also called direct in the array variable

arr[arr>2]
# array([ 3,  4,  5,  6,  7,  8,  9, 10])

x = 2
arr[arr>x]
# array([ 3,  4,  5,  6,  7,  8,  9, 10])

# setting new 2D array
arr_j = np.arange(50).reshape(5,10)
# array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9],
  #      [10, 11, 12, 13, 14, 15, 16, 17, 18, 19],
  #      [20, 21, 22, 23, 24, 25, 26, 27, 28, 29],
  #      [30, 31, 32, 33, 34, 35, 36, 37, 38, 39],
  #      [40, 41, 42, 43, 44, 45, 46, 47, 48, 49]])

# select smaller than  24
[arr_j < 24]
# return a list of values --> array([ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23])

# select from 25 to 28
arr_j[2,5:9]
# return a set of values --> array([25, 26, 27, 28])
```


## NumPy Operations

Arithmetic to perform array with array arithmetic, or scalar with array arithmetic.



```python
# setting up an array
import numpy as np
arr = np.arange(0,10)
# array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])

# Addition
arr + arr
# array([ 0,  2,  4,  6,  8, 10, 12, 14, 16, 18])

arr + 100
# array([100, 101, 102, 103, 104, 105, 106, 107, 108, 109])

# product -- >  arr * arr
# substraction --> arr - arr

# division
# Warning on division by zero, but not an error!
# Just replaced with nan

arr/arr
# array([nan,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.,  1.])

# Also warning, but not an error instead infinity
1/arr

# array([        inf,  1.        ,  0.5       ,  0.33333333,  0.25  , 0.2,  0.16666667,  0.14285714,  0.125     ,  0.11111111])


# exponents
arr**3
# array([  0,   1,   8,  27,  64, 125, 216, 343, 512, 729])

```

## Universal Array Functions

Numpy comes with many [universal array functions](http://docs.scipy.org/doc/numpy/reference/ufuncs.html), which are essentially just mathematical operations you can use to perform the operation across the array.

```python
#Taking Square Roots
np.sqrt(arr)
#array([ 0.        ,  1.        ,  1.41421356,  1.73205081,  2.        ,
#        2.23606798,  2.44948974,  2.64575131,  2.82842712,  3.        ])



#Calculating exponential (e^)

np.exp(arr)
#array([  1.00000000e+00,   2.71828183e+00,   7.38905610e+00,
#         2.00855369e+01,   5.45981500e+01,   1.48413159e+02,
#         4.03428793e+02,   1.09663316e+03,   2.98095799e+03,
#         8.10308393e+03])


np.max(arr) #same as arr.max()
# 9

# calculating Sinus |
np.sin(arr)
#array([ 0.        ,  0.84147098,  0.90929743,  0.14112001, -0.7568025 ,
#       -0.95892427, -0.2794155 ,  0.6569866 ,  0.98935825,  0.41211849])


# Calculate logarithm
np.log(arr)
# RuntimeWarning: divide by zero encountered in log  if __name__ == '__main__':
#array([       -inf,  0.        ,  0.69314718,  1.09861229,  1.38629436,
#        1.60943791,  1.79175947,  1.94591015,  2.07944154,  2.19722458])

# Get the standard deviation
np.std(arr)
# 2.8722813232690143
# # Get the standard deviation in int
np.std(mat, dtype=int)
# 2
```




```python
0
```
