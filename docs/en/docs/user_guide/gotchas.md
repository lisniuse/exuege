# Frequently Asked Questions (FAQ)

## DataFrame memory usage

The memory usage of a ``DataFrame`` (including the index) is shown when calling
the [``info()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html#pandas.DataFrame.info). A configuration option, ``display.memory_usage``
(see [the list of options](options.html#options-available)), specifies if the
``DataFrame``’s memory usage will be displayed when invoking the ``df.info()``
method.

For example, the memory usage of the ``DataFrame`` below is shown
when calling [``info()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html#pandas.DataFrame.info):

``` python
In [1]: dtypes = ['int64', 'float64', 'datetime64[ns]', 'timedelta64[ns]',
   ...:           'complex128', 'object', 'bool']
   ...: 

In [2]: n = 5000

In [3]: data = {t: np.random.randint(100, size=n).astype(t) for t in dtypes}

In [4]: df = pd.DataFrame(data)

In [5]: df['categorical'] = df['object'].astype('category')

In [6]: df.info()
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5000 entries, 0 to 4999
Data columns (total 8 columns):
int64              5000 non-null int64
float64            5000 non-null float64
datetime64[ns]     5000 non-null datetime64[ns]
timedelta64[ns]    5000 non-null timedelta64[ns]
complex128         5000 non-null complex128
object             5000 non-null object
bool               5000 non-null bool
categorical        5000 non-null category
dtypes: bool(1), category(1), complex128(1), datetime64[ns](1), float64(1), int64(1), object(1), timedelta64[ns](1)
memory usage: 289.1+ KB
```

The ``+`` symbol indicates that the true memory usage could be higher, because
pandas does not count the memory used by values in columns with
``dtype=object``.

Passing ``memory_usage='deep'`` will enable a more accurate memory usage report,
accounting for the full usage of the contained objects. This is optional
as it can be expensive to do this deeper introspection.

``` python
In [7]: df.info(memory_usage='deep')
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 5000 entries, 0 to 4999
Data columns (total 8 columns):
int64              5000 non-null int64
float64            5000 non-null float64
datetime64[ns]     5000 non-null datetime64[ns]
timedelta64[ns]    5000 non-null timedelta64[ns]
complex128         5000 non-null complex128
object             5000 non-null object
bool               5000 non-null bool
categorical        5000 non-null category
dtypes: bool(1), category(1), complex128(1), datetime64[ns](1), float64(1), int64(1), object(1), timedelta64[ns](1)
memory usage: 425.6 KB
```

By default the display option is set to ``True`` but can be explicitly
overridden by passing the ``memory_usage`` argument when invoking ``df.info()``.

The memory usage of each column can be found by calling the
[``memory_usage()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.memory_usage.html#pandas.DataFrame.memory_usage) method. This returns a ``Series`` with an index
represented by column names and memory usage of each column shown in bytes. For
the ``DataFrame`` above, the memory usage of each column and the total memory
usage can be found with the ``memory_usage`` method:

``` python
In [8]: df.memory_usage()
Out[8]: 
Index                128
int64              40000
float64            40000
datetime64[ns]     40000
timedelta64[ns]    40000
complex128         80000
object             40000
bool                5000
categorical        10920
dtype: int64

# total memory usage of dataframe
In [9]: df.memory_usage().sum()
Out[9]: 296048
```

By default the memory usage of the ``DataFrame``’s index is shown in the
returned ``Series``, the memory usage of the index can be suppressed by passing
the ``index=False`` argument:

``` python
In [10]: df.memory_usage(index=False)
Out[10]: 
int64              40000
float64            40000
datetime64[ns]     40000
timedelta64[ns]    40000
complex128         80000
object             40000
bool                5000
categorical        10920
dtype: int64
```

The memory usage displayed by the [``info()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.info.html#pandas.DataFrame.info) method utilizes the
[``memory_usage()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.memory_usage.html#pandas.DataFrame.memory_usage) method to determine the memory usage of a
``DataFrame`` while also formatting the output in human-readable units (base-2
representation; i.e. 1KB = 1024 bytes).

See also [Categorical Memory Usage](categorical.html#categorical-memory).

## Using if/truth statements with pandas

pandas follows the NumPy convention of raising an error when you try to convert
something to a ``bool``. This happens in an ``if``-statement or when using the
boolean operations: ``and``, ``or``, and ``not``. It is not clear what the result
of the following code should be:

``` python
>>> if pd.Series([False, True, False]):
...     pass
```

Should it be ``True`` because it’s not zero-length, or ``False`` because there
are ``False`` values? It is unclear, so instead, pandas raises a ``ValueError``:

``` python
>>> if pd.Series([False, True, False]):
...     print("I was true")
Traceback
    ...
ValueError: The truth value of an array is ambiguous. Use a.empty, a.any() or a.all().
```

You need to explicitly choose what you want to do with the ``DataFrame``, e.g.
use [``any()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.any.html#pandas.DataFrame.any), [``all()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.all.html#pandas.DataFrame.all) or [``empty()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.empty.html#pandas.DataFrame.empty).
Alternatively, you might want to compare if the pandas object is ``None``:

``` python
>>> if pd.Series([False, True, False]) is not None:
...     print("I was not None")
I was not None
```

Below is how to check if any of the values are ``True``:

``` python
>>> if pd.Series([False, True, False]).any():
...     print("I am any")
I am any
```

To evaluate single-element pandas objects in a boolean context, use the method
[``bool()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.bool.html#pandas.DataFrame.bool):

``` python
In [11]: pd.Series([True]).bool()
Out[11]: True

In [12]: pd.Series([False]).bool()
Out[12]: False

In [13]: pd.DataFrame([[True]]).bool()
Out[13]: True

In [14]: pd.DataFrame([[False]]).bool()
Out[14]: False
```

### Bitwise boolean

Bitwise boolean operators like ``==`` and ``!=`` return a boolean ``Series``,
which is almost always what you want anyways.

``` python
>>> s = pd.Series(range(5))
>>> s == 4
0    False
1    False
2    False
3    False
4     True
dtype: bool
```

See [boolean comparisons](https://pandas.pydata.org/pandas-docs/stable/getting_started/basics.html#basics-compare) for more examples.

### Using the ``in`` operator

Using the Python ``in`` operator on a ``Series`` tests for membership in the
index, not membership among the values.

``` python
In [15]: s = pd.Series(range(5), index=list('abcde'))

In [16]: 2 in s
Out[16]: False

In [17]: 'b' in s
Out[17]: True
```

If this behavior is surprising, keep in mind that using ``in`` on a Python
dictionary tests keys, not values, and ``Series`` are dict-like.
To test for membership in the values, use the method [``isin()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.isin.html#pandas.Series.isin):

``` python
In [18]: s.isin([2])
Out[18]: 
a    False
b    False
c     True
d    False
e    False
dtype: bool

In [19]: s.isin([2]).any()
Out[19]: True
```

For ``DataFrames``, likewise, ``in`` applies to the column axis,
testing for membership in the list of column names.

## ``NaN``, Integer ``NA`` values and ``NA`` type promotions

### Choice of ``NA`` representation

For lack of ``NA`` (missing) support from the ground up in NumPy and Python in
general, we were given the difficult choice between either:

- A *masked array* solution: an array of data and an array of boolean values
indicating whether a value is there or is missing.
- Using a special sentinel value, bit pattern, or set of sentinel values to
denote ``NA`` across the dtypes.

For many reasons we chose the latter. After years of production use it has
proven, at least in my opinion, to be the best decision given the state of
affairs in NumPy and Python in general. The special value ``NaN``
(Not-A-Number) is used everywhere as the ``NA`` value, and there are API
functions ``isna`` and ``notna`` which can be used across the dtypes to
detect NA values.

However, it comes with it a couple of trade-offs which I most certainly have
not ignored.

### Support for integer ``NA``

In the absence of high performance ``NA`` support being built into NumPy from
the ground up, the primary casualty is the ability to represent NAs in integer
arrays. For example:

``` python
In [20]: s = pd.Series([1, 2, 3, 4, 5], index=list('abcde'))

In [21]: s
Out[21]: 
a    1
b    2
c    3
d    4
e    5
dtype: int64

In [22]: s.dtype
Out[22]: dtype('int64')

In [23]: s2 = s.reindex(['a', 'b', 'c', 'f', 'u'])

In [24]: s2
Out[24]: 
a    1.0
b    2.0
c    3.0
f    NaN
u    NaN
dtype: float64

In [25]: s2.dtype
Out[25]: dtype('float64')
```

This trade-off is made largely for memory and performance reasons, and also so
that the resulting ``Series`` continues to be “numeric”.

If you need to represent integers with possibly missing values, use one of
the nullable-integer extension dtypes provided by pandas

- [``Int8Dtype``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Int8Dtype.html#pandas.Int8Dtype)
- [``Int16Dtype``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Int16Dtype.html#pandas.Int16Dtype)
- [``Int32Dtype``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Int32Dtype.html#pandas.Int32Dtype)
- [``Int64Dtype``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Int64Dtype.html#pandas.Int64Dtype)

``` python
In [26]: s_int = pd.Series([1, 2, 3, 4, 5], index=list('abcde'),
   ....:                   dtype=pd.Int64Dtype())
   ....: 

In [27]: s_int
Out[27]: 
a    1
b    2
c    3
d    4
e    5
dtype: Int64

In [28]: s_int.dtype
Out[28]: Int64Dtype()

In [29]: s2_int = s_int.reindex(['a', 'b', 'c', 'f', 'u'])

In [30]: s2_int
Out[30]: 
a      1
b      2
c      3
f    NaN
u    NaN
dtype: Int64

In [31]: s2_int.dtype
Out[31]: Int64Dtype()
```

See [Nullable integer data type](integer_na.html#integer-na) for more.

### ``NA`` type promotions

When introducing NAs into an existing ``Series`` or ``DataFrame`` via
[``reindex()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.Series.reindex.html#pandas.Series.reindex) or some other means, boolean and integer types will be
promoted to a different dtype in order to store the NAs. The promotions are
summarized in this table:

Typeclass | Promotion dtype for storing NAs
---|---
floating | no change
object | no change
integer | cast to float64
boolean | cast to object

While this may seem like a heavy trade-off, I have found very few cases where
this is an issue in practice i.e. storing values greater than 2**53. Some
explanation for the motivation is in the next section.

### Why not make NumPy like R?

Many people have suggested that NumPy should simply emulate the ``NA`` support
present in the more domain-specific statistical programming language [R](https://r-project.org). Part of the reason is the NumPy type hierarchy:

Typeclass | Dtypes
---|---
numpy.floating | float16, float32, float64, float128
numpy.integer | int8, int16, int32, int64
numpy.unsignedinteger | uint8, uint16, uint32, uint64
numpy.object_ | object_
numpy.bool_ | bool_
numpy.character | string_, unicode_

The R language, by contrast, only has a handful of built-in data types:
``integer``, ``numeric`` (floating-point), ``character``, and
``boolean``. ``NA`` types are implemented by reserving special bit patterns for
each type to be used as the missing value. While doing this with the full NumPy
type hierarchy would be possible, it would be a more substantial trade-off
(especially for the 8- and 16-bit data types) and implementation undertaking.

An alternate approach is that of using masked arrays. A masked array is an
array of data with an associated boolean *mask* denoting whether each value
should be considered ``NA`` or not. I am personally not in love with this
approach as I feel that overall it places a fairly heavy burden on the user and
the library implementer. Additionally, it exacts a fairly high performance cost
when working with numerical data compared with the simple approach of using
``NaN``. Thus, I have chosen the Pythonic “practicality beats purity” approach
and traded integer ``NA`` capability for a much simpler approach of using a
special value in float and object arrays to denote ``NA``, and promoting
integer arrays to floating when NAs must be introduced.

## Differences with NumPy

For ``Series`` and ``DataFrame`` objects, [``var()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.var.html#pandas.DataFrame.var) normalizes by
``N-1`` to produce unbiased estimates of the sample variance, while NumPy’s
``var`` normalizes by N, which measures the variance of the sample. Note that
[``cov()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.cov.html#pandas.DataFrame.cov) normalizes by ``N-1`` in both pandas and NumPy.

## Thread-safety

As of pandas 0.11, pandas is not 100% thread safe. The known issues relate to
the [``copy()``](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.copy.html#pandas.DataFrame.copy) method. If you are doing a lot of copying of
``DataFrame`` objects shared among threads, we recommend holding locks inside
the threads where the data copying occurs.

See [this link](https://stackoverflow.com/questions/13592618/python-pandas-dataframe-thread-safe)
for more information.

## Byte-Ordering issues

Occasionally you may have to deal with data that were created on a machine with
a different byte order than the one on which you are running Python. A common
symptom of this issue is an error like::

``` python
Traceback
    ...
ValueError: Big-endian buffer not supported on little-endian compiler
```

To deal
with this issue you should convert the underlying NumPy array to the native
system byte order *before* passing it to ``Series`` or ``DataFrame``
constructors using something similar to the following:

``` python
In [32]: x = np.array(list(range(10)), '>i4')  # big endian

In [33]: newx = x.byteswap().newbyteorder()  # force native byteorder

In [34]: s = pd.Series(newx)
```

See [the NumPy documentation on byte order](https://docs.scipy.org/doc/numpy/user/basics.byteswapping.html) for more
details.
