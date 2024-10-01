**© 2021 Scott A. Bruce. Do not distribute.**

# Data analytics with pandas and NumPy

This notebook follows Chapter 10 in the Python Workshop textbook.  Herein, we will use the `NumPy` and `pandas` packages for data analysis in Python.  Taken together, pandas and NumPy are masterful at handling big data. They are built for speed, efficiency, readability, and ease of use.

**Pandas** provide you with a unique framework to view and modify data. Pandas handles all data-related tasks such as creating DataFrames, importing data, scraping data from the web, merging data, pivoting, concatenating, and more.

**NumPy**, short for Numerical Python, is more focused on computation. NumPy interprets the rows and columns of pandas DataFrames as matrices in the form of NumPy arrays. When computing descriptive statistics such as the mean, median, mode, and quartiles, NumPy is blazingly fast.

## 1. NumPy and basic stats

NumPy is designed to handle big data swiftly. It includes the following essential components according to the NumPy documentation: 

- A powerful n-dimensional array object 
- Sophisticated (broadcasting) functions 
- Tools for integrating C/C++ and Fortran code 
- Useful linear algebra, Fourier transform, and random number capabilities 

From NumPy documentation: The term broadcasting describes how numpy treats arrays with different shapes during arithmetic operations. Subject to certain constraints, the smaller array is “broadcast” across the larger array so that they have compatible shapes. Broadcasting provides a means of vectorizing array operations so that looping occurs in C instead of Python. It does this without making needless copies of data and usually leads to efficient algorithm implementations. There are, however, cases where broadcasting is a bad idea because it leads to inefficient use of memory that slows computation.

Going forward, instead of using lists, you will use NumPy arrays. NumPy arrays are the basic elements of the NumPy package. NumPy arrays are designed to handle arrays of any dimension. 

Numpy arrays can be indexed easily and can have many types of data, such as float, int, string, and object, but the **types must be consistent** to improve speed.


### 1.1 Exercise 128: Converting Lists to NumPy Arrays


```python
%matplotlib inline
import matplotlib.pyplot as plt
import seaborn as sns
import numpy as np
import pandas as pd
import statsmodels.api as sm
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```


```python
test_scores = [70,65,95,88]
type(test_scores)
```




    list



**Note** Now that numpy has been imported, you can access all numpy methods, such as numpy arrays. Type `np.` + Tab on your keyboard to see the breadth of options. You are looking for an array.


```python
scores = np.array(test_scores)
type(scores)
```




    numpy.ndarray



### 1.2 Exercises 129-131: Summary statistics


```python
scores.mean()
```




    79.5




```python
income = np.array([75000, 55000, 88000, 125000, 64000, 97000])
income.mean()
```




    84000.0




```python
income = np.append(income, 12000000)
income.mean()
```




    1786285.7142857143




```python
np.median(income)
```




    88000.0



**Note** Median here is not a method of `np.array`, but it is a method of `numpy`. (The mean may be computed in the same way, as a method of numpy.)



```python
np.mean(income)
```




    1786285.7142857143




```python
income.std()
scores.std()
```




    4169786.007331644






    12.379418403139947




```python
scores.max()
scores.min()
scores.sum()
```




    95






    65






    318



## 2. Matrices

A DataFrame is generally composed of rows, and each row has the same number of columns. From one point of view, it's a two-dimensional grid containing lots of numbers. It can also be interpreted as a list of lists, or an array of arrays.

NumPy has methods for creating matrices or n-dimensional arrays. One option is to place random numbers between 0 and 1 into each entry, as follows.

### 2.1 Exercise 132: Matrices


```python
np.random.seed(seed=60) 
np.random.rand
random_square = np.random.rand(5,5) 
random_square


zero_square = np.zeros((5,5))
zero_square
```




    <function RandomState.rand>






    array([[0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ],
           [0.39825396, 0.37941492, 0.01058154, 0.1703656 , 0.12339337],
           [0.69240128, 0.87444156, 0.3373969 , 0.99245923, 0.13154007],
           [0.50032984, 0.28662051, 0.22058485, 0.50208555, 0.63606254],
           [0.63567694, 0.08043309, 0.58143375, 0.83919086, 0.29301825]])






    array([[0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.],
           [0., 0., 0., 0., 0.]])




```python
x=np.empty((5,5))
```

**Note** The `np.random.seed()` ensures that the same collection of random numbers are drawn every time, which is important for reproducibility.  You can set your own seed.


```python
random_square = np.random.rand(5,5) 
random_square
random_square = np.random.rand(5,5) 
random_square
```




    array([[0.98792976, 0.54719104, 0.95938589, 0.35628708, 0.51722779],
           [0.9880134 , 0.20425203, 0.97605287, 0.03992927, 0.01932668],
           [0.19401719, 0.01158554, 0.44687269, 0.65193731, 0.71131638],
           [0.25500845, 0.76391853, 0.9665803 , 0.53475831, 0.43086792],
           [0.49989585, 0.76616026, 0.32433834, 0.1636726 , 0.61835101]])






    array([[0.63983636, 0.78571794, 0.92774425, 0.83809046, 0.9628113 ],
           [0.55285888, 0.02937153, 0.14328132, 0.47193255, 0.68210871],
           [0.68834289, 0.72607081, 0.98941285, 0.39267928, 0.51336459],
           [0.92091723, 0.38656119, 0.44692699, 0.44778805, 0.26328203],
           [0.92882461, 0.51603348, 0.66412827, 0.83228683, 0.0034176 ]])




```python
np.random.seed(seed=60) 
random_square = np.random.rand(5,5) 
random_square
np.random.seed(seed=60) 
random_square = np.random.rand(5,5) 
random_square
```




    array([[0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ],
           [0.39825396, 0.37941492, 0.01058154, 0.1703656 , 0.12339337],
           [0.69240128, 0.87444156, 0.3373969 , 0.99245923, 0.13154007],
           [0.50032984, 0.28662051, 0.22058485, 0.50208555, 0.63606254],
           [0.63567694, 0.08043309, 0.58143375, 0.83919086, 0.29301825]])






    array([[0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ],
           [0.39825396, 0.37941492, 0.01058154, 0.1703656 , 0.12339337],
           [0.69240128, 0.87444156, 0.3373969 , 0.99245923, 0.13154007],
           [0.50032984, 0.28662051, 0.22058485, 0.50208555, 0.63606254],
           [0.63567694, 0.08043309, 0.58143375, 0.83919086, 0.29301825]])




```python
#Indexing, slicing, and accessing

#First row
random_square[0]
random_square[0,:]

#First column
random_square[:,0]

#First entry
random_square[0,0]
random_square[0][0]

#Third row, fourth column
random_square[2,3]

#Multiple rows and columns
random_square[0:2,:]
random_square[:,3:5]
random_square[[0,3,4],:]
```




    array([0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ])






    array([0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ])






    array([0.30087333, 0.39825396, 0.69240128, 0.50032984, 0.63567694])






    0.30087333004661876






    0.30087333004661876






    0.9924592256795676






    array([[0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ],
           [0.39825396, 0.37941492, 0.01058154, 0.1703656 , 0.12339337]])






    array([[0.66574957, 0.5669708 ],
           [0.1703656 , 0.12339337],
           [0.99245923, 0.13154007],
           [0.50208555, 0.63606254],
           [0.83919086, 0.29301825]])






    array([[0.30087333, 0.18694582, 0.32318268, 0.66574957, 0.5669708 ],
           [0.50032984, 0.28662051, 0.22058485, 0.50208555, 0.63606254],
           [0.63567694, 0.08043309, 0.58143375, 0.83919086, 0.29301825]])




