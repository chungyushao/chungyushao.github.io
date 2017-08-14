---
layout: post
title: "Hackerank -- Almost Sort"
excerpt: ""
categories: dsnalgo
tags: [algorithm, array]
comments: true
share: true
author: chungyu
---

# [Almost sort](https://www.hackerrank.com/challenges/almost-sorted)

> Definitely harder than you think hmm...

```cpp
#include <cmath>
#include <cstdio>
#include <vector>
#include <iostream>
#include <algorithm>
using namespace std;

void revRange(vector<int>& vals, int b, int e) {
    for (int i = 0; i < (e - b + 1) / 2; ++ i)
        std::swap(vals[b + i], vals[e - i]);
}

bool isAsending(const vector<int>& vals) {
    for (int i = 1; i < vals.size(); ++i)
        if (vals[i] < vals[i - 1]) return false;
    return true;
}

int main() {
    int count = 0;
    string buf = "";
    std::getline(std::cin, buf);
    count = std::stoi(buf);
    if (count < 2) {
        cout << "yes\n";
        return 0;
    }
    //
    vector<int> vals(count, 0);
    for (int i = 0; i < count; ++i)
        cin >> vals[i];

    if (isAsending(vals)) {
        cout << "yes\n";
        return 0;
    }    

    int i = 0, j = count - 1;
    for (; i < count && vals[i] < vals[i + 1]; i++) {;} //first wrong
    for (; j > 0 && vals[j] > vals[j - 1]; j--) {;} //last wrong

    std::swap(vals[i], vals[j]);
    if (isAsending(vals)) {
        cout << "yes\nswap " << i + 1 << " " << j + 1 << endl;
        return 0;
    }
    std::swap(vals[i], vals[j]);

    revRange(vals, i, j);

    if (isAsending(vals)) {
        cout << "yes\nreverse " << i + 1 << " " << j + 1 << endl;
        return 0;
    }

    cout << "no\n";
}
```
