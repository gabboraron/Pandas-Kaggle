# Pandas - Kaggle
> Solve short hands-on challenges to perfect your data manipulation skills.

## Creating, Reading and Writing
In this tutorial, you will learn how to create your own data, along with how to work with data that already exists.

```Python
import pandas as pd
```
### Objects
There are two core objects in pandas: the `DataFrame` and the `Series`.

`pd.DataFrame()` constructor to generate these `DataFrame` objects. The syntax for declaring a new one is a dictionary whose keys are the column names *(later Yes/No; Bob/Sue in example)*, and whose values are a list of entries. This is the standard way of constructing a new `DataFrame`.

```Python
# A DataFrame is a table. 
# It contains an array of individual entries, each of which has a certain value. 
# Each entry corresponds to a row (or record) and a column.

pd.DataFrame({'Yes': [50, 21], 'No': [131, 2]})
```
Out:
```
  	Yes 	No
0 	50 	131
1 	21 	2
```

```Python
# DataFrame entries are not limited to integers. 
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 'Sue': ['Pretty good.', 'Bland.']})
```
Out:
```
      Bob 	          Sue
0 	I liked it. 	Pretty good.
1 	It was awful. 	Bland.
```

The dictionary-list constructor assigns values to the column labels, but just uses an ascending count from 0 (0, 1, 2, 3, ...) for the row labels. Sometimes this is OK, but oftentimes we will want to assign these labels ourselves:
```Python
pd.DataFrame({'Bob': ['I liked it.', 'It was awful.'], 
              'Sue': ['Pretty good.', 'Bland.']},
             index=['Product A', 'Product B'])
```

Out:
```
          	Bob 	        Sue
Product A 	I liked it. 	Pretty good.
Product B 	It was awful. 	Bland.
```
### Series
A `Series`, by contrast, is a sequence of data values. If a `DataFrame` is a *table*, a `Series` is a *list*. And in fact you can create one with nothing more than a list:
```Python
pd.Series([1, 2, 3, 4, 5])
```
Out:
```
0    1
1    2
2    3
3    4
4    5
dtype: int64
```

You can assign column values to the `Series` the same way as before, using an `index` parameter. However, a `Series` does not have a column name, it only has one overall `name`:

```Python
pd.Series([30, 35, 40], index=['2015 Sales', '2016 Sales', '2017 Sales'], name='Product A')
```
Out:
```
2015 Sales    30
2016 Sales    35
2017 Sales    40
Name: Product A, dtype: int64
```

> *It's helpful to think of a `DataFrame` as actually being just a bunch of `Series` "glued together".*

Let's now set aside our toy datasets and see what a real dataset looks like when we read it into a `DataFrame`. We'll use the `pd.read_csv()` function to read the data into a `DataFrame`. This goes thusly:

```Python
wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv")
wine_reviews.shape #to check how large the resulting DataFrame
```
Out:
```
(129971, 14)
```
*This means 129971 records in each row in 14 columns separated, which means 1820000 entries.*

### useful short commands
- `wine_reviews.head()` - gives fst 5 row of dataset
- `wine_reviews.head()` - can have more than 30 optinoal parameter
   - `index_col=0` parameter to set fst column as index: *`wine_reviews = pd.read_csv("../input/wine-reviews/winemag-data-130k-v2.csv", index_col=0)`*
- `animals.to_csv("cows_and_goats.csv")` - save *animals* `DataFrame` into `cows_and_goats.csv`file on the disk

## Indexing, Selecting & Assigning
One of the first things you need to learn in working with data in Python is [how to go about selecting the data points](https://www.kaggle.com/residentmario/indexing-selecting-assigning) relevant to you quickly and effectively

- `desc = reviews['description']` and `desc = reviews.description` both gives the same result: the `description` column from [`reviews`](https://www.kaggle.com/zynicide/wine-reviews) and assign the result to the variable `desc`.
- `reviews.description.iloc[0]` and `reviews.description.loc[0]` and `reviews.description[0]` gives te same: the first value from the *description* column of *reviews*. **Both loc and iloc are row-first, column-second.**
- `reviews.iloc[0]` gives back the first row of *reviews* dataset
- `reviews.description.iloc[:10]` and `reviews.description.head(10)` and `reviews.loc[:9, "description"]` are the same, and give back the first 10 values from the *description* column in *reviews*
- `sample_reviews = reviews.iloc[[1,2,3,5,8],:]` - Select the records with index labels *1, 2, 3, 5*, and *8*, assigning the result to the variable *`sample_reviews`*
- `df = reviews.loc[[0,1,10,100], ['country', 'province', 'region_1', 'region_2']]` - Create a variable *`df`* containing the *country, province, region_1*, and *region_2* columns of the records with the index labels *0, 1, 10*, and *100*
- `df = reviews.loc[:99,['country','variety']]` - Create a variable *`df`* containing the *country* and *variety* columns of the first 100 records. **In this case `df.iloc[0:1000]` will return 1000 entries, while `df.loc[0:1000]` return 1001 of them!** 
- `italian_wines = reviews[reviews.country == 'Italy']` - Create a *DataFrame* *italian_wines* containing reviews of wines made in *Italy*
- `top_oceania_wines = reviews.loc[(reviews.country.isin(['Australia', 'New Zealand'])) & (reviews.points >= 95)]` - Create a DataFrame *`top_oceania_wines`* containing all reviews with *at least 95 points* (out of 100) for wines from *Australia or New Zealand*.
- `reviews.loc[reviews.price.notnull()]` -  ˛`isnull` (and its companion `notnull`). These methods let you highlight values which are (or are not) empty (NaN). 

It's worth knowing that negative numbers can be used in selection. This will start counting forwards from the end of the values `reviews.iloc[-5:]`

Label-based selection derives its power from the labels in the index. Critically, the index we use is not immutable. We can manipulate the index in any way we see fit. `reviews.set_index("title")` 

## Summary Functions and Maps
- `reviews.points.describe()` gives dataser's summary
- 

```Python

```