```python
#Matrix mean
random_square.mean()

#First row mean
random_square[0].mean()

#Last column mean
random_square[:,-1].mean()
```




    0.42917627159618377






    0.4087444389228477






    0.35019700684996913



### 2.2 Computation time for large matrices

Now that you have gotten a hang of creating random matrices, you can see how long it takes to generate a large matrix and compute the mean:


```python
%%time 
np.random.seed(seed=60) 
big_matrix = np.random.rand(100000, 100)
```

    CPU times: user 54.5 ms, sys: 162 ms, total: 217 ms
    Wall time: 214 ms
    


```python
%%time 
np.random.seed(seed=60) 
big_matrix = np.random.rand(100000, 100)
big_matrix.mean()
```

    CPU times: user 56.9 ms, sys: 171 ms, total: 228 ms
    Wall time: 226 ms
    




    0.5001528493018508



In the next exercise, you will create arrays using NumPy and compute various values through them. One such computation you will be using is `ndarray.numpy.ndarray` is a (usually fixed-size) multidimensional array container of items of the same type and size.

### 2.3 Exercise 133: Creating an array to implement NumPy computations


```python
# np.arange returns evenly spaced values 
# within a given interval.

np.arange(1,101,2)
```




    array([ 1,  3,  5,  7,  9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31, 33,
           35, 37, 39, 41, 43, 45, 47, 49, 51, 53, 55, 57, 59, 61, 63, 65, 67,
           69, 71, 73, 75, 77, 79, 81, 83, 85, 87, 89, 91, 93, 95, 97, 99])




```python
#reshape to 20 rows and 5 cols
np.arange(1, 101).reshape(20,5)
```




    array([[  1,   2,   3,   4,   5],
           [  6,   7,   8,   9,  10],
           [ 11,  12,  13,  14,  15],
           [ 16,  17,  18,  19,  20],
           [ 21,  22,  23,  24,  25],
           [ 26,  27,  28,  29,  30],
           [ 31,  32,  33,  34,  35],
           [ 36,  37,  38,  39,  40],
           [ 41,  42,  43,  44,  45],
           [ 46,  47,  48,  49,  50],
           [ 51,  52,  53,  54,  55],
           [ 56,  57,  58,  59,  60],
           [ 61,  62,  63,  64,  65],
           [ 66,  67,  68,  69,  70],
           [ 71,  72,  73,  74,  75],
           [ 76,  77,  78,  79,  80],
           [ 81,  82,  83,  84,  85],
           [ 86,  87,  88,  89,  90],
           [ 91,  92,  93,  94,  95],
           [ 96,  97,  98,  99, 100]])



**Note** `.reshape` fills the new matrix by rows (not by columns)


