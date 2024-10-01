**© 2022 Scott A. Bruce. Do not distribute.**


```python
from IPython.core.interactiveshell import InteractiveShell
InteractiveShell.ast_node_interactivity = "all"
```

# 1. Introduction

This notebook follows Chapter 2 of the Python Workshop textbook.  There are four types of basic data structures in Python: **list, tuple, dictionary** and **set**.  Each data structure has a set of operations that can be performed on data contained in the structure.  In the following sections, we will introduce these structures and how to manipulate them within Python.

# 2. Lists

Lists are used to store multiple data sets at the same time and may contain different data types within the same list.


```python
shopping = ['bread','milk','eggs']
print(shopping)
```


```python
for item in shopping:
    print(item)
```

Lists are similar to arrays encountered in other programming languages, but differ in that they can contain mixed data types.


```python
mixed = [365, 'days', True]
print(mixed)
```

## 2.1 Nested lists

Can we have a 'list of lists'? Yes.  For example, matrices can be stored as nested lists.


```python
m = [[1,2,3],[4,5,6]]
print(m)

#each element in the list of m is a list containing the row elements for that index
for row in m:
    print(row)

#can access elements using [row][column] notation
print(m[1][2])
#remember that indices start at 0
```

To iterate over elements of a matrix using for loops, consider the following examples.


```python
for i in range(len(m)):
    for j in range(len(m[i])):
        print(m[i][j])
```


```python
for row in m:
    for col in row:
        print(col)
```

Note that the first example explicitly states the row and column indices included in the iterations.  This is helpful when you do not want to access every element.  However, the second example spans all rows and columns and does not require us to know the number of rows or columns.

## 2.2 Activity 6

In your notebook, open a new cell below this one and work through Activity 6 to familiarize yourself with nested lists.

## 2.3 Matrix operations

Here is a rundown of basic matrix operations.


```python
#Addition and subtraction
X = [[1,2,3],[4,5,6],[7,8,9]]
Y = [[10,11,12],[13,14,15],[16,17,18]]

#initialize a result placeholder for each
addn = [[0,0,0],[0,0,0],[0,0,0]]
subtractn = [[0,0,0],[0,0,0],[0,0,0]]

for i in range(len(X)):
    for j in range(len(Y[0])):
        addn[i][j] = X[i][j] + Y[i][j]
        subtractn[i][j] = X[i][j] - Y[i][j]
        
print(addn)
print(subtractn)
```


```python
#Matrix multiplication
X = [[1,2,3],[4,5,6],[7,8,9]]
Y = [[10,11,12],[13,14,15],[16,17,18]]

#initialize a result placeholder for each
multn = [[0,0,0],[0,0,0],[0,0,0]]

for i in range(len(X)): #row of X
    for j in range(len(Y[0])): #column of Y
        for k in range(len(Y)): #row of Y
            multn[i][j] += X[i][k] * Y[k][j]
            
for row in multn:
    print(row)


```

Later on in the course, we will use NumPy for performing matrix calculations, since these are optimized for larger data structures.  More on this to come.

## 2.4 Basic list operations


```python
print(shopping)
len(shopping) #length

#concatenation
[1,2,3]+[4,5,6]

#repeating elements
print(['Whoop!']*3)
```

## 2.5 Accessing items in a list


```python
print(shopping)
print(shopping[1])
```


```python
shopping[1] = 'banana'
print(shopping)
```


```python
print(shopping[-1])
```

**Note**: In Python, a positive index counts forward, and a negative index counts backward.  Use negative indices to access elements from the back


```python
print(shopping[0:2])
print(shopping[:3])
print(shopping[1:])
```

## 2.6 Adding and inserting items into a list


```python
shopping = []
shopping += ['bread','milk','eggs']
print(shopping)
shopping.append('apple')
print(shopping)
shopping.insert(2,'ham')
print(shopping)
```

# 3 Dictionaries

A Python dictionary is an unordered collection.  They are written with curly brackets and consist of pairs of **keys** and **values**.


```python
email = {
    'subject': 'Your car\'s warranty is about to expire!',
    'from': 'dubious_email_address@gmail.com',
    'to': 'your_email_address@yahoo.com',
    'cc': '',
    'bcc': '',
    'date_received':'2021-09-30',
    'body':"""We\'ve been trying to reach you about your car\'s 
              warranty.  Please call us at 555-555-5555 
              to maintain coverage!"""
}
print(email)
```

**Note**: You might have noticed a resemblance between Python dictionaries and JSON.  Although you can load JSON directly into Python, a Python dictionary is a complete data structure with its own set of algorithms and operations.  JSON is just a string written in a similar format.

Dictionaries are like lists and share the following properties:

- Both can be used to store values
- Both can be chaned in place and can grow and shrink on demand
- Both can be nested (i.e. lists of lists, dictionaries of dictionaries, lists of dictionaries, dictionaries of lists)

The main difference is how elements are **accessed**. List elements are accessed by their position index, while dictionary elements are accessed via keys. 

Therefore, dictionaries are more suitable for representing collections of labeled items. However, there are some rules to remember:

- Keys must be unique (no duplicate keys)
- Keys must be immutable (can be a string, number, or a tuple)


```python
#print value for a given key
print(email['to'])

#update a value in place
email['cc'] = 'other_email_address1@hotmail.com'

print(email)
```

Dictionaries are flexible in terms of size


