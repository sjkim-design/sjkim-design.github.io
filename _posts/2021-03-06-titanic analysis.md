---

layout: post
title:  "캐글 타이타닉 생존자 예측 "
summary: "Titanic"
author: KSJ
date: '2021-03-05 09:41:00 +0900'
categories: analytics
thumbnail: /assets/ship-3401500_1920.png
---

캐글 필사를 시작하면서, 클래식한 Titanic data를 활용하여 DATA HANDLING, EDA, MODELING을 진행해 보려고 합니다.

-----

![타이타닉호](/assets/flying-639522_1920.jpg)





## Contents

1. [데이터 분석 문제 정의](#1.-데이터-분석-문제-정의)

2. [데이터 핸들링](#2.데이터-핸들링)

3. [데이터 EDA](3.-데이터EDA)

4. [모델링](#4.-모델링)

5. [마무리](#5.-마무리)

   ## 1. 데이터 분석 문제 정의

   타이타닉 문제의 목적은 
   타이타닉호에 탑승한 승객들의 데이터를 바탕으로 생존자를 예측하는 문제입니다.     즉 TARGET은 SURVIVED이며, 생존 여부 1,0을 예측하는 것이죠.   

   ```
|    |   PassengerId |   Survived |   Sex |    Age |      Fare |   Pclass_1 |   Pclass_2 |   Pclass_3 |   Family_size |   Title_1 |   Title_2 |   Title_3 |   Title_4 |   Emb_1 |   Emb_2 |   Emb_3 |
|---:|--------------:|-----------:|------:|-------:|----------:|-----------:|-----------:|-----------:|--------------:|----------:|----------:|----------:|----------:|--------:|--------:|--------:|
|  0 |             1 |          0 |     1 | 0.275  | 0.0141511 |          0 |          0 |          1 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
|  1 |             2 |          1 |     0 | 0.475  | 0.139136  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       1 |       0 |       0 |
|  2 |             3 |          1 |     0 | 0.325  | 0.0154686 |          0 |          0 |          1 |           0   |         0 |         0 |         0 |         1 |       0 |       0 |       1 |
|  3 |             4 |          1 |     0 | 0.4375 | 0.103644  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
|  4 |             5 |          0 |     1 | 0.4375 | 0.0157126 |          0 |          0 |          1 |           0   |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
   ```



***

   ## 2. 데이터 핸들링

핸들링에 필요한 파이썬 라이브러리인 `numpy`, `pandas` 등의 기본 라이브러리를 import하고   train/test데이터를 업로드한다.

------

**
**

   {% highlight python %}
   import pandas as pd
   import numpy as np

   train = pd.read_csv('train.csv')
   test = pd.read_csv('test.csv')
   {% endhighlight %}

데이터를 확인하기 위한 head를 실행한다.

{% highlight python %}
train.head()
{% endhighlight %}  

***

```
|    |   PassengerId |   Survived |   Sex |    Age |      Fare |   Pclass_1 |   Pclass_2 |   Pclass_3 |   Family_size |   Title_1 |   Title_2 |   Title_3 |   Title_4 |   Emb_1 |   Emb_2 |   Emb_3 | |---:|--------------:|-----------:|------:|-------:|----------:|-----------:|-----------:|-----------:|--------------:|----------:|----------:|----------:|----------:|--------:|--------:|--------:| |  0 |             1 |          0 |     1 | 0.275  | 0.0141511 |          0 |          0 |          1 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 | |  1 |             2 |          1 |     0 | 0.475  | 0.139136  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       1 |       0 |       0 | |  2 |             3 |          1 |     0 | 0.325  | 0.0154686 |          0 |          0 |          1 |           0   |         0 |         0 |         0 |         1 |       0 |       0 |       1 | |  3 |             4 |          1 |     0 | 0.4375 | 0.103644  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 | |  4 |             5 |          0 |     1 | 0.4375 | 0.0157126 |          0 |          0 |          1 |           0   |         1 |         0 |         0 |         0 |       0 |       0 |       1 |

```



```
|    |   PassengerId |   Survived |   Sex |    Age |      Fare |   Pclass_1 |   Pclass_2 |   Pclass_3 |   Family_size |   Title_1 |   Title_2 |   Title_3 |   Title_4 |   Emb_1 |   Emb_2 |   Emb_3 |
|---:|--------------:|-----------:|------:|-------:|----------:|-----------:|-----------:|-----------:|--------------:|----------:|----------:|----------:|----------:|--------:|--------:|--------:|
|  0 |             1 |          0 |     1 | 0.275  | 0.0141511 |          0 |          0 |          1 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
|  1 |             2 |          1 |     0 | 0.475  | 0.139136  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       1 |       0 |       0 |
|  2 |             3 |          1 |     0 | 0.325  | 0.0154686 |          0 |          0 |          1 |           0   |         0 |         0 |         0 |         1 |       0 |       0 |       1 |
|  3 |             4 |          1 |     0 | 0.4375 | 0.103644  |          1 |          0 |          0 |           0.1 |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
|  4 |             5 |          0 |     1 | 0.4375 | 0.0157126 |          0 |          0 |          1 |           0   |         1 |         0 |         0 |         0 |       0 |       0 |       1 |
```



***