```python
#matrix computations
mat1 = np.arange(1, 101).reshape(20,5) 
mat1 - 50
mat1*10
mat1+mat1
mat1*mat1
np.dot(mat1, mat1.T) #dot product of two arrays (for 2-d arrays, is same as matrix multiplication)
np.matmul(mat1,mat1.T) #matrix multiplication
```




    array([[-49, -48, -47, -46, -45],
           [-44, -43, -42, -41, -40],
           [-39, -38, -37, -36, -35],
           [-34, -33, -32, -31, -30],
           [-29, -28, -27, -26, -25],
           [-24, -23, -22, -21, -20],
           [-19, -18, -17, -16, -15],
           [-14, -13, -12, -11, -10],
           [ -9,  -8,  -7,  -6,  -5],
           [ -4,  -3,  -2,  -1,   0],
           [  1,   2,   3,   4,   5],
           [  6,   7,   8,   9,  10],
           [ 11,  12,  13,  14,  15],
           [ 16,  17,  18,  19,  20],
           [ 21,  22,  23,  24,  25],
           [ 26,  27,  28,  29,  30],
           [ 31,  32,  33,  34,  35],
           [ 36,  37,  38,  39,  40],
           [ 41,  42,  43,  44,  45],
           [ 46,  47,  48,  49,  50]])






    array([[  10,   20,   30,   40,   50],
           [  60,   70,   80,   90,  100],
           [ 110,  120,  130,  140,  150],
           [ 160,  170,  180,  190,  200],
           [ 210,  220,  230,  240,  250],
           [ 260,  270,  280,  290,  300],
           [ 310,  320,  330,  340,  350],
           [ 360,  370,  380,  390,  400],
           [ 410,  420,  430,  440,  450],
           [ 460,  470,  480,  490,  500],
           [ 510,  520,  530,  540,  550],
           [ 560,  570,  580,  590,  600],
           [ 610,  620,  630,  640,  650],
           [ 660,  670,  680,  690,  700],
           [ 710,  720,  730,  740,  750],
           [ 760,  770,  780,  790,  800],
           [ 810,  820,  830,  840,  850],
           [ 860,  870,  880,  890,  900],
           [ 910,  920,  930,  940,  950],
           [ 960,  970,  980,  990, 1000]])






    array([[  2,   4,   6,   8,  10],
           [ 12,  14,  16,  18,  20],
           [ 22,  24,  26,  28,  30],
           [ 32,  34,  36,  38,  40],
           [ 42,  44,  46,  48,  50],
           [ 52,  54,  56,  58,  60],
           [ 62,  64,  66,  68,  70],
           [ 72,  74,  76,  78,  80],
           [ 82,  84,  86,  88,  90],
           [ 92,  94,  96,  98, 100],
           [102, 104, 106, 108, 110],
           [112, 114, 116, 118, 120],
           [122, 124, 126, 128, 130],
           [132, 134, 136, 138, 140],
           [142, 144, 146, 148, 150],
           [152, 154, 156, 158, 160],
           [162, 164, 166, 168, 170],
           [172, 174, 176, 178, 180],
           [182, 184, 186, 188, 190],
           [192, 194, 196, 198, 200]])






    array([[    1,     4,     9,    16,    25],
           [   36,    49,    64,    81,   100],
           [  121,   144,   169,   196,   225],
           [  256,   289,   324,   361,   400],
           [  441,   484,   529,   576,   625],
           [  676,   729,   784,   841,   900],
           [  961,  1024,  1089,  1156,  1225],
           [ 1296,  1369,  1444,  1521,  1600],
           [ 1681,  1764,  1849,  1936,  2025],
           [ 2116,  2209,  2304,  2401,  2500],
           [ 2601,  2704,  2809,  2916,  3025],
           [ 3136,  3249,  3364,  3481,  3600],
           [ 3721,  3844,  3969,  4096,  4225],
           [ 4356,  4489,  4624,  4761,  4900],
           [ 5041,  5184,  5329,  5476,  5625],
           [ 5776,  5929,  6084,  6241,  6400],
           [ 6561,  6724,  6889,  7056,  7225],
           [ 7396,  7569,  7744,  7921,  8100],
           [ 8281,  8464,  8649,  8836,  9025],
           [ 9216,  9409,  9604,  9801, 10000]])






    array([[   55,   130,   205,   280,   355,   430,   505,   580,   655,
              730,   805,   880,   955,  1030,  1105,  1180,  1255,  1330,
             1405,  1480],
           [  130,   330,   530,   730,   930,  1130,  1330,  1530,  1730,
             1930,  2130,  2330,  2530,  2730,  2930,  3130,  3330,  3530,
             3730,  3930],
           [  205,   530,   855,  1180,  1505,  1830,  2155,  2480,  2805,
             3130,  3455,  3780,  4105,  4430,  4755,  5080,  5405,  5730,
             6055,  6380],
           [  280,   730,  1180,  1630,  2080,  2530,  2980,  3430,  3880,
             4330,  4780,  5230,  5680,  6130,  6580,  7030,  7480,  7930,
             8380,  8830],
           [  355,   930,  1505,  2080,  2655,  3230,  3805,  4380,  4955,
             5530,  6105,  6680,  7255,  7830,  8405,  8980,  9555, 10130,
            10705, 11280],
           [  430,  1130,  1830,  2530,  3230,  3930,  4630,  5330,  6030,
             6730,  7430,  8130,  8830,  9530, 10230, 10930, 11630, 12330,
            13030, 13730],
           [  505,  1330,  2155,  2980,  3805,  4630,  5455,  6280,  7105,
             7930,  8755,  9580, 10405, 11230, 12055, 12880, 13705, 14530,
            15355, 16180],
           [  580,  1530,  2480,  3430,  4380,  5330,  6280,  7230,  8180,
             9130, 10080, 11030, 11980, 12930, 13880, 14830, 15780, 16730,
            17680, 18630],
           [  655,  1730,  2805,  3880,  4955,  6030,  7105,  8180,  9255,
            10330, 11405, 12480, 13555, 14630, 15705, 16780, 17855, 18930,
            20005, 21080],
           [  730,  1930,  3130,  4330,  5530,  6730,  7930,  9130, 10330,
            11530, 12730, 13930, 15130, 16330, 17530, 18730, 19930, 21130,
            22330, 23530],
           [  805,  2130,  3455,  4780,  6105,  7430,  8755, 10080, 11405,
            12730, 14055, 15380, 16705, 18030, 19355, 20680, 22005, 23330,
            24655, 25980],
           [  880,  2330,  3780,  5230,  6680,  8130,  9580, 11030, 12480,
            13930, 15380, 16830, 18280, 19730, 21180, 22630, 24080, 25530,
            26980, 28430],
           [  955,  2530,  4105,  5680,  7255,  8830, 10405, 11980, 13555,
            15130, 16705, 18280, 19855, 21430, 23005, 24580, 26155, 27730,
            29305, 30880],
           [ 1030,  2730,  4430,  6130,  7830,  9530, 11230, 12930, 14630,
            16330, 18030, 19730, 21430, 23130, 24830, 26530, 28230, 29930,
            31630, 33330],
           [ 1105,  2930,  4755,  6580,  8405, 10230, 12055, 13880, 15705,
            17530, 19355, 21180, 23005, 24830, 26655, 28480, 30305, 32130,
            33955, 35780],
           [ 1180,  3130,  5080,  7030,  8980, 10930, 12880, 14830, 16780,
            18730, 20680, 22630, 24580, 26530, 28480, 30430, 32380, 34330,
            36280, 38230],
           [ 1255,  3330,  5405,  7480,  9555, 11630, 13705, 15780, 17855,
            19930, 22005, 24080, 26155, 28230, 30305, 32380, 34455, 36530,
            38605, 40680],
           [ 1330,  3530,  5730,  7930, 10130, 12330, 14530, 16730, 18930,
            21130, 23330, 25530, 27730, 29930, 32130, 34330, 36530, 38730,
            40930, 43130],
           [ 1405,  3730,  6055,  8380, 10705, 13030, 15355, 17680, 20005,
            22330, 24655, 26980, 29305, 31630, 33955, 36280, 38605, 40930,
            43255, 45580],
           [ 1480,  3930,  6380,  8830, 11280, 13730, 16180, 18630, 21080,
            23530, 25980, 28430, 30880, 33330, 35780, 38230, 40680, 43130,
            45580, 48030]])






    array([[   55,   130,   205,   280,   355,   430,   505,   580,   655,
              730,   805,   880,   955,  1030,  1105,  1180,  1255,  1330,
             1405,  1480],
           [  130,   330,   530,   730,   930,  1130,  1330,  1530,  1730,
             1930,  2130,  2330,  2530,  2730,  2930,  3130,  3330,  3530,
             3730,  3930],
           [  205,   530,   855,  1180,  1505,  1830,  2155,  2480,  2805,
             3130,  3455,  3780,  4105,  4430,  4755,  5080,  5405,  5730,
             6055,  6380],
           [  280,   730,  1180,  1630,  2080,  2530,  2980,  3430,  3880,
             4330,  4780,  5230,  5680,  6130,  6580,  7030,  7480,  7930,
             8380,  8830],
           [  355,   930,  1505,  2080,  2655,  3230,  3805,  4380,  4955,
             5530,  6105,  6680,  7255,  7830,  8405,  8980,  9555, 10130,
            10705, 11280],
           [  430,  1130,  1830,  2530,  3230,  3930,  4630,  5330,  6030,
             6730,  7430,  8130,  8830,  9530, 10230, 10930, 11630, 12330,
            13030, 13730],
           [  505,  1330,  2155,  2980,  3805,  4630,  5455,  6280,  7105,
             7930,  8755,  9580, 10405, 11230, 12055, 12880, 13705, 14530,
            15355, 16180],
           [  580,  1530,  2480,  3430,  4380,  5330,  6280,  7230,  8180,
             9130, 10080, 11030, 11980, 12930, 13880, 14830, 15780, 16730,
            17680, 18630],
           [  655,  1730,  2805,  3880,  4955,  6030,  7105,  8180,  9255,
            10330, 11405, 12480, 13555, 14630, 15705, 16780, 17855, 18930,
            20005, 21080],
           [  730,  1930,  3130,  4330,  5530,  6730,  7930,  9130, 10330,
            11530, 12730, 13930, 15130, 16330, 17530, 18730, 19930, 21130,
            22330, 23530],
           [  805,  2130,  3455,  4780,  6105,  7430,  8755, 10080, 11405,
            12730, 14055, 15380, 16705, 18030, 19355, 20680, 22005, 23330,
            24655, 25980],
           [  880,  2330,  3780,  5230,  6680,  8130,  9580, 11030, 12480,
            13930, 15380, 16830, 18280, 19730, 21180, 22630, 24080, 25530,
            26980, 28430],
           [  955,  2530,  4105,  5680,  7255,  8830, 10405, 11980, 13555,
            15130, 16705, 18280, 19855, 21430, 23005, 24580, 26155, 27730,
            29305, 30880],
           [ 1030,  2730,  4430,  6130,  7830,  9530, 11230, 12930, 14630,
            16330, 18030, 19730, 21430, 23130, 24830, 26530, 28230, 29930,
            31630, 33330],
           [ 1105,  2930,  4755,  6580,  8405, 10230, 12055, 13880, 15705,
            17530, 19355, 21180, 23005, 24830, 26655, 28480, 30305, 32130,
            33955, 35780],
           [ 1180,  3130,  5080,  7030,  8980, 10930, 12880, 14830, 16780,
            18730, 20680, 22630, 24580, 26530, 28480, 30430, 32380, 34330,
            36280, 38230],
           [ 1255,  3330,  5405,  7480,  9555, 11630, 13705, 15780, 17855,
            19930, 22005, 24080, 26155, 28230, 30305, 32380, 34455, 36530,
            38605, 40680],
           [ 1330,  3530,  5730,  7930, 10130, 12330, 14530, 16730, 18930,
            21130, 23330, 25530, 27730, 29930, 32130, 34330, 36530, 38730,
            40930, 43130],
           [ 1405,  3730,  6055,  8380, 10705, 13030, 15355, 17680, 20005,
            22330, 24655, 26980, 29305, 31630, 33955, 36280, 38605, 40930,
            43255, 45580],
           [ 1480,  3930,  6380,  8830, 11280, 13730, 16180, 18630, 21080,
            23530, 25980, 28430, 30880, 33330, 35780, 38230, 40680, 43130,
            45580, 48030]])




