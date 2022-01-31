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
- `reviews.points.describe()` gives dataser's summary, also possible to use on columns of dataset
- `.mean()` get the mean of the row, ex: *`reviews.points.mean()`*
- `.unique()` get the unique values, so let's filter the duplicate ones, ex: *`reviews.taster_name.unique()`*
- `.value_counts()`  how often the unique values appear, ex: *`reviews.taster_name.value_counts()`* => out: `Roger Voss           25514`

### Maps
> A map is a term, borrowed from mathematics, for a function that takes one set of values and "maps" them to another set of values. In data science we often have a need for creating new representations from existing data, or for transforming data from the format it is in now to the format that we want it to be in later.
>
>  There are two mapping methods that you will use often.

**Note that `map()` and `apply()` return new, transformed `Series` and `DataFrames`, respectively. They don't modify the original data they're called on.**

#### `map()`
The function you pass to `map()` should expect a single value from the `Series` (a point value, in the above example), and return a transformed version of that value. 

<table>
  <tr><th>example</th><th>Output</th></tr>
<tr>
<td>

```Python
# map() is the first, and slightly simpler one.
# For example, suppose that we wanted to remean
# the scores the wines received to 0. 
#
# We can do this as follows:

review_points_mean = reviews.points.mean()
reviews.points.map(lambda p: p - review_points_mean)
```
  
</td>
<td>
  
```
0        -1.447138
1        -1.447138
            ...   
129969    1.552862
129970    1.552862
Name: points, Length: 129971, dtype: float64
```
  
</td>
</tr>
<tr>
<td>
  
Create variable `centered_price` containing a version of the `price` column with the mean price subtracted.    
```Python
centered_price = reviews.price.map(lambda p: p-reviews.price.mean())

# of course this is equal to 
# centered_price = reviews.price - reviews.price.mean()
```
    
</td>
<td>

```
0               NaN
1        -20.363389
            ...    
129969    -3.363389
129970   -14.363389
Name: price, Length: 129971, dtype: float64
```
    
</td>
</tr>
</table>

#### `apply()`
`apply()` is the equivalent method if we want to transform a whole `DataFrame` by calling a custom method on each row.

If we had called `reviews.apply()` with `axis='index'`, then instead of passing a function to transform each row, we would need to give a function to transform each column.

<table>
<tr><td>

  ```Python
def remean_points(row):
    row.points = row.points - review_points_mean
    return row

reviews.apply(remean_points, axis='columns')
```

</td>
<td></td>
</tr><tr><td>
  
```Python
#  score of 95 or higher counts as 3 stars, 
# a score of at least 85 but less than 95 is 2 stars. 
# Any other score is 1 star.
#
# any wines from Canada should automatically get 3 stars,
# regardless of points.
#
# Create a series star_ratings with the number of stars 
# corresponding to each review in the dataset

def stars(row):
    if row.country == 'Canada':
        return 3
    elif row.points >= 95:
        return 3
    elif row.points >= 85:
        return 2
    else:
        return 1

star_ratings = reviews.apply(stars, axis='columns')
```

</td><td>
  
```
0         2
1         2
         ..
129969    2
129970    2
Length: 129971, dtype: int64
```
  
</td>
</tr>
</table>

## Grouping and Sorting
often we want to group our data, and then do something specific to the group the data is in

### Groupwise analysis
One function we've been using heavily thus far is the `value_counts()` function. We can replicate what `value_counts()` does by doing the following:
<table><tr><td>

```Python
reviews.points
```

</td><td>

```
0         87
1         87
2         87
3         87
4         87
          ..
129966    90
129967    90
129968    90
129969    90
129970    90
Name: points, Length: 129971, dtype: int64
```
  </td></tr><tr><td>
  
```Python
reviews.groupby('points').points.count()
```

</td><td>
  
```
points
80     397
81     692
      ... 
99      33
100     19
Name: points, Length: 21, dtype: int64
```
  
</td>
</tr>
</table>

`groupby()` created a group of reviews which allotted the same point values to the given wines. Then, for each of these groups, we grabbed the `points()` column and counted how many times it appeared. `value_counts()` is just a shortcut to this `groupby()` operation.

For example, here's one way of selecting the name of the first wine reviewed from each winery in the dataset: `reviews.groupby('winery').apply(lambda df: df.title.iloc[0])`

For even more fine-grained control, you can also group by more than one column. For an example, here's how we would pick out the best wine by country and province: `reviews.groupby(['country', 'province']).apply(lambda df: df.loc[df.points.idxmax()])`

#### `agg()`

Lets you run a bunch of different functions on your DataFrame simultaneously. For example, we can generate a simple statistical summary of the dataset as follows:
```Python
reviews.groupby(['country']).price.agg([len, min, max])
```
Out
```
            	len 	min 	max
country 			
Argentina 	3800 	4.0 	230.0
Armenia 	2 	14.0 	15.0
   ...     	... 	... 	...
Ukraine 	14 	6.0 	13.0
Uruguay 	109 	10.0 	130.0

43 rows × 3 columns
```

