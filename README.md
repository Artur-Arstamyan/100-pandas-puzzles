# Created issues
[Fixing AttributeError & suggesting Alternative solution for Puzzle N53](https://github.com/ajcr/100-pandas-puzzles/issues/49#issue-2559633093)  

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