```python
mat2 = np.random.rand(5, 5)
np.linalg.inv(mat2)
```




    array([[-18.92672655,   3.76904429,  -2.60074383, -14.02819983,
             15.57083737],
           [ 18.87521128,  -4.16201186,   5.7310077 ,  11.33350607,
            -15.87428648],
           [  6.93319874,  -1.94062715,   1.70393556,   5.62308912,
             -5.6372072 ],
           [ -8.36933118,   2.20093728,  -4.05669911,  -5.9567614 ,
              9.26768947],
           [  1.3800529 ,   1.82907354,  -2.8092127 ,   3.50044945,
             -1.54176019]])




```python
#dimension-specific computations
mat3=np.arange(1, 13).reshape(3,4)
mat3

np.sum(mat3,axis=0) #column means
np.mean(mat3,axis=1) #row means

np.std(mat3,axis=0) #column std
np.std(mat3,axis=1) #row std

#multi-dimensional
mat4=np.arange(1, 13).reshape(2,2,3)
mat4

np.mean(mat4,axis=0) 
np.mean(mat4,axis=1)
np.mean(mat4,axis=2) 
np.mean(mat4,axis=(0,1))
```




    array([[ 1,  2,  3,  4],
           [ 5,  6,  7,  8],
           [ 9, 10, 11, 12]])






    array([15, 18, 21, 24])






    array([ 2.5,  6.5, 10.5])






    array([3.26598632, 3.26598632, 3.26598632, 3.26598632])






    array([1.11803399, 1.11803399, 1.11803399])






    array([[[ 1,  2,  3],
            [ 4,  5,  6]],
    
           [[ 7,  8,  9],
            [10, 11, 12]]])






    array([[4., 5., 6.],
           [7., 8., 9.]])






    array([[ 2.5,  3.5,  4.5],
           [ 8.5,  9.5, 10.5]])






    array([[ 2.,  5.],
           [ 8., 11.]])






    array([5.5, 6.5, 7.5])



## 3. The pandas library

Pandas is the Python library that handles data on all fronts. Pandas can import data, read data, and display data in an object called a `DataFrame`. A DataFrame consists of rows and columns. One way to get a feel for DataFrames is to create one.

### 3.1 Exercise 134: Using DataFrames to Manipulate Stored Student testscore Data

In this exercise, you will create a dictionary, which is one of many ways to create a pandas DataFrame. You will then manipulate this data as required.


```python
# create dictionary of test scores
test_dict = {'Corey':[63,75,88], 
             'Kevin':[48,98,92], 
             'Akshay': [87, 86, 85]}
print(test_dict)

# create dataframe
df = pd.DataFrame(test_dict)
df
```

    {'Corey': [63, 75, 88], 'Kevin': [48, 98, 92], 'Akshay': [87, 86, 85]}
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Corey</th>
      <th>Kevin</th>
      <th>Akshay</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>63</td>
      <td>48</td>
      <td>87</td>
    </tr>
    <tr>
      <th>1</th>
      <td>75</td>
      <td>98</td>
      <td>86</td>
    </tr>
    <tr>
      <th>2</th>
      <td>88</td>
      <td>92</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
</div>



You can inspect the DataFrame: 

 - First, each dictionary key is listed as a column. 
 - Second, the rows are labeled with indices starting with 0 by default. 
 - Third, the visual layout is clear and legible. Each column of a DataFrame is officially represented as a Series. A series is a one-dimensional ndarray. 
 
 Now, you will rotate the DataFrame, which is also known as a transpose, a standard `pandas` method. A transpose turns rows into columns and columns into rows.


```python
df = df.T
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63</td>
      <td>75</td>
      <td>88</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48</td>
      <td>98</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87</td>
      <td>86</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
</div>



### 3.2 Exercise 135: DataFrame Computations with the Student testscore Data


```python
df.columns = ['Quiz_1', 'Quiz_2', 'Quiz_3'] 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63</td>
      <td>75</td>
      <td>88</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48</td>
      <td>98</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87</td>
      <td>86</td>
      <td>85</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.ndim
```




    2



Now, select a range of values from specific rows and columns. You will be using .iloc with the index number, which is a function present in a pandas DataFrame for selection. This is shown in the following step:


```python
#access first row by index number
df.iloc[0]
df.iloc[:,0]
```




    Quiz_1    63
    Quiz_2    75
    Quiz_3    88
    Name: Corey, dtype: int64






    Corey     63
    Kevin     48
    Akshay    87
    Name: Quiz_1, dtype: int64




```python
#access column by name
df['Quiz_1']
df.Quiz_1
```




    Corey     63
    Kevin     48
    Akshay    87
    Name: Quiz_1, dtype: int64






    Corey     63
    Kevin     48
    Akshay    87
    Name: Quiz_1, dtype: int64



### 3.3 Exercise 136: Computing DataFrames within DataFrames


```python
# Defining a new DataFrame from first 2 rows and last 2 columns 
rows = ['Corey', 'Kevin'] 
cols = ['Quiz_2', 'Quiz_3'] 
df_spring = df.loc[rows, cols] 
df_spring
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>75</td>
      <td>88</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>98</td>
      <td>92</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Select first 2 rows and last 2 columns using index numbers 
df.iloc[[0,1], [1,2]] 
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>75</td>
      <td>88</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>98</td>
      <td>92</td>
    </tr>
  </tbody>
</table>
</div>



Now, add a new column to find the quiz average of our students. You can generate new columns in a variety of ways. One way is to use available methods such as the mean. In pandas, it's important to specify the axis. An axis of 0 represents the columns, and an axis of 1 represents the rows.


```python
# Define new column as mean of other columns 
df['Quiz_Avg'] = df.mean(axis=1) 
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_Avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63</td>
      <td>75</td>
      <td>88</td>
      <td>75.333333</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48</td>
      <td>98</td>
      <td>92</td>
      <td>79.333333</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87</td>
      <td>86</td>
      <td>85</td>
      <td>86.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Create a new column as a list
df['Quiz_4'] = [92, 95, 88]
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_Avg</th>
      <th>Quiz_4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63</td>
      <td>75</td>
      <td>88</td>
      <td>75.333333</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48</td>
      <td>98</td>
      <td>92</td>
      <td>79.333333</td>
      <td>95</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87</td>
      <td>86</td>
      <td>85</td>
      <td>86.000000</td>
      <td>88</td>
    </tr>
  </tbody>
</table>
</div>




```python
# Delete column
del df['Quiz_Avg']
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63</td>
      <td>75</td>
      <td>88</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48</td>
      <td>98</td>
      <td>92</td>
      <td>95</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87</td>
      <td>86</td>
      <td>85</td>
      <td>88</td>
    </tr>
  </tbody>
</table>
</div>



In the next section, you will be looking at new rows and NaN, which is an official NumPy term.

### 3.4 New rows and NaN

It's not easy to add new rows to a pandas DataFrame. A common strategy is to generate a new DataFrame and then to concatenate the values. Say you have a new student who joins the class for the fourth quiz. What values should you put for the other three quizzes? The answer is NaN. It stands for Not a Number. NaN is an official NumPy term. It can be accessed using np.NaN. It is case-sensitive. In later exercises, you will look at how NaN can be used. In the next exercise, you will look at concatenating and working with null values.

Much more can be said on this.  **For more details**, see 
 - https://pandas.pydata.org/pandas-docs/dev/user_guide/gotchas.html#nan-integer-na-values-and-na-type-promotions
 - https://pandas.pydata.org/docs/user_guide/missing_data.html
 - https://stackoverflow.com/questions/60115806/pd-na-vs-np-nan-for-pandas

### 3.5 Exercise 137: Concatenating and Finding the Mean with Null Values for Our testscore Data




