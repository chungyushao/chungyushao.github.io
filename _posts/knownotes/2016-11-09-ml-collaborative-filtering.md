---
layout: post
title: "Collaborative Filtering"
excerpt: "recommendation system, collaborative filtering"
categories: knownotes
tags: [ML]
comments: true
share: true
author: chungyu
---

> Mainly from
* lecture notes of [ Practical Data Analysis](http://www.datasciencecourse.org/)
* [Collaborative Filtering for Implicit Feedback Datasets](http://yifanhu.net/PUB/cf.pdf)


![pdscf1]({{site.url}}/images/ml/pdscf1.png)
![pdscf2]({{site.url}}/images/ml/pdscf2.png)
![pdscf3]({{site.url}}/images/ml/pdscf3.png)


# Matrix factorization approaches

## Variables
* \\( m \\)
  * number of users
* \\( n \\)
  * number of items
* \\( f \\)
  * feature vector dimension
  * Typical lie between 20 and 200
* \\( r_{ui} \\)
  * Observations, For explicit feedback datasets
  * Those values would be ratings that indicate the preference by user \\( u \\) of item \\( i \\), where high values mean stronger preference.
  * For implicit feedback datasets, those values would indicate observations for user actions.
* \\( x_{u} \in \mathbb{R}^f \\)
  * user-factors vector
  * \\( X \\) will be \\( m\ ×\  f \\) matrix
* \\( y_{i} \in \mathbb{R}^f \\)
  * item-factors vector
  * \\( Y \\) will be \\( n\ ×\  f \\) matrix
* \\( \hat{r_{ui}} = x_{u}^Ty_{i} \\)  
  * prediction
* \\( p_{ui} = x_{u}^Ty_{i} \\)
  * Indicates the preference of user \\( u \\) to item \\( i \\).
  * The vectors strive to map users and items into a common latent factor space where they can be directly compared.
  * \\( p_{ui} = 1\ \textit{if}\ r_{ui} > 0 \\)
  * \\( p_{ui} = 0\ \textit{if}\ r_{ui} = 0 \\)
* \\( c_{ui} = 1 + \alpha r_{ui} \\)
  * measure our confidence in observing \\( p_{ui} \\).
* \\( C^{u} \\)
  * For each user \\( u \\), let us define the
  * \\( C_{ii}^u = c_{ui} \\)
* \\( n_u \\)
  * the number of items for which \\( r_{ui} > 0 \\) for user \\( u \\)
  * typically \\( n_u \ll n \\)  
* \\( \lambda \\)
  * regularizing parameters

## Cost Function and Optimization
> alternating-least-squares optimization process
![cf1]({{site.url}}/images/ml/cf1.png)
![cf2]({{site.url}}/images/ml/cf2.png)
![cf3]({{site.url}}/images/ml/cf3.png)

* A recomputation of the user-factors is followed by a recomputation of all item-factors in a parallel fashion.
* The whole process scales **linearly** with the size of the data.

###### Recomputing all user factors
> ![cf2]({{site.url}}/images/ml/cf2.png)

1. compute the \\( f\ ×\  f \\)  matrix \\( Y^TY \\)  in \\( O(f^2n) \\)
2. For each user \\( u \\), define:
  * \\( n\ ×\  n \\) diagonal matrix \\( C^{u} \\)
  * \\( p(u) \in \mathbb{R}^n \\) that contains all the preferences by \\( u \\) (the \\( p_{ui} \\) values)
3. Trick: \\( Y^TC^uY = Y^TY + Y^T\(C^u - I\)Y \\)

###### Recomputing all item factors
>![cf3]({{site.url}}/images/ml/cf3.png)

1. compute the \\( f\ ×\  f \\)  matrix \\( X^TX \\)  in \\( O(f^2m) \\)
2. For each item \\( i \\), define:
  * \\( m\ ×\  m \\) diagonal matrix \\( C^{i} \\)
  * \\( p(i) \in \mathbb{R}^m \\) that contains all the preferences for \\( i \\) (the \\( p_{ui} \\) values)
3. Same trick can be applid: \\( X^TC^iX = X^TX + X^T\(C^i - I\)X \\)
