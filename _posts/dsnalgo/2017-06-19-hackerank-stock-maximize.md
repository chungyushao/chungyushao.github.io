---
layout: post
title: "Hackerank -- Stock Maximize"
excerpt: ""
categories: dsnalgo
tags: [algorithm, array]
comments: true
share: true
author: chungyu
---

# [Stock Maximize](https://www.hackerrank.com/challenges/stockmax)

### Input Format

* The first line contains the number of test cases T. T test cases follow:
* The first line of each test case contains a number N. The next line contains N integers, denoting the predicted price of WOT shares for the next N days.

### Constraints

* All share prices are between 1 and 100000

### Output Format

* Output T lines, containing the maximum profit which can be obtained for the corresponding test case.

### Sample Input

```
3
3
5 3 2
3
1 2 100
4
1 3 1 2
```

### Sample Output

```
0
197
3
```

### Explanation
* For the first case, you cannot obtain any profit because the share price never rises.
* For the second case, you can buy one share on the first two days, and sell both of them on the third day.
* For the third case, you can buy one share on day 1, sell one on day 2, buy one share on day 3, and sell one share on day 4.


# Solution

```cpp
using namespace std;

long long solver(vector<int>&& prices)
{
    if (prices.empty()) return 0;
    int pMax = prices[prices.size() - 1];
    long long earn = 0;
    for (int i = prices.size() - 2; i >= 0; --i)
    {
        if (prices[i] > pMax) pMax = prices[i];
        else earn += (pMax - prices[i]);
    }
    return earn;
}

vector<int> get_input()
{
    int numDay;
    vector<int> prices;
    cin >> numDay;
    for (int j = 0; j < numDay; ++j)
    {
        int p;
        cin >> p;
        prices.push_back(p);            
    }
    return prices;
}

int main()
{
    int numCase;
    cin >> numCase;
    for (int i = 0; i < numCase; ++i)
        cout << solver(std::move(get_input())) << endl;
    return 0;
}

```
