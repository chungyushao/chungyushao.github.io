---
layout: post
title: "Pandas DataFrame Operation"
excerpt: "operation, groupby, apply"
categories: knownotes
tags: [python, pandas]
comments: true
share: true
author: chungyu
---

> mainly from
> * [10 Minutes to pandas](http://pandas.pydata.org/pandas-docs/stable/10min.html)

# Operation
```python
df.sort_index(axis=1, ascending=False)
#                    D         C         B         A
# 2013-01-01 -1.135632 -1.509059 -0.282863  0.469112
# 2013-01-02 -1.044236  0.119209 -0.173215  1.212112
# 2013-01-03  1.071804 -0.494929 -2.104569 -0.861849
# 2013-01-04  0.271860 -1.039575 -0.706771  0.721555
# 2013-01-05 -1.087401  0.276232  0.567020 -0.424972
# 2013-01-06  0.524988 -1.478427  0.113648 -0.673690

df.sort_values(by='B')
#                    A         B         C         D
# 2013-01-03 -0.861849 -2.104569 -0.494929  1.071804
# 2013-01-04  0.721555 -0.706771 -1.039575  0.271860
# 2013-01-01  0.469112 -0.282863 -1.509059 -1.135632
# 2013-01-02  1.212112 -0.173215  0.119209 -1.044236
# 2013-01-06 -0.673690  0.113648 -1.478427  0.524988
# 2013-01-05 -0.424972  0.567020  0.276232 -1.087401

df.describe()
#               A         B         C         D
# count  6.000000  6.000000  6.000000  6.000000
# mean   0.073711 -0.431125 -0.687758 -0.233103
# std    0.843157  0.922818  0.779887  0.973118
# min   -0.861849 -2.104569 -1.509059 -1.135632
# 25%   -0.611510 -0.600794 -1.368714 -1.076610
# 50%    0.022070 -0.228039 -0.767252 -0.386188
# 75%    0.658444  0.041933 -0.034326  0.461706
# max    1.212112  0.567020  0.276232  1.071804
```

# Apply
`df.apply(np.cumsum)`:

```python
df.apply(np.cumsum)
#                    A         B         C   D     F
# 2013-01-01  0.000000  0.000000 -1.509059   5   NaN
# 2013-01-02  1.212112 -0.173215 -1.389850  10   1.0
# 2013-01-03  0.350263 -2.277784 -1.884779  15   3.0
# 2013-01-04  1.071818 -2.984555 -2.924354  20   6.0
# 2013-01-05  0.646846 -2.417535 -2.648122  25  10.0
# 2013-01-06 -0.026844 -2.303886 -4.126549  30  15.0

df.apply(lambda x: x.max() - x.min())
# A    2.073961
# B    2.671590
# C    1.785291
# D    0.000000
# F    4.000000
# dtype: float64
```


# Grouping

“group by” we are referring to a process involving one or more of the following steps

* **Splitting** the data into groups based on some criteria
* **Applying** a function to each group independently
* **Combining** the results into a data structure

```python
df = pd.DataFrame({'A' : ['foo', 'bar', 'foo', 'bar',
                          'foo', 'bar', 'foo', 'foo'],
                   'B' : ['one', 'one', 'two', 'three',
                          'two', 'two', 'one', 'three'],
                   'C' : np.random.randn(8),
                   'D' : np.random.randn(8)})                   
#      A      B         C         D
# 0  foo    one -1.202872 -0.055224
# 1  bar    one -1.814470  2.395985
# 2  foo    two  1.018601  1.552825
# 3  bar  three -0.595447  0.166599
# 4  foo    two  1.395433  0.047609
# 5  bar    two -0.392670 -0.136473
# 6  foo    one  0.007207 -0.561757
# 7  foo  three  1.928123 -1.623033


df.groupby('A').sum()
#             C        D
# A                     
# bar -2.802588  2.42611
# foo  3.146492 -0.63958
```

Grouping by multiple columns forms a hierarchical index, which we then apply the function.
```python
df.groupby(['A','B']).sum()
#                   C         D
# A   B                        
# bar one   -1.814470  2.395985
#     three -0.595447  0.166599
#     two   -0.392670 -0.136473
# foo one   -1.195665 -0.616981
#     three  1.928123 -1.623033
#     two    2.414034  1.600434
```
