# 100 pandas puzzles

Inspired by [100 Numpy exerises](https://github.com/rougier/numpy-100), here are 100* short puzzles for testing your knowledge of [pandas'](http://pandas.pydata.org/) power.

Since pandas is a large library with many different specialist features and functions, these excercises focus mainly on the fundamentals of manipulating data (indexing, grouping, aggregating, cleaning), making use of the core DataFrame and Series objects. 

Many of the excerises here are stright-forward in that the solutions require no more than a few lines of code (in pandas or NumPy... don't go using pure Python or Cython!). Choosing the right methods and following best practices is the underlying goal.

The exercises are loosely divided in sections. Each section has a difficulty rating; these ratings are subjective, of course, but should be a seen as a rough guide as to how inventive the required solution is.

If you're just starting out with pandas and you are looking for some other resources, the official documentation  is very extensive. In particular, some good places get a broader overview of pandas are...

- [10 minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)
- [pandas basics](http://pandas.pydata.org/pandas-docs/stable/basics.html)
- [tutorials](http://pandas.pydata.org/pandas-docs/stable/tutorials.html)
- [cookbook and idioms](http://pandas.pydata.org/pandas-docs/stable/cookbook.html#cookbook)

Enjoy the puzzles!

\* *the list of exercises is not yet complete! Pull requests or suggestions for additional exercises, corrections and improvements are welcomed.*

## Importing pandas

### Getting started and checking your pandas setup

Difficulty: *easy* 

**1.** Import pandas under the alias `pd`.


```python
import pandas as pd
```

**2.** Print the version of pandas that has been imported.


```python
pd.__version__
```

**3.** Print out all the version information of the libraries that are required by the pandas library.


```python
pd.show_versions()
```

## DataFrame basics

### A few of the fundamental routines for selecting, sorting, adding and aggregating data in DataFrames

Difficulty: *easy*

Note: remember to import numpy using:
```python
import numpy as np
```

Consider the following Python dictionary `data` and Python list `labels`:

``` python
data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']
```
(This is just some meaningless data I made up with the theme of animals and trips to a vet.)

**4.** Create a DataFrame `df` from this dictionary `data` which has the index `labels`.


```python
import numpy as np

data = {'animal': ['cat', 'cat', 'snake', 'dog', 'dog', 'cat', 'snake', 'cat', 'dog', 'dog'],
        'age': [2.5, 3, 0.5, np.nan, 5, 2, 4.5, np.nan, 7, 3],
        'visits': [1, 3, 2, 3, 2, 3, 1, 1, 2, 1],
        'priority': ['yes', 'yes', 'no', 'yes', 'no', 'no', 'no', 'yes', 'no', 'no']}

labels = ['a', 'b', 'c', 'd', 'e', 'f', 'g', 'h', 'i', 'j']

df = pd.DataFrame(data, index=labels)
```

**5.** Display a summary of the basic information about this DataFrame and its data (*hint: there is a single method that can be called on the DataFrame*).


```python
df.info()

# ...or...

df.describe()
```

**6.** Return the first 3 rows of the DataFrame `df`.


```python
df.iloc[:3]

# or equivalently

df.head(3)
```

**7.** Select just the 'animal' and 'age' columns from the DataFrame `df`.


```python
df.loc[:, ['animal', 'age']]

# or

df[['animal', 'age']]
```

**8.** Select the data in rows `[3, 4, 8]` *and* in columns `['animal', 'age']`.


```python
df.loc[df.index[[3, 4, 8]], ['animal', 'age']]
```

**9.** Select only the rows where the number of visits is greater than 3.


```python
df[df['visits'] > 3]
```

**10.** Select the rows where the age is missing, i.e. it is `NaN`.


```python
df[df['age'].isnull()]
```

**11.** Select the rows where the animal is a cat *and* the age is less than 3.


```python
df[(df['animal'] == 'cat') & (df['age'] < 3)]
```

**12.** Select the rows the age is between 2 and 4 (inclusive).


```python
df[df['age'].between(2, 4)]
```

**13.** Change the age in row 'f' to 1.5.


```python
df.loc['f', 'age'] = 1.5
```

**14.** Calculate the sum of all visits in `df` (i.e. the total number of visits).


```python
df['visits'].sum()
```

**15.** Calculate the mean age for each different animal in `df`.


```python
df.groupby('animal')['age'].mean()
```

**16.** Append a new row 'k' to `df` with your choice of values for each column. Then delete that row to return the original DataFrame.


```python
df.loc['k'] = [5.5, 'dog', 'no', 2]

# and then deleting the new row...

df = df.drop('k')
```

**17.** Count the number of each type of animal in `df`.


```python
df['animal'].value_counts()
```

**18.** Sort `df` first by the values in the 'age' in *decending* order, then by the value in the 'visits' column in *ascending* order (so row `i` should be first, and row `d` should be last).


```python
df.sort_values(by=['age', 'visits'], ascending=[False, True])
```

**19.** The 'priority' column contains the values 'yes' and 'no'. Replace this column with a column of boolean values: 'yes' should be `True` and 'no' should be `False`.


```python
df['priority'] = df['priority'].map({'yes': True, 'no': False})
```

**20.** In the 'animal' column, change the 'snake' entries to 'python'.


```python
df['animal'] = df['animal'].replace('snake', 'python')
```

**21.** For each animal type and each number of visits, find the mean age. In other words, each row is an animal, each column is a number of visits and the values are the mean ages (*hint: use a pivot table*).


```python
df.pivot_table(index='animal', columns='visits', values='age', aggfunc='mean')
```

## DataFrames: beyond the basics

### Slightly trickier: you may need to combine two or more methods to get the right answer

Difficulty: *medium*

The previous section was tour through some basic but essential DataFrame operations. Below are some ways that you might need to cut your data, but for which there is no single "out of the box" method.

**22.** You have a DataFrame `df` with a column 'A' of integers. For example:
```python
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 5, 5, 5, 6, 7, 7]})
```

How do you filter out rows which contain the same integer as the row immediately above?

You should be left with a column containing the following values:

```python
1, 2, 3, 4, 5, 6, 7
```


```python
df = pd.DataFrame({'A': [1, 2, 2, 3, 4, 5, 5, 5, 6, 7, 7]})

df.loc[df['A'].shift() != df['A']]

# Alternatively, we could use drop_duplicates() here. Note
# that this removes *all* duplicates though, so it won't
# work as desired if A is [1, 1, 2, 2, 1, 1] for example.

df.drop_duplicates(subset='A')
```

**23.** Given a DataFrame of random numeric values:
```python
df = pd.DataFrame(np.random.random(size=(5, 3))) # this is a 5x3 DataFrame of float values
```

how do you subtract the row mean from each element in the row?


```python
df = pd.DataFrame(np.random.random(size=(5, 3)))

df.sub(df.mean(axis=1), axis=0)
```

**24.** Suppose you have DataFrame with 10 columns of real numbers, for example:

```python
df = pd.DataFrame(np.random.random(size=(5, 10)), columns=list('abcdefghij'))
```
Which column of numbers has the smallest sum? Return that column's label.


```python
df = pd.DataFrame(np.random.random(size=(5, 10)), columns=list('abcdefghij'))

df.sum().idxmin()
```

**25.** How do you count how many unique rows a DataFrame has (i.e. ignore all rows that are duplicates)?


```python
df = pd.DataFrame(np.random.randint(0, 2, size=(10, 3)))

len(df) - df.duplicated(keep=False).sum()

# or perhaps more simply...

len(df.drop_duplicates(keep=False))
```

       0  1  2
    0  1  0  0
    1  1  0  0
    2  1  1  0
    3  1  1  1
    4  0  1  0
    5  0  1  1
    6  1  1  1
    7  0  0  1
    8  1  1  1
    9  0  1  1
    




    3



The next three puzzles are slightly harder.

**26.** In the cell below, you have a DataFrame `df` that consists of 10 columns of floating-point numbers. Exactly 5 entries in each row are NaN values. 

For each row of the DataFrame, find the *column* which contains the *third* NaN value.

You should return a Series of column labels: `e, c, d, h, d`


```python
nan = np.nan

data = [[0.04,  nan,  nan, 0.25,  nan, 0.43, 0.71, 0.51,  nan,  nan],
        [ nan,  nan,  nan, 0.04, 0.76,  nan,  nan, 0.67, 0.76, 0.16],
        [ nan,  nan, 0.5 ,  nan, 0.31, 0.4 ,  nan,  nan, 0.24, 0.01],
        [0.49,  nan,  nan, 0.62, 0.73, 0.26, 0.85,  nan,  nan,  nan],
        [ nan,  nan, 0.41,  nan, 0.05,  nan, 0.61,  nan, 0.48, 0.68]]

columns = list('abcdefghij')

df = pd.DataFrame(data, columns=columns)


(df.isnull().cumsum(axis=1) == 3).idxmax(axis=1)
```

**27.** A DataFrame has a column of groups 'grps' and and column of integer values 'vals': 

```python
df = pd.DataFrame({'grps': list('aaabbcaabcccbbc'), 
                   'vals': [12,345,3,1,45,14,4,52,54,23,235,21,57,3,87]})
```
For each *group*, find the sum of the three greatest values.  You should end up with the answer as follows:
```
grps
a    409
b    156
c    345
```


```python
df = pd.DataFrame({'grps': list('aaabbcaabcccbbc'), 
                   'vals': [12,345,3,1,45,14,4,52,54,23,235,21,57,3,87]})

df.groupby('grps')['vals'].nlargest(3).sum(level=0)
```

**28.** The DataFrame `df` constructed below has two integer columns 'A' and 'B'. The values in 'A' are between 1 and 100 (inclusive). 

For each group of 10 consecutive integers in 'A' (i.e. `(0, 10]`, `(10, 20]`, ...), calculate the sum of the corresponding values in column 'B'.

The answer should be a Series as follows:

```
A
(0, 10]      635
(10, 20]     360
(20, 30]     315
(30, 40]     306
(40, 50]     750
(50, 60]     284
(60, 70]     424
(70, 80]     526
(80, 90]     835
(90, 100]    852
```


```python
df = pd.DataFrame(np.random.RandomState(8765).randint(1, 101, size=(100, 2)), columns = ["A", "B"])

df.groupby(pd.cut(df['A'], np.arange(0, 101, 10)))['B'].sum()
```

## DataFrames: harder problems 

### These might require a bit of thinking outside the box...

...but all are solvable using just the usual pandas/NumPy methods (and so avoid using explicit `for` loops).

Difficulty: *hard*

**29.** Consider a DataFrame `df` where there is an integer column 'X':
```python
df = pd.DataFrame({'X': [7, 2, 0, 3, 4, 2, 5, 0, 3, 4]})
```
For each value, count the difference back to the previous zero (or the start of the Series, whichever is closer). These values should therefore be 

```
[1, 2, 0, 1, 2, 3, 4, 0, 1, 2]
```

Make this a new column 'Y'.


```python
df = pd.DataFrame({'X': [7, 2, 0, 3, 4, 2, 5, 0, 3, 4]})

izero = np.r_[-1, (df == 0).values.nonzero()[0]]  # indices of zeros
idx = np.arange(len(df))
y = df['X'] != 0
df['Y'] = idx - izero[np.searchsorted(izero - 1, idx) - 1]

# http://stackoverflow.com/questions/30730981/how-to-count-distance-to-the-previous-zero-in-pandas-series/
# credit: Behzad Nouri
```

    [-1  2  7]
    [-2  1  6]
    [0 1 2 3 4 5 6 7 8 9]
    [1 1 2 2 2 2 2 3 3 3]
    [-1 -1  2  2  2  2  2  7  7  7]
    

Here's an alternative approach based on a [cookbook recipe](http://pandas.pydata.org/pandas-docs/stable/cookbook.html#grouping):


```python
df = pd.DataFrame({'X': [7, 2, 0, 3, 4, 2, 5, 0, 3, 4]})

x = (df['X'] != 0).cumsum()
y = x != x.shift()
df['Y'] = y.groupby((y != y.shift()).cumsum()).cumsum()
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
      <th>X</th>
      <th>Y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>7</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>5</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>6</th>
      <td>5</td>
      <td>4</td>
    </tr>
    <tr>
      <th>7</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>8</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>4</td>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>



And another approach using a groupby operation:


```python
df = pd.DataFrame({'X': [7, 2, 0, 3, 4, 2, 5, 0, 3, 4]})

df['Y'] = df.groupby((df['X'] == 0).cumsum()).cumcount()

# We're off by one before we reach the first zero.
first_zero_idx = (df['X'] == 0).idxmax()
df['Y'].iloc[0:first_zero_idx] += 1
```

**30.** Consider the DataFrame constructed below which contains rows and columns of numerical data. 

Create a list of the column-row index locations of the 3 largest values in this DataFrame. In this case, the answer should be:
```
[(5, 7), (6, 4), (2, 5)]
```


```python
df = pd.DataFrame(np.random.RandomState(30).randint(1, 101, size=(8, 8)))

df.unstack().sort_values()[-3:].index.tolist()

# http://stackoverflow.com/questions/14941261/index-and-column-for-the-max-value-in-pandas-dataframe/
# credit: DSM
```

**31.** You are given the DataFrame below with a column of group IDs, 'grps', and a column of corresponding integer values, 'vals'.

```python
df = pd.DataFrame({"vals": np.random.RandomState(31).randint(-30, 30, size=15), 
                   "grps": np.random.RandomState(31).choice(["A", "B"], 15)})
```

Create a new column 'patched_values' which contains the same values as the 'vals' any negative values in 'vals' with the group mean:

```
    vals grps  patched_vals
0    -12    A          13.6
1     -7    B          28.0
2    -14    A          13.6
3      4    A           4.0
4     -7    A          13.6
5     28    B          28.0
6     -2    A          13.6
7     -1    A          13.6
8      8    A           8.0
9     -2    B          28.0
10    28    A          28.0
11    12    A          12.0
12    16    A          16.0
13   -24    A          13.6
14   -12    A          13.6
```


```python
df = pd.DataFrame({"vals": np.random.RandomState(31).randint(-30, 30, size=15), 
                   "grps": np.random.RandomState(31).choice(["A", "B"], 15)})

def replace(group):
    mask = group<0
    group[mask] = group[~mask].mean()
    return group

df.groupby(['grps'])['vals'].transform(replace)

# http://stackoverflow.com/questions/14760757/replacing-values-with-groupby-means/
# credit: unutbu
```

**32.** Implement a rolling mean over groups with window size 3, which ignores NaN value. For example consider the following DataFrame:

```python
>>> df = pd.DataFrame({'group': list('aabbabbbabab'),
                       'value': [1, 2, 3, np.nan, 2, 3, np.nan, 1, 7, 3, np.nan, 8]})
>>> df
   group  value
0      a    1.0
1      a    2.0
2      b    3.0
3      b    NaN
4      a    2.0
5      b    3.0
6      b    NaN
7      b    1.0
8      a    7.0
9      b    3.0
10     a    NaN
11     b    8.0
```
The goal is to compute the Series:

```
0     1.000000
1     1.500000
2     3.000000
3     3.000000
4     1.666667
5     3.000000
6     3.000000
7     2.000000
8     3.666667
9     2.000000
10    4.500000
11    4.000000
```
E.g. the first window of size three for group 'b' has values 3.0, NaN and 3.0 and occurs at row index 5. Instead of being NaN the value in the new column at this row index should be 3.0 (just the two non-NaN values are used to compute the mean (3+3)/2)


```python
df = pd.DataFrame({'group': list('aabbabbbabab'),
                   'value': [1, 2, 3, np.nan, 2, 3, np.nan, 1, 7, 3, np.nan, 8]})

g1 = df.groupby(['group'])['value']              # group values  
g2 = df.fillna(0).groupby(['group'])['value']    # fillna, then group values

s = g2.rolling(3, min_periods=1).sum() / g1.rolling(3, min_periods=1).count() # compute means

s.reset_index(level=0, drop=True).sort_index()  # drop/sort index

# http://stackoverflow.com/questions/36988123/pandas-groupby-and-rolling-apply-ignoring-nans/
```

## Series and DatetimeIndex

### Exercises for creating and manipulating Series with datetime data

Difficulty: *easy/medium*

pandas is fantastic for working with dates and times. These puzzles explore some of this functionality.


**33.** Create a DatetimeIndex that contains each business day of 2015 and use it to index a Series of random numbers. Let's call this Series `s`.


```python
dti = pd.date_range(start='2015-01-01', end='2015-12-31', freq='B') 
s = pd.Series(np.random.rand(len(dti)), index=dti)
s
```

**34.** Find the sum of the values in `s` for every Wednesday.


```python
s[s.index.weekday == 2].sum() 
```

**35.** For each calendar month in `s`, find the mean of values.


```python
s.resample('M').mean()
```

**36.** For each group of four consecutive calendar months in `s`, find the date on which the highest value occurred.


```python
s.groupby(pd.Grouper(freq='4M')).idxmax()
```

**37.** Create a DateTimeIndex consisting of the third Thursday in each month for the years 2015 and 2016.


```python
pd.date_range('2015-01-01', '2016-12-31', freq='WOM-3THU')
```

## Cleaning Data

### Making a DataFrame easier to work with

Difficulty: *easy/medium*

It happens all the time: someone gives you data containing malformed strings, Python, lists and missing data. How do you tidy it up so you can get on with the analysis?

Take this monstrosity as the DataFrame to use in the following puzzles:

```python
df = pd.DataFrame({'From_To': ['LoNDon_paris', 'MAdrid_miLAN', 'londON_StockhOlm', 
                               'Budapest_PaRis', 'Brussels_londOn'],
              'FlightNumber': [10045, np.nan, 10065, np.nan, 10085],
              'RecentDelays': [[23, 47], [], [24, 43, 87], [13], [67, 32]],
                   'Airline': ['KLM(!)', '<Air France> (12)', '(British Airways. )', 
                               '12. Air France', '"Swiss Air"']})
```

Formatted, it looks like this:

```
            From_To  FlightNumber  RecentDelays              Airline
0      LoNDon_paris       10045.0      [23, 47]               KLM(!)
1      MAdrid_miLAN           NaN            []    <Air France> (12)
2  londON_StockhOlm       10065.0  [24, 43, 87]  (British Airways. )
3    Budapest_PaRis           NaN          [13]       12. Air France
4   Brussels_londOn       10085.0      [67, 32]          "Swiss Air"
```

(It's some flight data I made up; it's not meant to be accurate in any way.)


**38.** Some values in the the **FlightNumber** column are missing (they are `NaN`). These numbers are meant to increase by 10 with each row so 10055 and 10075 need to be put in place. Modify `df` to fill in these missing numbers and make the column an integer column (instead of a float column).


```python
df = pd.DataFrame({'From_To': ['LoNDon_paris', 'MAdrid_miLAN', 'londON_StockhOlm', 
                               'Budapest_PaRis', 'Brussels_londOn'],
              'FlightNumber': [10045, np.nan, 10065, np.nan, 10085],
              'RecentDelays': [[23, 47], [], [24, 43, 87], [13], [67, 32]],
                   'Airline': ['KLM(!)', '<Air France> (12)', '(British Airways. )', 
                               '12. Air France', '"Swiss Air"']})

df['FlightNumber'] = df['FlightNumber'].interpolate().astype(int)
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
      <th>From_To</th>
      <th>FlightNumber</th>
      <th>RecentDelays</th>
      <th>Airline</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>LoNDon_paris</td>
      <td>10045</td>
      <td>[23, 47]</td>
      <td>KLM(!)</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MAdrid_miLAN</td>
      <td>10055</td>
      <td>[]</td>
      <td>&lt;Air France&gt; (12)</td>
    </tr>
    <tr>
      <th>2</th>
      <td>londON_StockhOlm</td>
      <td>10065</td>
      <td>[24, 43, 87]</td>
      <td>(British Airways. )</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Budapest_PaRis</td>
      <td>10075</td>
      <td>[13]</td>
      <td>12. Air France</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Brussels_londOn</td>
      <td>10085</td>
      <td>[67, 32]</td>
      <td>"Swiss Air"</td>
    </tr>
  </tbody>
</table>
</div>



**39.** The **From\_To** column would be better as two separate columns! Split each string on the underscore delimiter `_` to give a new temporary DataFrame called 'temp' with the correct values. Assign the correct column names 'From' and 'To' to this temporary DataFrame. 


```python
temp = df.From_To.str.split('_', expand=True)
temp.columns = ['From', 'To']
temp
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
      <th>From</th>
      <th>To</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>LoNDon</td>
      <td>paris</td>
    </tr>
    <tr>
      <th>1</th>
      <td>MAdrid</td>
      <td>miLAN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>londON</td>
      <td>StockhOlm</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Budapest</td>
      <td>PaRis</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Brussels</td>
      <td>londOn</td>
    </tr>
  </tbody>
</table>
</div>



**40.** Notice how the capitalisation of the city names is all mixed up in this temporary DataFrame 'temp'. Standardise the strings so that only the first letter is uppercase (e.g. "londON" should become "London".)


```python
temp['From'] = temp['From'].str.capitalize()
temp['To'] = temp['To'].str.capitalize()
temp
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
      <th>From</th>
      <th>To</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>London</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>1</th>
      <td>Madrid</td>
      <td>Milan</td>
    </tr>
    <tr>
      <th>2</th>
      <td>London</td>
      <td>Stockholm</td>
    </tr>
    <tr>
      <th>3</th>
      <td>Budapest</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>4</th>
      <td>Brussels</td>
      <td>London</td>
    </tr>
  </tbody>
</table>
</div>



**41.** Delete the **From_To** column from `df` and attach the temporary DataFrame 'temp' from the previous questions.


```python
df = df.drop('From_To', axis=1)
df = df.join(temp)
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
      <th>FlightNumber</th>
      <th>RecentDelays</th>
      <th>Airline</th>
      <th>From</th>
      <th>To</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10045</td>
      <td>[23, 47]</td>
      <td>KLM(!)</td>
      <td>London</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10055</td>
      <td>[]</td>
      <td>&lt;Air France&gt; (12)</td>
      <td>Madrid</td>
      <td>Milan</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10065</td>
      <td>[24, 43, 87]</td>
      <td>(British Airways. )</td>
      <td>London</td>
      <td>Stockholm</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10075</td>
      <td>[13]</td>
      <td>12. Air France</td>
      <td>Budapest</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10085</td>
      <td>[67, 32]</td>
      <td>"Swiss Air"</td>
      <td>Brussels</td>
      <td>London</td>
    </tr>
  </tbody>
</table>
</div>



**42**. In the **Airline** column, you can see some extra punctuation and symbols have appeared around the airline names. Pull out just the airline name. E.g. `'(British Airways. )'` should become `'British Airways'`.


```python
df['Airline'] = df['Airline'].str.extract('([a-zA-Z\s]+)', expand=False).str.strip()
# note: using .strip() gets rid of any leading/trailing spaces
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
      <th>FlightNumber</th>
      <th>RecentDelays</th>
      <th>Airline</th>
      <th>From</th>
      <th>To</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10045</td>
      <td>[23, 47]</td>
      <td>KLM</td>
      <td>London</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10055</td>
      <td>[]</td>
      <td>Air France</td>
      <td>Madrid</td>
      <td>Milan</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10065</td>
      <td>[24, 43, 87]</td>
      <td>British Airways</td>
      <td>London</td>
      <td>Stockholm</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10075</td>
      <td>[13]</td>
      <td>Air France</td>
      <td>Budapest</td>
      <td>Paris</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10085</td>
      <td>[67, 32]</td>
      <td>Swiss Air</td>
      <td>Brussels</td>
      <td>London</td>
    </tr>
  </tbody>
</table>
</div>



**43**. In the **RecentDelays** column, the values have been entered into the DataFrame as a list. We would like each first value in its own column, each second value in its own column, and so on. If there isn't an Nth value, the value should be NaN.

Expand the Series of lists into a new DataFrame named 'delays', rename the columns 'delay_1', 'delay_2', etc. and replace the unwanted RecentDelays column in `df` with 'delays'.


```python
# there are several ways to do this, but the following approach is possibly the simplest
def series(x):
    return pd.Series(x, dtype='float64')

delays = df['RecentDelays'].apply(series)

delays.columns = ['delay_{}'.format(n) for n in range(1, len(delays.columns)+1)]

df = df.drop('RecentDelays', axis=1).join(delays)

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
      <th>FlightNumber</th>
      <th>Airline</th>
      <th>From</th>
      <th>To</th>
      <th>delay_1</th>
      <th>delay_2</th>
      <th>delay_3</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>10045</td>
      <td>KLM</td>
      <td>London</td>
      <td>Paris</td>
      <td>23.0</td>
      <td>47.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>10055</td>
      <td>Air France</td>
      <td>Madrid</td>
      <td>Milan</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>2</th>
      <td>10065</td>
      <td>British Airways</td>
      <td>London</td>
      <td>Stockholm</td>
      <td>24.0</td>
      <td>43.0</td>
      <td>87.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>10075</td>
      <td>Air France</td>
      <td>Budapest</td>
      <td>Paris</td>
      <td>13.0</td>
      <td>NaN</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>4</th>
      <td>10085</td>
      <td>Swiss Air</td>
      <td>Brussels</td>
      <td>London</td>
      <td>67.0</td>
      <td>32.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



The DataFrame should look much better now:

```
   FlightNumber          Airline      From         To  delay_1  delay_2  delay_3
0         10045              KLM    London      Paris     23.0     47.0      NaN
1         10055       Air France    Madrid      Milan      NaN      NaN      NaN
2         10065  British Airways    London  Stockholm     24.0     43.0     87.0
3         10075       Air France  Budapest      Paris     13.0      NaN      NaN
4         10085        Swiss Air  Brussels     London     67.0     32.0      NaN
```

## Using MultiIndexes

### Go beyond flat DataFrames with additional index levels

Difficulty: *medium*

Previous exercises have seen us analysing data from DataFrames equipped with a single index level. However, pandas also gives you the possibilty of indexing your data using *multiple* levels. This is very much like adding new dimensions to a Series or a DataFrame. For example, a Series is 1D, but by using a MultiIndex with 2 levels we gain of much the same functionality as a 2D DataFrame.

The set of puzzles below explores how you might use multiple index levels to enhance data analysis.

To warm up, we'll look make a Series with two index levels. 

**44**. Given the lists `letters = ['A', 'B', 'C']` and `numbers = list(range(10))`, construct a MultiIndex object from the product of the two lists. Use it to index a Series of random numbers. Call this Series `s`.


```python
letters = ['A', 'B', 'C']
numbers = list(range(10))

mi = pd.MultiIndex.from_product([letters, numbers])
s = pd.Series(np.random.rand(30), index=mi)
s
```




    A  0    0.024515
       1    0.537941
       2    0.695320
       3    0.452670
       4    0.942782
       5    0.949681
       6    0.066035
       7    0.028101
       8    0.284061
       9    0.313399
    B  0    0.334977
       1    0.845310
       2    0.757886
       3    0.725123
       4    0.187561
       5    0.586844
       6    0.701173
       7    0.471642
       8    0.858012
       9    0.638724
    C  0    0.897332
       1    0.668662
       2    0.179371
       3    0.396274
       4    0.348394
       5    0.695388
       6    0.981722
       7    0.843412
       8    0.498246
       9    0.960123
    dtype: float64



**45.** Check the index of `s` is lexicographically sorted (this is a necessary proprty for indexing to work correctly with a MultiIndex).


```python
s.index.is_monotonic_increasing
```




    True



**46**. Select the labels `1`, `3` and `6` from the second level of the MultiIndexed Series.


```python
s.loc[:, [1, 3, 6]]
```




    A  1    0.537941
       3    0.452670
       6    0.066035
    B  1    0.845310
       3    0.725123
       6    0.701173
    C  1    0.668662
       3    0.396274
       6    0.981722
    dtype: float64



**47**. Slice the Series `s`; slice up to label 'B' for the first level and from label 5 onwards for the second level.


```python
s.loc[pd.IndexSlice[:'B', 5:]]

# or equivalently without IndexSlice...
s.loc[slice(None, 'B'), slice(5, None)]

s.loc[:'B', 5:]
```




    A  5    0.949681
       6    0.066035
       7    0.028101
       8    0.284061
       9    0.313399
    B  5    0.586844
       6    0.701173
       7    0.471642
       8    0.858012
       9    0.638724
    dtype: float64



**48**. Sum the values in `s` for each label in the first level (you should have Series giving you a total for labels A, B and C).


```python
s.groupby(level=0).sum()

# s.sum(level=0) will be removed in a future version
```




    A    4.294503
    B    6.107252
    C    6.468923
    dtype: float64



**49**. Suppose that `sum()` (and other methods) did not accept a `level` keyword argument. How else could you perform the equivalent of `s.sum(level=1)`?


```python
# One way is to use .unstack()... 
# This method should convince you that s is essentially just a regular DataFrame in disguise!
s.unstack().sum(axis=0)
```




    0    1.256824
    1    2.051912
    2    1.632577
    3    1.574067
    4    1.478736
    5    2.231913
    6    1.748931
    7    1.343154
    8    1.640318
    9    1.912246
    dtype: float64



**50**. Exchange the levels of the MultiIndex so we have an index of the form (letters, numbers). Is this new Series properly lexsorted? If not, sort it.


```python
new_s = s.swaplevel(0, 1)
print(new_s)

if not new_s.index.is_monotonic_increasing:
    print('not_sorted')
    new_s = new_s.sort_index()

new_s
```

    0  A    0.024515
    1  A    0.537941
    2  A    0.695320
    3  A    0.452670
    4  A    0.942782
    5  A    0.949681
    6  A    0.066035
    7  A    0.028101
    8  A    0.284061
    9  A    0.313399
    0  B    0.334977
    1  B    0.845310
    2  B    0.757886
    3  B    0.725123
    4  B    0.187561
    5  B    0.586844
    6  B    0.701173
    7  B    0.471642
    8  B    0.858012
    9  B    0.638724
    0  C    0.897332
    1  C    0.668662
    2  C    0.179371
    3  C    0.396274
    4  C    0.348394
    5  C    0.695388
    6  C    0.981722
    7  C    0.843412
    8  C    0.498246
    9  C    0.960123
    dtype: float64
    not_sorted
    




    0  A    0.024515
       B    0.334977
       C    0.897332
    1  A    0.537941
       B    0.845310
       C    0.668662
    2  A    0.695320
       B    0.757886
       C    0.179371
    3  A    0.452670
       B    0.725123
       C    0.396274
    4  A    0.942782
       B    0.187561
       C    0.348394
    5  A    0.949681
       B    0.586844
       C    0.695388
    6  A    0.066035
       B    0.701173
       C    0.981722
    7  A    0.028101
       B    0.471642
       C    0.843412
    8  A    0.284061
       B    0.858012
       C    0.498246
    9  A    0.313399
       B    0.638724
       C    0.960123
    dtype: float64



## Minesweeper

### Generate the numbers for safe squares in a Minesweeper grid

Difficulty: *medium* to *hard*

If you've ever used an older version of Windows, there's a good chance you've played with Minesweeper:
- https://en.wikipedia.org/wiki/Minesweeper_(video_game)


If you're not familiar with the game, imagine a grid of squares: some of these squares conceal a mine. If you click on a mine, you lose instantly. If you click on a safe square, you reveal a number telling you how many mines are found in the squares that are immediately adjacent. The aim of the game is to uncover all squares in the grid that do not contain a mine.

In this section, we'll make a DataFrame that contains the necessary data for a game of Minesweeper: coordinates of the squares, whether the square contains a mine and the number of mines found on adjacent squares.

**51**. Let's suppose we're playing Minesweeper on a 5 by 4 grid, i.e.
```
X = 5
Y = 4
```
To begin, generate a DataFrame `df` with two columns, `'x'` and `'y'` containing every coordinate for this grid. That is, the DataFrame should start:
```
   x  y
0  0  0
1  0  1
2  0  2
...
```


```python
X = 5
Y = 4

p = pd.core.reshape.util.cartesian_product([np.arange(X), np.arange(Y)])
df = pd.DataFrame(np.asarray(p).T, columns=['x', 'y'])
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
      <th>x</th>
      <th>y</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>2</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>3</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2</td>
      <td>0</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2</td>
      <td>2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2</td>
      <td>3</td>
    </tr>
    <tr>
      <th>12</th>
      <td>3</td>
      <td>0</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3</td>
      <td>1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3</td>
      <td>2</td>
    </tr>
    <tr>
      <th>15</th>
      <td>3</td>
      <td>3</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>4</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>4</td>
      <td>2</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4</td>
      <td>3</td>
    </tr>
  </tbody>
</table>
</div>



**52**. For this DataFrame `df`, create a new column of zeros (safe) and ones (mine). The probability of a mine occuring at each location should be 0.4.


```python
# One way is to draw samples from a binomial distribution.

df['mine'] = np.random.binomial(1, 0.4, X*Y)
```

**53**. Now create a new column for this DataFrame called `'adjacent'`. This column should contain the number of mines found on adjacent squares in the grid. 

(E.g. for the first row, which is the entry for the coordinate `(0, 0)`, count how many mines are found on the coordinates `(0, 1)`, `(1, 0)` and `(1, 1)`.)


```python
# Here is one way to solve using merges.
# It's not necessary the optimal way, just 
# the solution I thought of first...

df['adjacent'] = \
    df.merge(df + [ 1,  1, 0], on=['x', 'y'], how='left')\
      .merge(df + [ 1, -1, 0], on=['x', 'y'], how='left')\
      .merge(df + [-1,  1, 0], on=['x', 'y'], how='left')\
      .merge(df + [-1, -1, 0], on=['x', 'y'], how='left')\
      .merge(df + [ 1,  0, 0], on=['x', 'y'], how='left')\
      .merge(df + [-1,  0, 0], on=['x', 'y'], how='left')\
      .merge(df + [ 0,  1, 0], on=['x', 'y'], how='left')\
      .merge(df + [ 0, -1, 0], on=['x', 'y'], how='left')\
       .iloc[:, 3:]\
        .sum(axis=1)

# <<https://github.com/Artur-Arstamyan>>'s method
from numpy.lib.stride_tricks import sliding_window_view
arr = df['mine'].values.reshape(5, 4)
df['adjacent'] = (sliding_window_view(np.pad(arr, 1), window_shape=(3, 3)).sum(axis=(-2, -1))-arr).ravel()
print(df)

# An alternative solution is to pivot the DataFrame 
# to form the "actual" grid of mines and use convolution.
# See https://github.com/jakevdp/matplotlib_pydata2013/blob/master/examples/minesweeper.py

from scipy.signal import convolve2d

mine_grid = df.pivot_table(columns='x', index='y', values='mine')
counts = convolve2d(mine_grid.astype(complex), np.ones((3, 3)), mode='same').real.astype(int)
df['adjacent'] = (counts - mine_grid).to_numpy().ravel('F')
df
```

        x  y  mine  adjacent
    0   0  0     0         1
    1   0  1     0         1
    2   0  2     0         3
    3   0  3     1         1
    4   1  0     0         1
    5   1  1     1         0
    6   1  2     0         3
    7   1  3     1         1
    8   2  0     0         1
    9   2  1     0         1
    10  2  2     0         2
    11  2  3     0         1
    12  3  0     0         1
    13  3  1     0         1
    14  3  2     0         1
    15  3  3     0         1
    16  4  0     1         0
    17  4  1     0         1
    18  4  2     0         1
    19  4  3     1         0
    

    C:\Users\ARTUR\AppData\Local\Temp\ipykernel_7136\4115826604.py:6: FutureWarning: Passing 'suffixes' which cause duplicate columns {'mine_x'} in the result is deprecated and will raise a MergeError in a future version.
      df.merge(df + [ 1,  1, 0], on=['x', 'y'], how='left')\
    C:\Users\ARTUR\AppData\Local\Temp\ipykernel_7136\4115826604.py:6: FutureWarning: Passing 'suffixes' which cause duplicate columns {'mine_x'} in the result is deprecated and will raise a MergeError in a future version.
      df.merge(df + [ 1,  1, 0], on=['x', 'y'], how='left')\
    C:\Users\ARTUR\AppData\Local\Temp\ipykernel_7136\4115826604.py:6: FutureWarning: Passing 'suffixes' which cause duplicate columns {'mine_x'} in the result is deprecated and will raise a MergeError in a future version.
      df.merge(df + [ 1,  1, 0], on=['x', 'y'], how='left')\
    




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
      <th>x</th>
      <th>y</th>
      <th>mine</th>
      <th>adjacent</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>0</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>1</th>
      <td>0</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>2</th>
      <td>0</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>3</th>
      <td>0</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>4</th>
      <td>1</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>5</th>
      <td>1</td>
      <td>1</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>6</th>
      <td>1</td>
      <td>2</td>
      <td>0</td>
      <td>3</td>
    </tr>
    <tr>
      <th>7</th>
      <td>1</td>
      <td>3</td>
      <td>1</td>
      <td>1</td>
    </tr>
    <tr>
      <th>8</th>
      <td>2</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>9</th>
      <td>2</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>10</th>
      <td>2</td>
      <td>2</td>
      <td>0</td>
      <td>2</td>
    </tr>
    <tr>
      <th>11</th>
      <td>2</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>12</th>
      <td>3</td>
      <td>0</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>13</th>
      <td>3</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>14</th>
      <td>3</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>15</th>
      <td>3</td>
      <td>3</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>16</th>
      <td>4</td>
      <td>0</td>
      <td>1</td>
      <td>0</td>
    </tr>
    <tr>
      <th>17</th>
      <td>4</td>
      <td>1</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>18</th>
      <td>4</td>
      <td>2</td>
      <td>0</td>
      <td>1</td>
    </tr>
    <tr>
      <th>19</th>
      <td>4</td>
      <td>3</td>
      <td>1</td>
      <td>0</td>
    </tr>
  </tbody>
</table>
</div>



**54**. For rows of the DataFrame that contain a mine, set the value in the `'adjacent'` column to NaN.


```python
df.loc[df['mine'] == 1, 'adjacent'] = np.nan
```

**55**. Finally, convert the DataFrame to grid of the adjacent mine counts: columns are the `x` coordinate, rows are the `y` coordinate.


```python
df.drop('mine', axis=1).set_index(['y', 'x']).unstack()
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead tr th {
        text-align: left;
    }

    .dataframe thead tr:last-of-type th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr>
      <th></th>
      <th colspan="5" halign="left">adjacent</th>
    </tr>
    <tr>
      <th>x</th>
      <th>0</th>
      <th>1</th>
      <th>2</th>
      <th>3</th>
      <th>4</th>
    </tr>
    <tr>
      <th>y</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
    <tr>
      <th>1</th>
      <td>1.0</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>2</th>
      <td>3.0</td>
      <td>3.0</td>
      <td>2.0</td>
      <td>1.0</td>
      <td>1.0</td>
    </tr>
    <tr>
      <th>3</th>
      <td>NaN</td>
      <td>NaN</td>
      <td>1.0</td>
      <td>1.0</td>
      <td>NaN</td>
    </tr>
  </tbody>
</table>
</div>



## Plotting

### Visualize trends and patterns in data

Difficulty: *medium*

To really get a good understanding of the data contained in your DataFrame, it is often essential to create plots: if you're lucky, trends and anomalies will jump right out at you. This functionality is baked into pandas and the puzzles below explore some of what's possible with the library.

**56.** Pandas is highly integrated with the plotting library matplotlib, and makes plotting DataFrames very user-friendly! Plotting in a notebook environment usually makes use of the following boilerplate:

```python
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')
```

matplotlib is the plotting library which pandas' plotting functionality is built upon, and it is usually aliased to ```plt```.

```%matplotlib inline``` tells the notebook to show plots inline, instead of creating them in a separate window.  

```plt.style.use('ggplot')``` is a style theme that most people find agreeable, based upon the styling of R's ggplot package.

For starters, make a scatter plot of this random data, but use black X's instead of the default markers. 

```df = pd.DataFrame({"xs":[1,5,2,8,1], "ys":[4,2,1,9,6]})```

Consult the [documentation](https://pandas.pydata.org/pandas-docs/stable/generated/pandas.DataFrame.plot.html) if you get stuck!


```python
import matplotlib.pyplot as plt
%matplotlib inline
plt.style.use('ggplot')

df = pd.DataFrame({"xs":[1,5,2,8,1], "ys":[4,2,1,9,6]})

df.plot.scatter("xs", "ys", color = "black", marker = "x")
```




    <AxesSubplot:xlabel='xs', ylabel='ys'>




    
![png](output_123_1.png)
    


**57.** Columns in your DataFrame can also be used to modify colors and sizes.  Bill has been keeping track of his performance at work over time, as well as how good he was feeling that day, and whether he had a cup of coffee in the morning.  Make a plot which incorporates all four features of this DataFrame.

(Hint:  If you're having trouble seeing the plot, try multiplying the Series which you choose to represent size by 10 or more)

*The chart doesn't have to be pretty: this isn't a course in data viz!*

```
df = pd.DataFrame({"productivity":[5,2,3,1,4,5,6,7,8,3,4,8,9],
                   "hours_in"    :[1,9,6,5,3,9,2,9,1,7,4,2,2],
                   "happiness"   :[2,1,3,2,3,1,2,3,1,2,2,1,3],
                   "caffienated" :[0,0,1,1,0,0,0,0,1,1,0,1,0]})
```


```python
df = pd.DataFrame({"productivity":[5,2,3,1,4,5,6,7,8,3,4,8,9],
                   "hours_in"    :[1,9,6,5,3,9,2,9,1,7,4,2,2],
                   "happiness"   :[2,1,3,2,3,1,2,3,1,2,2,1,3],
                   "caffienated" :[0,0,1,1,0,0,0,0,1,1,0,1,0]})

df.plot.scatter("hours_in", "productivity", s = df.happiness * 30, c = df.caffienated)
```




    <AxesSubplot:xlabel='hours_in', ylabel='productivity'>




    
![png](output_125_1.png)
    


**58.**  What if we want to plot multiple things?  Pandas allows you to pass in a matplotlib *Axis* object for plots, and plots will also return an Axis object.

Make a bar plot of monthly revenue with a line plot of monthly advertising spending (numbers in millions)

```
df = pd.DataFrame({"revenue":[57,68,63,71,72,90,80,62,59,51,47,52],
                   "advertising":[2.1,1.9,2.7,3.0,3.6,3.2,2.7,2.4,1.8,1.6,1.3,1.9],
                   "month":range(12)
                  })
```


```python
df = pd.DataFrame({"revenue":[57,68,63,71,72,90,80,62,59,51,47,52],
                   "advertising":[2.1,1.9,2.7,3.0,3.6,3.2,2.7,2.4,1.8,1.6,1.3,1.9],
                   "month":range(12)
                  })

ax = df.plot.bar("month", "revenue", color = "green")
df.plot.line("month", "advertising", secondary_y = True, ax = ax)
ax.set_xlim((-1,12))
```




    (-1.0, 12.0)




    
![png](output_127_1.png)
    


Now we're finally ready to create a candlestick chart, which is a very common tool used to analyze stock price data.  A candlestick chart shows the opening, closing, highest, and lowest price for a stock during a time window.  The color of the "candle" (the thick part of the bar) is green if the stock closed above its opening price, or red if below.

![Candlestick Example](img/candle.jpg)

This was initially designed to be a pandas plotting challenge, but it just so happens that this type of plot is just not feasible using pandas' methods.  If you are unfamiliar with matplotlib, we have provided a function that will plot the chart for you so long as you can use pandas to get the data into the correct format.

Your first step should be to get the data in the correct format using pandas' time-series grouping function.  We would like each candle to represent an hour's worth of data.  You can write your own aggregation function which returns the open/high/low/close, but pandas has a built-in which also does this.

The below cell contains helper functions.  Call ```day_stock_data()``` to generate a DataFrame containing the prices a hypothetical stock sold for, and the time the sale occurred.  Call ```plot_candlestick(df)``` on your properly aggregated and formatted stock data to print the candlestick chart.


```python
#This function is designed to create semi-interesting random stock price data

import numpy as np
def float_to_time(x):
    return str(int(x)) + ":" + str(int(x%1 * 60)).zfill(2) + ":" + str(int(x*60 % 1 * 60)).zfill(2)

def day_stock_data():
    #NYSE is open from 9:30 to 4:00
    time = 9.5
    price = 100
    results = [(float_to_time(time), price)]
    while time < 16:
        elapsed = np.random.exponential(.001)
        time += elapsed
        if time > 16:
            break
        price_diff = np.random.uniform(.999, 1.001)
        price *= price_diff
        results.append((float_to_time(time), price))
    
    
    df = pd.DataFrame(results, columns = ['time','price'])
    df.time = pd.to_datetime(df.time)
    return df

def plot_candlestick(agg):
    fig, ax = plt.subplots()
    for time in agg.index:
        ax.plot([time.hour] * 2, agg.loc[time, ["high","low"]].values, color = "black")
        ax.plot([time.hour] * 2, agg.loc[time, ["open","close"]].values, color = agg.loc[time, "color"], linewidth = 10)

    ax.set_xlim((8,16))
    ax.set_ylabel("Price")
    ax.set_xlabel("Hour")
    ax.set_title("OHLC of Stock Value During Trading Day")
    plt.show()
```

**59.** Generate a day's worth of random stock data, and aggregate / reformat it so that it has hourly summaries of the opening, highest, lowest, and closing prices


```python
df = day_stock_data()
df.head()
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
      <th>time</th>
      <th>price</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>2024-10-01 09:30:00</td>
      <td>100.000000</td>
    </tr>
    <tr>
      <th>1</th>
      <td>2024-10-01 09:30:07</td>
      <td>99.918037</td>
    </tr>
    <tr>
      <th>2</th>
      <td>2024-10-01 09:30:08</td>
      <td>99.954687</td>
    </tr>
    <tr>
      <th>3</th>
      <td>2024-10-01 09:30:10</td>
      <td>100.044063</td>
    </tr>
    <tr>
      <th>4</th>
      <td>2024-10-01 09:30:11</td>
      <td>100.040818</td>
    </tr>
  </tbody>
</table>
</div>




```python
df.set_index("time", inplace = True)
agg = df.resample("H").ohlc()
agg.columns = agg.columns.droplevel()
agg["color"] = (agg.close > agg.open).map({True:"green",False:"red"})
agg
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
      <th>open</th>
      <th>high</th>
      <th>low</th>
      <th>close</th>
      <th>color</th>
    </tr>
    <tr>
      <th>time</th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>2024-10-01 09:00:00</th>
      <td>100.000000</td>
      <td>101.520050</td>
      <td>98.623558</td>
      <td>99.003093</td>
      <td>red</td>
    </tr>
    <tr>
      <th>2024-10-01 10:00:00</th>
      <td>98.954078</td>
      <td>98.967985</td>
      <td>94.294978</td>
      <td>94.977975</td>
      <td>red</td>
    </tr>
    <tr>
      <th>2024-10-01 11:00:00</th>
      <td>95.050889</td>
      <td>97.007680</td>
      <td>94.426740</td>
      <td>96.237364</td>
      <td>green</td>
    </tr>
    <tr>
      <th>2024-10-01 12:00:00</th>
      <td>96.178514</td>
      <td>97.719937</td>
      <td>95.639788</td>
      <td>97.562842</td>
      <td>green</td>
    </tr>
    <tr>
      <th>2024-10-01 13:00:00</th>
      <td>97.511261</td>
      <td>99.240745</td>
      <td>97.190005</td>
      <td>98.101822</td>
      <td>green</td>
    </tr>
    <tr>
      <th>2024-10-01 14:00:00</th>
      <td>98.186962</td>
      <td>98.604292</td>
      <td>96.721650</td>
      <td>98.604292</td>
      <td>green</td>
    </tr>
    <tr>
      <th>2024-10-01 15:00:00</th>
      <td>98.699194</td>
      <td>100.538098</td>
      <td>97.800425</td>
      <td>97.800425</td>
      <td>red</td>
    </tr>
  </tbody>
</table>
</div>



**60.** Now that you have your properly-formatted data, try to plot it yourself as a candlestick chart.  Use the ```plot_candlestick(df)``` function above, or matplotlib's [```plot``` documentation](https://matplotlib.org/api/_as_gen/matplotlib.axes.Axes.plot.html) if you get stuck.


```python
plot_candlestick(agg)
```


    
![png](output_135_0.png)
    


*More exercises to follow soon...*
