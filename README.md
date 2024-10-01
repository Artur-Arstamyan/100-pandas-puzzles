# Notable exercises
## Exercise N21 
**Problem**  
For each animal type and each number of visits, find the mean age. In other words, each row is an animal, each column is a number of visits and the values are the mean ages (hint: use a pivot table).  
**Code**
```python
df.pivot_table(index='animal', columns='visits', values='age', aggfunc='mean')
```
## Exercise N28
**Problem** 
For each group of 10 consecutive integers in column 'A' (i.e. (0, 10], (10, 20], ...), calculate the sum of the corresponding values in column 'B'.  
**Code**
```python
df = pd.DataFrame(np.random.RandomState(8765).randint(1, 101, size=(100, 2)), columns = ["A", "B"])
df.groupby(pd.cut(df['A'], np.arange(0, 101, 10)))['B'].sum()
```
## Exercise N53 - [Fixing AttributeError & suggesting Alternative solution](https://github.com/ajcr/100-pandas-puzzles/issues/49#issue-2559633093)
**AttributeError**
- **Code**
```python
from scipy.signal import convolve2d
mine_grid = df.pivot_table(columns='x', index='y', values='mine')
counts = convolve2d(mine_grid.astype(complex), np.ones((3, 3)), mode='same').real.astype(int)
df['adjacent'] = (counts - mine_grid).ravel('F')
df
```
- **Output**
```
AttributeError: 'DataFrame' object has no attribute 'ravel'
```
- **Fixing** using **to_numpy()** 
```
df['adjacent'] = (counts - mine_grid).to_numpy().ravel('F')
```

**Alternative solution**
- **Code**
```python
from numpy.lib.stride_tricks import sliding_window_view
arr = df['mine'].values.reshape(5, 4)
df['adjacent'] = (sliding_window_view(np.pad(arr, 1), window_shape=(3, 3)).sum(axis=(-2, -1))-arr).ravel()
print(df)
```
