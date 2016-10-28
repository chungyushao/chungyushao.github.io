---
layout: post
title: "Numpy basics"
excerpt: "numpy array creation, attribute, access, math operation, broadcasting"
categories: knownotes
tags: [python, numpy]
modified: 2016-10-24
comments: true
share: true
author: chungyu
---

> Mainly from [Python Numpy Tutorial](http://cs231n.github.io/python-numpy-tutorial/#numpy) 

# Array creation
```python
a = np.array([1, 2, 3])
f = np.arange(3)  # array([0, 1, 2])
g = np.arange(3.0) # array([ 0.,  1.,  2.])
h = np.arange(3,7) # array([3, 4, 5, 6])
i = np.arange(3,7,2) # array([3, 5]), 2 is the step size

a = np.zeros((2,2))
b = np.ones((1,2)) # "[[ 1.  1.]]"
c = np.full((2,2), 7) # "[[ 7.  7.]
                      #   [ 7.  7.]]"
d = np.eye(2) # "[[ 1.  0.]
               #  [ 0.  1.]]"                      
e = np.random.random((2,2))
p = np.random.permutation(10)               
```

# Array attributes
```python
a = np.array([1, 2, 3]) # Create a rank 1 array
type(a) # "<type 'numpy.ndarray'>"
a.shape # "(3,)"
a       # "[1, 2, 3]"
b = np.array([[1,2,3],[4,5,6]]) # Create a rank 2 array
b.shape  # "(2, 3)"
b.dtype  # "int64"
b.T  #[[1 4]
 	 # [2 5]
     # [3 6]]
```

# Array accessing
```python
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])

# Use slicing to pull out the subarray consisting of the first 2 rows
# and columns 1 and 2; b is the following array of shape (2, 2):
# [[2 3]
#  [6 7]]
b = a[:2, 1:3]

# A slice of an array is a view into the same data, so modifying it
# will modify the original array.
print a[0, 1]   # Prints "2"
b[0, 0] = 77    # b[0, 0] is the same piece of data as a[0, 1]
print a[0, 1]   # Prints "77"


# accessing element @ (0, 1), (1, 1), (2, 0)
print a[[0, 1, 2], [1, 1, 0]] # Prints [77, 6, 9]
```
###### Mixing integer indexing with slices yields an array of lower rank, while using only slices yields an array of the same rank as the original array


```python
a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
row_r1 = a[1, :]    # Rank 1 view of the second row of a  
row_r2 = a[1:2, :]  # Rank 2 view of the second row of a
print row_r1, row_r1.shape  # Prints "[5 6 7 8] (4,)"
print row_r2, row_r2.shape  # Prints "[[5 6 7 8]] (1, 4)"

col_r1 = a[:, 1]
col_r2 = a[:, 1:2]
print col_r1, col_r1.shape  # Prints "[ 2  6 10] (3,)"
print col_r2, col_r2.shape  # Prints "[[ 2]
                            #          [ 6]
                            #          [10]] (3, 1)"
```

###### Filtering
```python
a = np.array([[1,2], [3, 4], [5, 6]])

bool_idx = (a > 2)  # Find the elements of a that are bigger than 2;
                    # this returns a numpy array of Booleans of the same
                    # shape as a, where each slot of bool_idx tells
                    # whether that element of a is > 2.
            
print bool_idx      # Prints "[[False False]
                    #          [ True  True]
                    #          [ True  True]]"

# We use boolean array indexing to construct a rank 1 array
# consisting of the elements of a corresponding to the True values
# of bool_idx
print a[bool_idx]  # Prints "[3 4 5 6]"

# We can do all of the above in a single concise statement:
print a[a > 2]     # Prints "[3 4 5 6]"
```

# Math operation

###### Basic mathematical functions operate elementwise on arrays
```python
x = np.array([[1,2],[3,4]], dtype=np.float64)
y = np.array([[5,6],[7,8]], dtype=np.float64)

print x + y
print np.add(x, y)
# [[ 6.0  8.0]
#  [10.0 12.0]]

print x - y
print np.subtract(x, y)
# [[-4.0 -4.0]
#  [-4.0 -4.0]]

print x * y
print np.multiply(x, y)
# [[ 5.0 12.0]
#  [21.0 32.0]]

print x / y
print np.divide(x, y)
# [[ 0.2         0.33333333]
#  [ 0.42857143  0.5       ]]

print x ** 2
# [[  1.   4.]
#  [  9.  16.]]

print np.sqrt(x)
# [[ 1.          1.41421356]
#  [ 1.73205081  2.        ]]
```

###### Scalar multiply
Numpy treats scalars as arrays of shape (); these can be broadcast together to each elements
```python
x = np.array([[1,2],[3,4]], dtype=np.float64)

# [[ 2.  4.]
# [ 6.  8.]]
print 2 * x
print x * 2

```


###### `dot`
* inner products of vectors, 
* to multiply a vector by a matrix, and 
* to multiply matrices. 

```python
v1 = np.array([9,10])
v2 = np.array([11, 12])

# Inner product of vectors; both produce 219
print v1.dot(v2)
print np.dot(v1, v2)

M1 = np.array([[1,2],[3,4]])
M2 = np.array([[5,6],[7,8]])

# Matrix / vector product; both produce the rank 1 array [29 67]
print M1.dot(v1)
print np.dot(M1, v1)

# Matrix / matrix product; both produce the rank 2 array
# [[19 22]
#  [43 50]]
print M1.dot(M2)
print np.dot(M1, M2)
```

###### `sum`
```python
x = np.array([[1,2],[3,4]])

print np.sum(x)  # Compute sum of all elements; prints "10"
print np.sum(x, axis=0)  # Compute sum of each column; prints "[4 6]"
print np.sum(x, axis=1)  # Compute sum of each row; prints "[3 7]"
```

# [Broadcasting](http://scipy.github.io/old-wiki/pages/EricsBroadcastingDoc)
```python

# Compute outer product of vectors
v = np.array([1,2,3])  # v has shape (3,)
w = np.array([4,5])    # w has shape (2,)
# To compute an outer product, we first reshape v to be a column
# vector of shape (3, 1); we can then broadcast it against w to yield
# an output of shape (3, 2), which is the outer product of v and w:
# [[ 4  5]
#  [ 8 10]
#  [12 15]]
print np.reshape(v, (3, 1)) * w

# Add a vector to each row of a matrix
x = np.array([[1,2,3], [4,5,6]])
# x has shape (2, 3) and v has shape (3,) so they broadcast to (2, 3),
# giving the following matrix:
# [[2 4 6]
#  [5 7 9]]
print x + v

# Add a vector to each column of a matrix
# x has shape (2, 3) and w has shape (2,).
# If we transpose x then it has shape (3, 2) and can be broadcast
# against w to yield a result of shape (3, 2); transposing this result
# yields the final result of shape (2, 3) which is the matrix x with
# the vector w added to each column. Gives the following matrix:
# [[ 5  6  7]
#  [ 9 10 11]]
print (x.T + w).T
# Another solution is to reshape w to be a row vector of shape (2, 1);
# we can then broadcast it directly against x to produce the same
# output.
print x + np.reshape(w, (2, 1))

# Multiply a matrix by a constant:
# x has shape (2, 3). Numpy treats scalars as arrays of shape ();
# these can be broadcast together to shape (2, 3), producing the
# following array:
# [[ 2  4  6]
#  [ 8 10 12]]
print x * 2
```
