---
tags:
  - python
  - ML
  - DL
---
- NumPy is a scientific computing python library. 
- It provides
	- a multidimensional array object
	- derived objects (eg. masked arrays and matrices)
	- a bunch of methods for operations on arrays such as:
		- mathematical and logical operations
		- basic statistical operations
		- linear Algebra
		- discrete fourier transformation
		- shape manipulation and more

## `ndarray` object 
- ***core*** of NumPy package
- encapsulates n-dimensional arrays of homogeneous data types 
- many of the operations on these arrays are performed in compiled code performance aka very fast 

## Python lists vs NumPy arrays
1. np arrays have fixed size at creation unlike python lists that can grow dynamically. Changing the size of an `ndarray` will create a new array and delete the original. 
2. Elements in a NumPy array are all required to be of the same data type, and thus will be the same size in memory. The exception: one can have arrays of (Python, including numpy) objects, thereby allowing for arrays of different sized elements.
3. np arrays facilitate advanced mathematical and other types of operations on large numbers of data. Typically, such operations are executed more efficiently and with less code than is possible using Python’s built-in sequences.
4. Many of the scientific and mathematical Python-based packages are using NumPy arrays; though these typically support Python-sequence input, they convert such input to NumPy arrays prior to processing, and they often output NumPy arrays. In other words, in order to efficiently use much (perhaps even most) of today’s scientific/mathematical Python-based software, just knowing how to use Python’s built-in sequence types is insufficient - one also needs to know how to use NumPy arrays.

## Why is NumPy fast?
- NumPy is nearly as fast as code written in C while combining the ease of writing code in Python. 
- For example, element-wise multiplication of two 1-D arrays in Python is simple to do but as the size of the arrays increase, so does the computational time of looping in python. Writing the same logic in C would be faster but having to do it for multidimensional arrays would be tedious. For example, these example code snippets demonstrate each case:
- In Python, 1D element wise multiplication of `a` and `b`
```python
c = []
for i in range(len(a)):
	c.append(a[i]*b[i])
```
- If `a` and `b` contained millions of numbers, the python for loop would be computationally very expensive. In C, this would be a lot quicker. 
```c
for (i = 0; i < rows; i++) {
  c[i] = a[i]*b[i];
}
```
>This saves all the overhead involved in interpreting the Python code and manipulating Python objects, but at the expense of the benefits gained from coding in Python.

- Writing C code becomes tedious even just for 2D arrays. E.g.
```C
for (i = 0; i < rows; i++) {
  for (j = 0; j < columns; j++) {
    c[i][j] = a[i][j]*b[i][j];
  }
}
```

- In NumPy, this can simply be achieved as and at near C speeds
```python
c = a * b
```
>NumPy gives us the best of both worlds: element-by-element operations are the “default mode” when an [`ndarray`](https://numpy.org/doc/stable/reference/generated/numpy.ndarray.html#numpy.ndarray "numpy.ndarray") is involved, but the element-by-element operation is speedily executed by pre-compiled C code.


Core to NumPy's computational efficiency are:
1. Vectorization - Treats arrays as a single vector object and therefore, abstracts out the element-wise indexing for multiplication. This is happening behind the scenes ofc, but more efficiently in optimized pre-compiled C code. Advantages of vectorized code:
	1. vectorized code is more concise and easier to read
	2. fewer lines of code generally means fewer bugs
	3. the code more closely resembles standard mathematical notation (making it easier, typically, to correctly code mathematical constructs)
	4. vectorization results in more “Pythonic” code. Without vectorization, our code would be littered with inefficient and difficult to read `for` loops.
2. Broadcasting - Term used to refer to the implicit element-by-element behavior of operations. 




## Sources 
1. [What is NumPy?](https://numpy.org/doc/stable/user/whatisnumpy.html)
2. 