```python
#create dictionary one key-value pair at a time
email = {}
email['subject'] = 'Your car\'s warranty is about to expire!'
email['from'] = 'dubious_email_address@gmail.com'
email['to'] = 'your_email_address@yahoo.com'
email['cc'] = ''
email['bcc'] = ''
email['date_received'] = '2021-09-30'
email['body'] = 'We\'ve been trying to reach you about your car\'s warranty.  Please call us at 555-555-5555 to maintain coverage!'
print(email)
```


```python
#list in a dictionary
email['cc'] = ['email1@gmail.com','email2@hotmail.com','email3@aol.com']
print(email['cc'])

#dictionary in a dictionary
email['metadata'] = {
    'sender_ip': '132:123:1231:12',
    'attachment': True,
    'fraud_score': 10
}
print(email['metadata'])
```

By combining lists and dictionaries creatively, we can store complex real-world information and model structures directly and easily.  This is one of the min benefits of scripting languages such as Python.

For example, a table in a database can be stored as a list with nested dictionaries for each record (see Activity 7).

## 3.1 Zipping and unzipping dictionaries using zip()

Sometimes you obtain information from multiple lists that need to be aggregated for analysis.  In this case you can use the `zip()` method to aggregate lists and create a zip object, which can then be unzipped into a list, tuple, or dictionary.  This can be useful, especially when converting a list to a dictionary.


```python
items = ['apple','orange','banana']
quantity = [5,3,2]
```


```python
orders = zip(items,quantity)
print(orders)
```


```python
orders = zip(items,quantity)
print(list(orders))
```


```python
orders = zip(items,quantity)
print(tuple(orders))
```


```python
orders = zip(items,quantity)
print(dict(orders))
```

## 3.2 Dictionary methods


```python
orders = {'apple':5, 'orange':3, 'banana':2}
print(orders.values()) #access values
print(list(orders.values())) #store values in list (can be for later use)
print(list(orders.keys())) #store keys in list (can be for later use)
```

**Note**: You can't directly iterate a dictionary.  You can first convert it to a list of tuples using the `items()` method, then iterate the resulting list and access it.


```python
orders = {'apple':5, 'orange':3, 'banana':2}
for tuple in list(orders.items()):
    print(tuple)
```

# 4. Tuples

A tuple object is similar to a list, but **it cannot be changed after initialization**.  Use typles to represent fixed collections of items.


```python
weekdays_list = ['Monday', 'Tuesday',
                 'Wednesday','Thursday',
                 'Friday','Saturday','Sunday']
```

However, this does not guarantee that values will remain unchanged throughout its lifetime.  Lists are **mutable**.


```python
weekdays_list = ['Monday', 'Tuesday',
                 'Wednesday','Thursday',
                 'Friday','Saturday','Sunday']
weekdays_list.remove('Monday')
print(weekdays_list)
```

Tuples on the other hand are **immutable**.  


```python
weekdays_tup = ('Monday', 'Tuesday',
                 'Wednesday','Thursday',
                 'Friday','Saturday','Sunday')
weekdays_tup.remove('Monday')
```


```python
weekdays_tup[0]
weekdays_tup[0] = 'Saturday'
```


```python
print(weekdays_tup)
```

You can't append to a tuple, but you can create a new tuple by concatenating an existing tuple with new items.


```python
weekdays_tup.append('Funday')
```


```python
print(weekdays_tup + ('Funday',))
```

Tuples also support mixed data types and nesting.


```python
mixed = ('apple',True,3)
shopping = ('apple',3),('orange',2),('banana',5)
print(mixed)
print(shopping)
```

# 5. Sets

Sets are relatively new to Python. Sets are unordered collections of **unique** and **immutable** objects that support operations mimicking mathmematical set theory.  Sets can also be used to prevent duplicate values.


```python
#initialize by passing in a list
s1 = set([1,2,3,4,5,6])
print(s1)
s2 = set([1,2,2,3,4,4,5,6,6])
print(s2)
s3 = set([3,4,5,6,6,6,1,1,2])
print(s3)
```


```python
#initialize with curly brackets directly
s4 = {'apple','orange','banana'}
print(s4)
```

While the objects in a set are immutable, sets are mutable.


```python
s4.add('pineapple')
print(s4)
```

## 5.1 Set operations


```python
s5,s6 = {1,2,3,4},{3,4,5,6}

#unions
print(s5|s6)
print(s5.union(s6))

#intersections
print(s5 & s6)
print(s6.intersection(s5))
```


```python
s5,s6 = {1,2,3,4},{3,4,5,6}

#set minus
print(s5 - s6)
print(s5.difference(s6))

#check if subset
print(s5 <= s6)
print(s5.issubset(s6))

s7 = {1,2,3}
print(s7 <= s5)
print(s7 <= s7)

#check if proper subset
print(s7 < s5)
print(s7 < s7)

#check if superset
print(s5 >= s7)
print(s5.issuperset(s7))
print(s5 > s7) # proper superset 
print(s5 > s5)
```

    {1, 2}
    {1, 2}
    False
    False
    True
    True
    True
    False
    True
    True
    True
    False
    

**Note**: How are sets different from lists and dictionaries?
 - Sets are different from lists in that they are unordered.  
 - Sets are different from dictionaries in that they do not map keys to values.  
 - Sets are neither a sequence or a mapping type; they are a type by themselves.

# 6. Choosing the right structure

Here are some general guidelines for choosing the right structure (based on their unique characteristics):
 
 - Lists are used to store multiple objects and retain a sequence
 - Dictionaries store unique key-value pair mappings
 - Tuples are immutable
 - Sets only store unique elements
 
Choosing an inappropriate data structure could lead to low efficiency when running code, security issues, and data loss.  Choose wisely!