```python
# Create new DataFrame of one row 
df_new = pd.DataFrame({'Quiz_1':[np.NaN], 'Quiz_2':[np.NaN], 'Quiz_3': [np.NaN], 'Quiz_4':[71]}, index=['Adrian'])

# Concatenate DataFrames 
df = pd.concat([df, df_new])

df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_4</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63.0</td>
      <td>75.0</td>
      <td>88.0</td>
      <td>92</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48.0</td>
      <td>98.0</td>
      <td>92.0</td>
      <td>95</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87.0</td>
      <td>86.0</td>
      <td>85.0</td>
      <td>88</td>
    </tr>
    <tr>
      <th>Adrian</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>71</td>
    </tr>
  </tbody>
</table>
</div>




```python
df['Quiz_Avg'] = df.mean(axis=1, skipna=True)
df
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_4</th>
      <th>Quiz_Avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63.0</td>
      <td>75.0</td>
      <td>88.0</td>
      <td>92</td>
      <td>79.50</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48.0</td>
      <td>98.0</td>
      <td>92.0</td>
      <td>95</td>
      <td>83.25</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87.0</td>
      <td>86.0</td>
      <td>85.0</td>
      <td>88</td>
      <td>86.50</td>
    </tr>
    <tr>
      <th>Adrian</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>71</td>
      <td>71.00</td>
    </tr>
  </tbody>
</table>
</div>



Notice that all values are floats except for **Quiz_4**. There will be occasions when you need to cast all values in a particular column as another type.

### 3.6 Casting column types


```python
df.Quiz_4.astype(float)
df['Quiz_4'] = df.Quiz_4.astype(float)
df
```




    Corey     92.0
    Kevin     95.0
    Akshay    88.0
    Adrian    71.0
    Name: Quiz_4, dtype: float64






<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>Quiz_1</th>
      <th>Quiz_2</th>
      <th>Quiz_3</th>
      <th>Quiz_4</th>
      <th>Quiz_Avg</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>Corey</th>
      <td>63.0</td>
      <td>75.0</td>
      <td>88.0</td>
      <td>92.0</td>
      <td>79.50</td>
    </tr>
    <tr>
      <th>Kevin</th>
      <td>48.0</td>
      <td>98.0</td>
      <td>92.0</td>
      <td>95.0</td>
      <td>83.25</td>
    </tr>
    <tr>
      <th>Akshay</th>
      <td>87.0</td>
      <td>86.0</td>
      <td>85.0</td>
      <td>88.0</td>
      <td>86.50</td>
    </tr>
    <tr>
      <th>Adrian</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>71.0</td>
      <td>71.00</td>
    </tr>
  </tbody>
</table>
</div>



## 4. Data

Now that you have been introduced to NumPy and pandas, you will use them to analyze some real data. Data scientists analyze data that exists in the cloud or online. One strategy is to download data directly to your computer.
 
 
It is recommended to create a new folder to store all of your data. You can open your Jupyter Notebook in this same folder.

### 4.1 Downloading data

Data comes in many formats, and pandas is equipped to handle most of them. In general, when looking for data to analyze, it's worth searching the keyword "dataset." A dataset is a collection of data. Online, "data" is everywhere, whereas datasets contain data in its raw format. You will start by examining the famous Boston Housing dataset from 1980, which is available on our GitHub repository. This dataset can be found here https://packt.live/31Cd96j. You can begin by first downloading the dataset onto our system.

### 4.2 Reading data

Here is a list of standard data files that pandas will read, along with the code for reading data:

- csv files: `pd.read_csv('file_name')`
- excel files: `pd.read_excel('file_name')`
- feather files: `pd.read_feather('file_name')`
- html files: `pd.read_html('file_name')`
- json files: `pd.read_json('file_name')`
- sql database: `pd.read_sql('file_name')`

If the files are clean, pandas will read them properly. Sometimes, files are not clean, and changing function parameters may be required. It's advisable to copy any errors and search for solutions online. A further point of consideration is that the data should be read into a DataFrame. Pandas will convert the data into a DataFrame upon reading it, but you need to save DataFrame as a variable.

### 4.3 Exercise 138: Reading and viewing the Boston Housing dataset


```python
housing_df = pd.read_csv('HousingData.csv')
housing_df.head()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0.00632</td>
      <td>18.0</td>
      <td>2.31</td>
      <td>0.0</td>
      <td>0.538</td>
      <td>6.575</td>
      <td>65.2</td>
      <td>4.0900</td>
      <td>1</td>
      <td>296</td>
      <td>15.3</td>
      <td>396.90</td>
      <td>4.98</td>
      <td>24.0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0.02731</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>6.421</td>
      <td>78.9</td>
      <td>4.9671</td>
      <td>2</td>
      <td>242</td>
      <td>17.8</td>
      <td>396.90</td>
      <td>9.14</td>
      <td>21.6</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0.02729</td>
      <td>0.0</td>
      <td>7.07</td>
      <td>0.0</td>
      <td>0.469</td>
      <td>7.185</td>
      <td>61.1</td>
      <td>4.9671</td>
      <td>2</td>
      <td>242</td>
      <td>17.8</td>
      <td>392.83</td>
      <td>4.03</td>
      <td>34.7</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0.03237</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>6.998</td>
      <td>45.8</td>
      <td>6.0622</td>
      <td>3</td>
      <td>222</td>
      <td>18.7</td>
      <td>394.63</td>
      <td>2.94</td>
      <td>33.4</td>
    </tr>
    <tr>
      <th>4</th>
      <td>0.06905</td>
      <td>0.0</td>
      <td>2.18</td>
      <td>0.0</td>
      <td>0.458</td>
      <td>7.147</td>
      <td>54.2</td>
      <td>6.0622</td>
      <td>3</td>
      <td>222</td>
      <td>18.7</td>
      <td>396.90</td>
      <td>NaN</td>
      <td>36.2</td>
    </tr>
  </tbody>
</table>
</div>



Data description: 

![image.png](image.png)

### 4.4 Exercise 139: Gaining data insights from the Boston Housing dataset


```python
housing_df.describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>486.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>506.000000</td>
      <td>486.000000</td>
      <td>506.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.611874</td>
      <td>11.211934</td>
      <td>11.083992</td>
      <td>0.069959</td>
      <td>0.554695</td>
      <td>6.284634</td>
      <td>68.518519</td>
      <td>3.795043</td>
      <td>9.549407</td>
      <td>408.237154</td>
      <td>18.455534</td>
      <td>356.674032</td>
      <td>12.715432</td>
      <td>22.532806</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.720192</td>
      <td>23.388876</td>
      <td>6.835896</td>
      <td>0.255340</td>
      <td>0.115878</td>
      <td>0.702617</td>
      <td>27.999513</td>
      <td>2.105710</td>
      <td>8.707259</td>
      <td>168.537116</td>
      <td>2.164946</td>
      <td>91.294864</td>
      <td>7.155871</td>
      <td>9.197104</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.006320</td>
      <td>0.000000</td>
      <td>0.460000</td>
      <td>0.000000</td>
      <td>0.385000</td>
      <td>3.561000</td>
      <td>2.900000</td>
      <td>1.129600</td>
      <td>1.000000</td>
      <td>187.000000</td>
      <td>12.600000</td>
      <td>0.320000</td>
      <td>1.730000</td>
      <td>5.000000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.081900</td>
      <td>0.000000</td>
      <td>5.190000</td>
      <td>0.000000</td>
      <td>0.449000</td>
      <td>5.885500</td>
      <td>45.175000</td>
      <td>2.100175</td>
      <td>4.000000</td>
      <td>279.000000</td>
      <td>17.400000</td>
      <td>375.377500</td>
      <td>7.125000</td>
      <td>17.025000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.253715</td>
      <td>0.000000</td>
      <td>9.690000</td>
      <td>0.000000</td>
      <td>0.538000</td>
      <td>6.208500</td>
      <td>76.800000</td>
      <td>3.207450</td>
      <td>5.000000</td>
      <td>330.000000</td>
      <td>19.050000</td>
      <td>391.440000</td>
      <td>11.430000</td>
      <td>21.200000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.560263</td>
      <td>12.500000</td>
      <td>18.100000</td>
      <td>0.000000</td>
      <td>0.624000</td>
      <td>6.623500</td>
      <td>93.975000</td>
      <td>5.188425</td>
      <td>24.000000</td>
      <td>666.000000</td>
      <td>20.200000</td>
      <td>396.225000</td>
      <td>16.955000</td>
      <td>25.000000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>88.976200</td>
      <td>100.000000</td>
      <td>27.740000</td>
      <td>1.000000</td>
      <td>0.871000</td>
      <td>8.780000</td>
      <td>100.000000</td>
      <td>12.126500</td>
      <td>24.000000</td>
      <td>711.000000</td>
      <td>22.000000</td>
      <td>396.900000</td>
      <td>37.970000</td>
      <td>50.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
