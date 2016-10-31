---
layout: post
title: "Pandas DataFrame"
excerpt: "DataFrame"
categories: knownotes
tags: [python, pandas]
comments: true
share: true
author: chungyu
---

> mainly from
> * [10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)

# The data

```python
dates = pd.date_range('20130101', periods=6)

# DatetimeIndex(['2013-01-01', '2013-01-02', '2013-01-03', '2013-01-04',
#                '2013-01-05', '2013-01-06'],
#               dtype='datetime64[ns]', freq='D')

df = pd.DataFrame(np.random.randn(6,4), index=dates, columns=list('ABCD'))

#                    A         B         C         D
# 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
# 2013-01-04  0.721555 -0.706771 -1.039575  0.271860
# 2013-01-05 -0.424972  0.567020  0.276232 -1.087401
# 2013-01-06 -0.673690  0.113648 -1.478427  0.524988
```

# Accessing

#### Getting Submatrix
`df[0:3]`: Selecting via [], which slices the rows.
```python
df[0:3]
#                A         B         C         D
# 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804

df['20130102':'20130104']
#                  A         B         C         D
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
# 2013-01-04  0.721555 -0.706771 -1.039575  0.271860
```

`df.loc[:,['A','B']]`: Selecting on a multi-axis by label

```python
df.loc[:,['A','B']]
#                    A         B
# 2013-01-01  0.469112 -0.282863
# 2013-01-02  1.212112 -0.173215
# 2013-01-03 -0.861849 -2.104569
# 2013-01-04  0.721555 -0.706771
# 2013-01-05 -0.424972  0.567020
# 2013-01-06 -0.673690  0.113648

df.loc['20130102':'20130104',['A','B']]
#                    A         B
# 2013-01-02  1.212112 -0.173215
# 2013-01-03 -0.861849 -2.104569
# 2013-01-04  0.721555 -0.706771

```

`df.iloc[3:5,0:2]`: By integer slices, acting similar to numpy/python

```python
df.iloc[3:5,0:2]
#                    A         B
# 2013-01-04  0.721555 -0.706771
# 2013-01-05 -0.424972  0.567020
```

`df.iloc[[1,2,4],[0,2]]`: By lists of integer position locations, similar to the numpy/python style

```python
df.iloc[[1,2,4],[0,2]]
#                    A         C
# 2013-01-02  1.212112  0.119209
# 2013-01-03 -0.861849 -0.494929
# 2013-01-05 -0.424972  0.276232
```


#### Getting Column

`df['A']`: Selecting a single column, which yields a Series, equivalent to `df.A`

```python
df['A']
# 2013-01-01    0.469112
# 2013-01-02    1.212112
# 2013-01-03   -0.861849
# 2013-01-04    0.721555
# 2013-01-05   -0.424972
# 2013-01-06   -0.673690
# Freq: D, Name: A, dtype: float64
```

#### Getting Row

`df.iloc[3]`: Select via the position of the passed integers

```python
df.iloc[3]
# A    0.721555
# B   -0.706771
# C   -1.039575
# D    0.271860
# Name: 2013-01-04 00:00:00, dtype: float64
```

`df.loc[dates[0]]`: getting a cross section using a label
```python
df.loc[dates[0]]
# A    0.469112
# B   -0.282863
# C   -1.509059
# D   -1.135632
# Name: 2013-01-01 00:00:00, dtype: float64
```

#### Getting Cell

```python
df.loc[dates[0],'A']
# 0.46911229990718628

df.at[dates[0],'A']
# 0.46911229990718628

df.iloc[1,1]
# -0.17321464905330858

df.iat[1,1]
# -0.17321464905330858
```

# Filtering

`df[df.A > 0]`: Using a single column’s values to select data.
```python
df[df.A > 0]
#                    A         B         C         D
# 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
# 2013-01-04  0.721555 -0.706771 -1.039575  0.271860
```

`df[df > 0]`: A where operation for getting.

```python
df[df > 0]
#                    A         B         C         D
# 2013-01-01  0.469112       NaN       NaN       NaN
# 2013-01-02  1.212112       NaN  0.119209       NaN
# 2013-01-03       NaN       NaN       NaN  1.071804
# 2013-01-04  0.721555       NaN       NaN  0.271860
# 2013-01-05       NaN  0.567020  0.276232       NaN
# 2013-01-06       NaN  0.113648       NaN  0.524988
```

`isin()`

```python
df2 = df.copy()

df2['E'] = ['one', 'one','two','three','four','three']
#                    A         B         C         D      E
# 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632    one
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236    one
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804    two
# 2013-01-04  0.721555 -0.706771 -1.039575  0.271860  three
# 2013-01-05 -0.424972  0.567020  0.276232 -1.087401   four
# 2013-01-06 -0.673690  0.113648 -1.478427  0.524988  three

df2[df2['E'].isin(['two','four'])]
#                    A         B         C         D     E
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804   two
# 2013-01-05 -0.424972  0.567020  0.276232 -1.087401  four

```
