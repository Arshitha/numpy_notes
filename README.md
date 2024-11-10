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
- ***core*** to NumPy library
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


[Notebook](https://github.com/Arshitha/numpy_notes/blob/main/quickstart.ipynb) that partially follows along the [Quickstart guide](https://numpy.org/doc/stable/user/quickstart.html) on the official NumPy docs site. Rest of the notes are in this README. 

## Basic operations
- Arithmetic operators on arrays apply _elementwise_. A new array is created and filled with the result.
- Unlike in many matrix languages, the product operator `*` operates elementwise in NumPy arrays. The matrix product can be performed using the `@` operator (in python >=3.5) or the `dot` function or method.
```python
A = np.array([[1, 1],
              [0, 1]])
B = np.array([[2, 0],
              [3, 4]])
A * B     # elementwise product
array([[2, 0],
       [0, 4]])
A @ B     # matrix product
array([[5, 4],
       [3, 4]])
A.dot(B)  # another matrix product
array([[5, 4],
       [3, 4]])
```
- Some operations, such as `+=` and `*=`, act in place to modify an existing array rather than create a new one.
- When operating with arrays of different types, the type of the resulting array corresponds to the more general or precise one (a behavior known as upcasting).
```python
>> a = np.ones(3, dtype=np.int32)
>> b = np.linspace(0, pi, 3)

>> b.dtype.name
>> 'float64'

>> c = a + b
>> c
array([1.        , 2.57079633, 4.14159265])

>> c.dtype.name
'float64'

>> d = np.exp(c * 1j)
>> d
array([ 0.54030231+0.84147098j, -0.84147098+0.54030231j,
       -0.54030231-0.84147098j])

>> d.dtype.name
'complex128'
```

- Many unary operations, such as computing the sum of all the elements in the array, are implemented as methods of the `ndarray` class.
```python
>> a = rg.random((2, 3))
>> a
array([[0.82770259, 0.40919914, 0.54959369],
       [0.02755911, 0.75351311, 0.53814331]])

>> a.sum()
3.1057109529998157
>> a.min()
0.027559113243068367
```

- By default, these operations apply to the array as though it were a list of numbers, regardless of its shape. However, by specifying the `axis` parameter you can apply an operation along the specified axis of an array:
```python
>> b = np.arange(12).reshape(3, 4)
>> b
array([[ 0,  1,  2,  3],
       [ 4,  5,  6,  7],
       [ 8,  9, 10, 11]])
>> b.sum(axis=0)  # sum of each column
array([12, 15, 18, 21])
>> b.min(axis=1)     # min of each row
array([0, 4, 8])
>> b.cumsum(axis=1)  # cumulative sum along each row
array([[ 0,  1,  3,  6],
       [ 4,  9, 15, 22],
       [ 8, 17, 27, 38]])
```


## Universal functions

NumPy provides familiar mathematical functions such as sin, cos, and exp. In NumPy, these are called “universal functions” (`ufunc`). Within NumPy, these functions operate element-wise on an array, producing an array as output.

## Sources 
1. [What is NumPy?](https://numpy.org/doc/stable/user/whatisnumpy.html)
2. [Quickstart guide](https://numpy.org/doc/stable/user/quickstart.html)
