# Pandas教程

## 1.Creating, Reading and Writing

### Creating data

#### DataFrame

```python
# 字典的键Yes和No为列标签，字典的值list为一列的值
pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
# 行标签通过index指定
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
              index=['Product A', 'Product B'])
```

#### Series

```python
# Series is a sequence of data values.
pd.Series([1, 2, 3, 4, 5])
# index指定行标签，Series没有列名，但有一个全局的name
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```

### Reading data files

```python
# 使用第0列为index，即行标签
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)
wine_reviews.head()
# 保存到另一个文件
wine_reviews.to_csv('demo.csv')
```

## 2.Indexing, Selecting & Assigning