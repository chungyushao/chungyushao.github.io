---
layout: post
title: "Elementary Finance Tools"
excerpt: "Compound"
categories: finance
tags: [finance]
comments: true
share: true
author: chungyu
---
> mainly from
> * [Coursera](https://www.coursera.org/learn/global-financial-markets-instruments/home/welcome)

# Time value of money

### What is time value of money?
* A dollar today is worth more than a dollar tomorrow.

### What is the future value of a cash flow today?
* Future value of a cash flow today is the value of the funds invested at your opportunity cost

### What is compounding?
> If you invest $5000 in a CD with an interest rate of 8% per year, how much will you have
at the end of 5 years?

* `T` = numbers of periods to maturity
* `r` = the interest rate
* \\[ P_T = P_{0}(1 + r)^{T} \\]
* \\[ P_5 = 5000(1 + 0.08)^{5} = 7346.64 \\]

### What is the present value of a cash flow in the future?
* Present value of a future cash flow is the amount of cash you would take today instead of the promised future cash flow.

> What is the present value of $100,000 paid at the end of ten years, if your opportunity
cost is 5%?

* \\[ P_0 = P_{T}(1 + r)^{-T} \\]
* \\[ P_0 = 100000(1 + 0.005)^{-10} = 61391.33 \\]

# Valuing streams of cash flows

### What is an annuity?
* An annuity is a series of equal fixed payments for a specified number of
periods.
* Annuity Compound Factor, `ACF(r,n)`, sums up the compounding factors
for `n` payments at a constant interest rate `r`
* \\[ ACF(r, n) = \frac{(1 + r)^n - 1}{r} \\]

### What is the present value of a stream of cash flows?
* `T`: number of annuity payments
* `r`: the interest rate
* present value of annuity
  * \\[ P_0 = \frac{A}{r}(1 - (1 + r)^{-T}) \\]
* OR
* \\[ V_0 = C * ADF(r, n) \\]
* \\[ ADF(r, n) = \frac{1 - (1 + r)^ {-n}}{r} \\]

> Which one would you prefer?
* a. Receive $10,000 now
* b. Receive $1,000 every year for 13 years (the last payment occurs at the end of 13
years), if the annual interest rate is 4%

* choose (a): \\[ P_0 = \frac{1000}{0.04}(1 - (1 + 0.04)^{-13}) = 9985.54 < 10000\\]

### What is the future value of a stream of cash flows?
* Future value of the annuity
  * \\[ V_T = C * ACF(r, n) \\]

> How much would you have saved in twenty years if you save $5000 every year and can
guarantee earning 6% per year?

* \\[ V_T = C x ACF(r, n) = 5000 x ACF(0.06, 20) = 5000 * \frac{(1 + 0.06)^n - 1}{0.06} ~= 183927\\]

### Car loan

> You are buying a new car. The car dealer gives you three financing options. If your
objective is to minimize the present value of your car payments and your opportunity cost
of capital is 0.5% per month, which one would you choose? $500 per month for 36 months or $600 per month for 24 months?

* \\[ V_0 = C * ADF(r, n) \\]
  * \\[ V_0 = 500 * ADF(0.005, 36) = 16435 \\]
  * \\[ V_0 = 600 * ADF(0.005, 24) = 13537 \\]

### House montage

> You are buying a new house for $450,000. Reviewing different financing options, you
have determined that you would like to minimize your monthly payment. Which
financing option would you choose? a. 30-year mortgage with annual interest rate of 3.5 percent or b. 20-year mortgage with an annual interest rate of 3 percent?

* The payments are monthly and the interest rate is annual,
  * \\[ r_a = \frac{3.5}{12} = 0.2917% \\]
  * \\[ n_a = 12*30 = 360\\]
  * \\[ r_b = \frac{3}{12} = 0.25% \\]
  * \\[ n_b = 12*20 = 240\\]
* \\[ V_0 = C * ADF(r, n) \\]
  * \\[ C_a = 45000/ADF(0.00297, 360) = 2020.70 \\]
  * \\[ C_b = 45000/ADF(0.0025, 240) = 2495.69 \\]