housing_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 506 entries, 0 to 505
    Data columns (total 14 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   CRIM     486 non-null    float64
     1   ZN       486 non-null    float64
     2   INDUS    486 non-null    float64
     3   CHAS     486 non-null    float64
     4   NOX      506 non-null    float64
     5   RM       506 non-null    float64
     6   AGE      486 non-null    float64
     7   DIS      506 non-null    float64
     8   RAD      506 non-null    int64  
     9   TAX      506 non-null    int64  
     10  PTRATIO  506 non-null    float64
     11  B        506 non-null    float64
     12  LSTAT    486 non-null    float64
     13  MEDV     506 non-null    float64
    dtypes: float64(12), int64(2)
    memory usage: 55.5 KB
    


```python
housing_df.shape
```




    (506, 14)



This confirms that you have 506 rows and 14 columns. Notice that shape does not have any parentheses after it. This is because it's technically an attribute and pre-computed.

### 4.5 Null values

You need to do something about the null values. There are several popular choices when dealing with null values: 

- Eliminate the rows: Can work if null values are a very small percentage, such as 1% of the total dataset. 
- Replace missing values with the mean/median/mode and add a missing indicator (for use in downstream modeling efforts)
- Impute missing values: Depends on the reason for missingness.  Can use other fields to impute missing values in a given field if it is reasonable to assume that missingness can be "explained" by other **observed** values.  This is not always the case.


### 4.6 Exercise 140: Null value operations on a dataset


```python
housing_df.isnull()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>1</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>2</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>3</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>4</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>501</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
    </tr>
    <tr>
      <th>502</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>503</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>504</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
    <tr>
      <th>505</th>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>True</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
      <td>False</td>
    </tr>
  </tbody>
</table>
<p>506 rows × 14 columns</p>
</div>




```python
housing_df.isnull().any()
```




    CRIM        True
    ZN          True
    INDUS       True
    CHAS        True
    NOX        False
    RM         False
    AGE         True
    DIS        False
    RAD        False
    TAX        False
    PTRATIO    False
    B          False
    LSTAT       True
    MEDV       False
    dtype: bool




```python
housing_df.loc[:, housing_df.isnull().any()].describe()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>AGE</th>
      <th>LSTAT</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>count</th>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
      <td>486.000000</td>
    </tr>
    <tr>
      <th>mean</th>
      <td>3.611874</td>
      <td>11.211934</td>
      <td>11.083992</td>
      <td>0.069959</td>
      <td>68.518519</td>
      <td>12.715432</td>
    </tr>
    <tr>
      <th>std</th>
      <td>8.720192</td>
      <td>23.388876</td>
      <td>6.835896</td>
      <td>0.255340</td>
      <td>27.999513</td>
      <td>7.155871</td>
    </tr>
    <tr>
      <th>min</th>
      <td>0.006320</td>
      <td>0.000000</td>
      <td>0.460000</td>
      <td>0.000000</td>
      <td>2.900000</td>
      <td>1.730000</td>
    </tr>
    <tr>
      <th>25%</th>
      <td>0.081900</td>
      <td>0.000000</td>
      <td>5.190000</td>
      <td>0.000000</td>
      <td>45.175000</td>
      <td>7.125000</td>
    </tr>
    <tr>
      <th>50%</th>
      <td>0.253715</td>
      <td>0.000000</td>
      <td>9.690000</td>
      <td>0.000000</td>
      <td>76.800000</td>
      <td>11.430000</td>
    </tr>
    <tr>
      <th>75%</th>
      <td>3.560263</td>
      <td>12.500000</td>
      <td>18.100000</td>
      <td>0.000000</td>
      <td>93.975000</td>
      <td>16.955000</td>
    </tr>
    <tr>
      <th>max</th>
      <td>88.976200</td>
      <td>100.000000</td>
      <td>27.740000</td>
      <td>1.000000</td>
      <td>100.000000</td>
      <td>37.970000</td>
    </tr>
  </tbody>
</table>
</div>



Breakdown of the above code:
- `housing_df` is the DataFrame. 
- `.loc` allows you to specify rows and columns. 
- `:` selects all rows. 
- `housing_df.isnull().any()` selects only columns with null values.
- `.describe()` pulls up the statistics.

### 4.7 Replacing null values

Pandas include a nice method, `fillna`, which can be used to replace null values. It works for individual columns and entire DataFrames. You will use three approaches, 

- replacing the null values of a column with the mean
- replacing the null values of a column with another value
- replacing all the null values in the entire dataset with the median. 


```python
# replacing with the mean
housing_df['AGE'] = housing_df['AGE'].fillna(housing_df.mean())

# replacing with another value
housing_df['CHAS'] = housing_df['CHAS'].fillna(0)

# replacing with median
housing_df = housing_df.fillna(housing_df.median())

housing_df.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 506 entries, 0 to 505
    Data columns (total 14 columns):
     #   Column   Non-Null Count  Dtype  
    ---  ------   --------------  -----  
     0   CRIM     506 non-null    float64
     1   ZN       506 non-null    float64
     2   INDUS    506 non-null    float64
     3   CHAS     506 non-null    float64
     4   NOX      506 non-null    float64
     5   RM       506 non-null    float64
     6   AGE      506 non-null    float64
     7   DIS      506 non-null    float64
     8   RAD      506 non-null    int64  
     9   TAX      506 non-null    int64  
     10  PTRATIO  506 non-null    float64
     11  B        506 non-null    float64
     12  LSTAT    506 non-null    float64
     13  MEDV     506 non-null    float64
    dtypes: float64(12), int64(2)
    memory usage: 55.5 KB
    

After eliminating all null values, the dataset is much cleaner. There may also be unrealistic outliers or extreme outliers that will lead to poor prediction. These can often be detected through visual analysis, which you will be covering in the next section.

## 5. Visualization (but faster this time)


```python
# Set up seaborn dark grid
sns.set()
```


```python
plt.hist(housing_df['MEDV'])
plt.show()
```




    (array([ 21.,  55.,  82., 154.,  84.,  41.,  30.,   8.,  10.,  21.]),
     array([ 5. ,  9.5, 14. , 18.5, 23. , 27.5, 32. , 36.5, 41. , 45.5, 50. ]),
     <BarContainer object of 10 artists>)




    
![png](output_86_1.png)
    



```python
plt.hist(housing_df['MEDV'])
plt.title('Median Boston Housing Prices')
plt.xlabel('1980 Median Value in Thousands')
plt.ylabel('Count')
plt.show()
```




    (array([ 21.,  55.,  82., 154.,  84.,  41.,  30.,   8.,  10.,  21.]),
     array([ 5. ,  9.5, 14. , 18.5, 23. , 27.5, 32. , 36.5, 41. , 45.5, 50. ]),
     <BarContainer object of 10 artists>)






    Text(0.5, 1.0, 'Median Boston Housing Prices')






    Text(0.5, 0, '1980 Median Value in Thousands')






    Text(0, 0.5, 'Count')




    
![png](output_87_4.png)
    



```python
title = 'Median Boston Housing Prices'
plt.figure(figsize=(10,6))
plt.hist(housing_df['MEDV'])
plt.title(title, fontsize=15)
plt.xlabel('1980 Median Value in Thousands')
plt.ylabel('Count')
plt.savefig(title, dpi=300)
plt.show()
```




    <Figure size 1000x600 with 0 Axes>






    (array([ 21.,  55.,  82., 154.,  84.,  41.,  30.,   8.,  10.,  21.]),
     array([ 5. ,  9.5, 14. , 18.5, 23. , 27.5, 32. , 36.5, 41. , 45.5, 50. ]),
     <BarContainer object of 10 artists>)






    Text(0.5, 1.0, 'Median Boston Housing Prices')






    Text(0.5, 0, '1980 Median Value in Thousands')






    Text(0, 0.5, 'Count')




    
![png](output_88_5.png)
    



```python
x = housing_df['RM']
y = housing_df['MEDV']
plt.scatter(x, y)
plt.show()
```




    <matplotlib.collections.PathCollection at 0x7f8c9ba2f748>




    
![png](output_89_1.png)
    



```python
housing_df.corr()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>CRIM</th>
      <th>ZN</th>
      <th>INDUS</th>
      <th>CHAS</th>
      <th>NOX</th>
      <th>RM</th>
      <th>AGE</th>
      <th>DIS</th>
      <th>RAD</th>
      <th>TAX</th>
      <th>PTRATIO</th>
      <th>B</th>
      <th>LSTAT</th>
      <th>MEDV</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>CRIM</th>
      <td>1.000000</td>
      <td>-0.185359</td>
      <td>0.392632</td>
      <td>-0.055585</td>
      <td>0.410971</td>
      <td>-0.220045</td>
      <td>0.343427</td>
      <td>-0.366025</td>
      <td>0.601224</td>
      <td>0.560469</td>
      <td>0.277964</td>
      <td>-0.365336</td>
      <td>0.437417</td>
      <td>-0.383895</td>
    </tr>
    <tr>
      <th>ZN</th>
      <td>-0.185359</td>
      <td>1.000000</td>
      <td>-0.507304</td>
      <td>-0.032992</td>
      <td>-0.498619</td>
      <td>0.312295</td>
      <td>-0.535341</td>
      <td>0.632428</td>
      <td>-0.300061</td>
      <td>-0.304385</td>
      <td>-0.394622</td>
      <td>0.170125</td>
      <td>-0.398838</td>
      <td>0.362292</td>
    </tr>
    <tr>
      <th>INDUS</th>
      <td>0.392632</td>
      <td>-0.507304</td>
      <td>1.000000</td>
      <td>0.054693</td>
      <td>0.738387</td>
      <td>-0.377978</td>
      <td>0.614248</td>
      <td>-0.698621</td>
      <td>0.592735</td>
      <td>0.716267</td>
      <td>0.385366</td>
      <td>-0.354840</td>
      <td>0.564508</td>
      <td>-0.476394</td>
    </tr>
    <tr>
      <th>CHAS</th>
      <td>-0.055585</td>
      <td>-0.032992</td>
      <td>0.054693</td>
      <td>1.000000</td>
      <td>0.070867</td>
      <td>0.106797</td>
      <td>0.074984</td>
      <td>-0.092318</td>
      <td>-0.003339</td>
      <td>-0.035822</td>
      <td>-0.109451</td>
      <td>0.050608</td>
      <td>-0.047279</td>
      <td>0.183844</td>
    </tr>
    <tr>
      <th>NOX</th>
      <td>0.410971</td>
      <td>-0.498619</td>
      <td>0.738387</td>
      <td>0.070867</td>
      <td>1.000000</td>
      <td>-0.302188</td>
      <td>0.711864</td>
      <td>-0.769230</td>
      <td>0.611441</td>
      <td>0.668023</td>
      <td>0.188933</td>
      <td>-0.380051</td>
      <td>0.573040</td>
      <td>-0.427321</td>
    </tr>
    <tr>
      <th>RM</th>
      <td>-0.220045</td>
      <td>0.312295</td>
      <td>-0.377978</td>
      <td>0.106797</td>
      <td>-0.302188</td>
      <td>1.000000</td>
      <td>-0.239518</td>
      <td>0.205246</td>
      <td>-0.209847</td>
      <td>-0.292048</td>
      <td>-0.355501</td>
      <td>0.128069</td>
      <td>-0.604323</td>
      <td>0.695360</td>
    </tr>
    <tr>
      <th>AGE</th>
      <td>0.343427</td>
      <td>-0.535341</td>
      <td>0.614248</td>
      <td>0.074984</td>
      <td>0.711864</td>
      <td>-0.239518</td>
      <td>1.000000</td>
      <td>-0.724354</td>
      <td>0.447088</td>
      <td>0.498408</td>
      <td>0.261826</td>
      <td>-0.268029</td>
      <td>0.575022</td>
      <td>-0.377572</td>
    </tr>
    <tr>
      <th>DIS</th>
      <td>-0.366025</td>
      <td>0.632428</td>
      <td>-0.698621</td>
      <td>-0.092318</td>
      <td>-0.769230</td>
      <td>0.205246</td>
      <td>-0.724354</td>
      <td>1.000000</td>
      <td>-0.494588</td>
      <td>-0.534432</td>
      <td>-0.232471</td>
      <td>0.291512</td>
      <td>-0.483244</td>
      <td>0.249929</td>
    </tr>
    <tr>
      <th>RAD</th>
      <td>0.601224</td>
      <td>-0.300061</td>
      <td>0.592735</td>
      <td>-0.003339</td>
      <td>0.611441</td>
      <td>-0.209847</td>
      <td>0.447088</td>
      <td>-0.494588</td>
      <td>1.000000</td>
      <td>0.910228</td>
      <td>0.464741</td>
      <td>-0.444413</td>
      <td>0.467765</td>
      <td>-0.381626</td>
    </tr>
    <tr>
      <th>TAX</th>
      <td>0.560469</td>
      <td>-0.304385</td>
      <td>0.716267</td>
      <td>-0.035822</td>
      <td>0.668023</td>
      <td>-0.292048</td>
      <td>0.498408</td>
      <td>-0.534432</td>
      <td>0.910228</td>
      <td>1.000000</td>
      <td>0.460853</td>
      <td>-0.441808</td>
      <td>0.524156</td>
      <td>-0.468536</td>
    </tr>
    <tr>
      <th>PTRATIO</th>
      <td>0.277964</td>
      <td>-0.394622</td>
      <td>0.385366</td>
      <td>-0.109451</td>
      <td>0.188933</td>
      <td>-0.355501</td>
      <td>0.261826</td>
      <td>-0.232471</td>
      <td>0.464741</td>
      <td>0.460853</td>
      <td>1.000000</td>
      <td>-0.177383</td>
      <td>0.370727</td>
      <td>-0.507787</td>
    </tr>
    <tr>
      <th>B</th>
      <td>-0.365336</td>
      <td>0.170125</td>
      <td>-0.354840</td>
      <td>0.050608</td>
      <td>-0.380051</td>
      <td>0.128069</td>
      <td>-0.268029</td>
      <td>0.291512</td>
      <td>-0.444413</td>
      <td>-0.441808</td>
      <td>-0.177383</td>
      <td>1.000000</td>
      <td>-0.370993</td>
      <td>0.333461</td>
    </tr>
    <tr>
      <th>LSTAT</th>
      <td>0.437417</td>
      <td>-0.398838</td>
      <td>0.564508</td>
      <td>-0.047279</td>
      <td>0.573040</td>
      <td>-0.604323</td>
      <td>0.575022</td>
      <td>-0.483244</td>
      <td>0.467765</td>
      <td>0.524156</td>
      <td>0.370727</td>
      <td>-0.370993</td>
      <td>1.000000</td>
      <td>-0.723093</td>
    </tr>
    <tr>
      <th>MEDV</th>
      <td>-0.383895</td>
      <td>0.362292</td>
      <td>-0.476394</td>
      <td>0.183844</td>
      <td>-0.427321</td>
      <td>0.695360</td>
      <td>-0.377572</td>
      <td>0.249929</td>
      <td>-0.381626</td>
      <td>-0.468536</td>
      <td>-0.507787</td>
      <td>0.333461</td>
      <td>-0.723093</td>
      <td>1.000000</td>
    </tr>
  </tbody>
</table>
</div>




```python
corr = housing_df.corr()
plt.figure(figsize=(8,6))
sns.heatmap(corr, xticklabels=corr.columns.values,
yticklabels=corr.columns.values, cmap="Blues", linewidths=1.25, alpha=0.8)
plt.show()
```




    <Figure size 800x600 with 0 Axes>






    <AxesSubplot:>




    
![png](output_91_2.png)
    



```python
x = housing_df['RM']
y = housing_df['MEDV']
plt.boxplot(x)
plt.show()
```




    {'whiskers': [<matplotlib.lines.Line2D at 0x7f8c9ba63240>,
      <matplotlib.lines.Line2D at 0x7f8c701ed780>],
     'caps': [<matplotlib.lines.Line2D at 0x7f8c701eda58>,
      <matplotlib.lines.Line2D at 0x7f8c701edd30>],
     'boxes': [<matplotlib.lines.Line2D at 0x7f8c701ed208>],
     'medians': [<matplotlib.lines.Line2D at 0x7f8c7028ac88>],
     'fliers': [<matplotlib.lines.Line2D at 0x7f8c701edcf8>],
     'means': []}




    
![png](output_92_1.png)
    



```python
plt.violinplot(x) 
plt.show() 
```




    {'bodies': [<matplotlib.collections.PolyCollection at 0x7f8c701bae80>],
     'cmaxes': <matplotlib.collections.LineCollection at 0x7f8c701bae48>,
     'cmins': <matplotlib.collections.LineCollection at 0x7f8c701bad68>,
     'cbars': <matplotlib.collections.LineCollection at 0x7f8c70251208>}




    
![png](output_93_1.png)
    



```python
plt.figure(figsize=(10, 7)) 
sns.regplot(x='RM',y='MEDV',data=housing_df) 
plt.show() 
```




    <Figure size 1000x700 with 0 Axes>






    <AxesSubplot:xlabel='RM', ylabel='MEDV'>




    
![png](output_94_2.png)
    



```python
x = housing_df['RM']
y = housing_df['MEDV']
X = sm.add_constant(x) 
model = sm.OLS(y, X) 
est = model.fit() 
print(est.summary())
```

                                OLS Regression Results                            
    ==============================================================================
    Dep. Variable:                   MEDV   R-squared:                       0.484
    Model:                            OLS   Adj. R-squared:                  0.483
    Method:                 Least Squares   F-statistic:                     471.8
    Date:                Tue, 08 Nov 2022   Prob (F-statistic):           2.49e-74
    Time:                        19:31:54   Log-Likelihood:                -1673.1
    No. Observations:                 506   AIC:                             3350.
    Df Residuals:                     504   BIC:                             3359.
    Df Model:                           1                                         
    Covariance Type:            nonrobust                                         
    ==============================================================================
                     coef    std err          t      P>|t|      [0.025      0.975]
    ------------------------------------------------------------------------------
    const        -34.6706      2.650    -13.084      0.000     -39.877     -29.465
    RM             9.1021      0.419     21.722      0.000       8.279       9.925
    ==============================================================================
    Omnibus:                      102.585   Durbin-Watson:                   0.684
    Prob(Omnibus):                  0.000   Jarque-Bera (JB):              612.449
    Skew:                           0.726   Prob(JB):                    1.02e-133
    Kurtosis:                       8.190   Cond. No.                         58.4
    ==============================================================================
    
    Notes:
    [1] Standard Errors assume that the covariance matrix of the errors is correctly specified.
    

    /usr/local/lib/python3.7/site-packages/statsmodels/tsa/tsatools.py:142: FutureWarning: In a future version of pandas all arguments of concat except for the argument 'objs' will be keyword-only
      x = pd.concat(x[::order], 1)
    

You began our introduction to data analysis with **NumPy**, Python's incredibly fast library for handling massive matrix computations. Next, you learned about the fundamentals of **pandas**, Python's library for handling DataFrames. Taken together, you used NumPy and pandas to analyze the Boston Housing dataset, which included descriptive statistical methods and Matplotlib and Seaborn's graphical libraries. You also learned about advanced methods for creating clean, clearly labeled, publishable graphs.

# Example: Broadcasting is not always faster

From https://numpy.org/doc/stable/user/basics.broadcasting.html

"Broadcasting is a powerful tool for writing short and usually intuitive code that does its computations very efficiently in C. However, there are cases when broadcasting uses unnecessarily large amounts of memory for a particular algorithm. In these cases, it is better to write the algorithm’s outer loop in Python. This may also produce more readable code, as algorithms that use broadcasting tend to become more difficult to interpret as the number of dimensions in the broadcast increases."

See the following example and explanation from https://stackoverflow.com/questions/49632993/why-python-broadcasting-in-the-example-below-is-slower-than-a-simple-loop.
 


```python
#function to take squared some of rows after row-wise subtraction

def norm_loop(M, v):
  n = M.shape[0]
  d = np.zeros(n)
  for i in range(n):
    d[i] = np.sum((M[i] - v)**2)
  return d

def norm_bcast(M, v):
     return np.sum((M - v)**2, axis=1)

#broadcasting is better in this instance for smaller datasets
M = np.random.random_sample((10, 100))
v = M[0]
%timeit norm_loop(M, v) 
%timeit norm_bcast(M, v)

#bigger datasets tell a different story
M = np.random.random_sample((1000, 10000))
v = M[0]
%timeit norm_loop(M, v) 
%timeit norm_bcast(M, v)
```

    66.3 µs ± 8.18 µs per loop (mean ± std. dev. of 7 runs, 1000 loops each)
    9.66 µs ± 179 ns per loop (mean ± std. dev. of 7 runs, 100000 loops each)
    20 ms ± 795 µs per loop (mean ± std. dev. of 7 runs, 10 loops each)
    38.9 ms ± 1.67 ms per loop (mean ± std. dev. of 7 runs, 10 loops each)
    

What gives? It comes down to **memory access**. 

In the broadcast version, every element of `M` is subtracted from `v`. By the time the last row of `M` is processed, the results of processing the first row have been evicted from cache, so for the second step, these differences are again loaded into cache memory and squared. Finally, they are loaded and processed a third time for the summation. Since `M` is quite large, parts of the cache are cleared on each step to acomodate all of the data.

In the looped version, each row is processed completely in one smaller step, leading to fewer cache misses (i.e. inability to retreive needed data from cache because it has been cleared and needs to be reloaded) and overall faster code.


```python
M = np.random.random_sample((1000, 10000))
v = M[0]
```


```python

```