### Multi-indexes
`groupby()` is slightly different in the fact that, depending on the operation we run, it will sometimes result in what is called a multi-index. A multi-index differs from a regular index in that it has multiple levels. For example:
```Python
countries_reviewed = reviews.groupby(['country', 'province']).description.agg([len])
countries_reviewed
```
Out:
```
	      	                    len
country     province 	
Argentina   Mendoza Province  3264
            Other             536
...         ...               ...
Uruguay     San Jose          3
            Uruguay           24
```
The use cases for a multi-index are detailed alongside instructions on using them in the [MultiIndex / Advanced Selection](https://pandas.pydata.org/pandas-docs/stable/user_guide/advanced.html) section of the pandas documentation.

However, in general the multi-index method you will use most often is the one for converting back to a regular index, the `reset_index()` method:

```Python
countries_reviewed.reset_index()
```
Out:
```
	country 	province              len
0 	Argentina 	Mendoza Province  3264
1 	Argentina 	Other             536
... 	...       ...               ...
423 	Uruguay 	San Jose          3
424 	Uruguay 	Uruguay           24

425 rows × 3 columns
```
### Sorting
- grouping returns data in index order, not in value order
- To get data in the order want it in we can sort it ourselves. The `sort_values()` method is handy for this.
- also we can set descending sort by `ascending=False` in `sort_values()` argument

```Python
countries_reviewed = countries_reviewed.reset_index()
countries_reviewed.sort_values(by='len')
```
Out:
```
	    country 	province 	           len
179 	Greece 	Muscat of Kefallonian  1
192 	Greece 	Sterea Ellada          1
... 	... 	...                      ...
415 	US 	Washington                 8639
392 	US 	California                 36247

425 rows × 3 columns
```

sort by more than one column at a time:
```Python
countries_reviewed.sort_values(by=['country', 'len'])
```
Out:
```
      country     province           len
1     Argentina   Other              536
0     Argentina   Mendoza Province   3264
... 	...         ...                ...
424   Uruguay     Uruguay            24
419   Uruguay     Canelones          43

425 rows × 3 columns
```

more in [exercise-grouping-and-sorting.ipynb](https://github.com/gabboraron/Pandas-Kaggle/blob/main/exercise-grouping-and-sorting.ipynb)

## Data Types and Missing Values
### Dtypes
The data type for a column in a DataFrame or a Series is known as the dtype. You can use the `dtype()` property to grab the type of a specific column. Alternatively, the dtypes property returns the dtype of every column in the DataFrame: `reviews.dtypes`. 

It's possible to convert a column of one type into another wherever such a conversion makes sense by using the `astype()` function. For example, we may transform the points column from its existing `int64` data type into a `float64` data type: `reviews.points.astype('float64')`

### Missing data
To select `NaN` entries you can use `pd.isnull()` (or its companion `pd.notnull()`). Pandas provides a really handy method for this problem: `fillna()`. `reviews.region_2.fillna("Unknown")` replace each NaN with an "Unknown".

`.replace("@kerinokeefe", "@kerino")` replace *@kerinokeefe* to *@kerino*. 
 
more in [exercise-data-types-and-missing-values.ipynb](https://github.com/gabboraron/Pandas-Kaggle/blob/main/exercise-data-types-and-missing-values.ipynb)

## Renaming and Combining
Oftentimes data will come to us with column names, index names, or other naming conventions that we are not satisfied with.

- `rename()` 
   - lets you change index names and/or column names. ex: *to change the points column in our dataset to score, we would do: `reviews.rename(columns={'points': 'score'})`*
   - rename index or column values by specifying a index or column keyword parameter, respectively: `reviews.rename(index={0: 'firstEntry', 1: 'secondEntry'})`
- `set_index()` rename indexes more convenient
- `rename_axis()` rename a row and a column index, ex: *`reviews.rename_axis("wines", axis='rows').rename_axis("fields", axis='columns')`*

When performing operations on a dataset, we will sometimes need to combine different DataFrames and/or Series in non-trivial ways. 
- `merge()` - `join()` - 
- `concat()` - simplest combining method 

Concat different databases with same structure:
```Python
canadian_youtube = pd.read_csv("../input/youtube-new/CAvideos.csv")
british_youtube = pd.read_csv("../input/youtube-new/GBvideos.csv")

pd.concat([canadian_youtube, british_youtube])
```
same:
```Python
left = canadian_youtube.set_index(['title', 'trending_date'])
right = british_youtube.set_index(['title', 'trending_date'])

left.join(right, lsuffix='_CAN', rsuffix='_UK')
```

more in [exercise-renaming-and-combining.ipynb](https://github.com/gabboraron/Pandas-Kaggle/blob/main/exercise-renaming-and-combining.ipynb)
