---

layout: post
title:  "캐글 타이타닉 생존자 예측 "
summary: "Titanic"
author: KSJ
date: '2021-03-05 09:41:00 +0900'
categories: analytics
thumbnail: /imgages/ship-3401500_1920.png
---

캐글 필사를 시작하면서, 클래식한 Titanic data를 활용하여 DATA HANDLING, EDA, MODELING을 진행해 보려고 합니다.

-----







## Contents

1. [데이터 분석 문제 정의](#1-데이터-분석-문제-정의)

2. [데이터 핸들링](#2.-핸들링)

3. [데이터 EDA](3.-데이터EDA)

4. [모델링](#4.-모델링)

5. [마무리](#5.-마무리)

   -----

   ## 1. 데이터 분석 문제 정의


   타이타닉 문제의 목적은 
   타이타닉호에 탑승한 승객들의 데이터를 바탕으로 생존자를 예측하는 문제입니다.   
   즉 TARGET은 SURVIVED이며, 생존 여부 1,0을 예측하는 것이죠.   

   

   ***

   ## 2. 데이터 핸들링

핸들링에 필요한 파이썬 라이브러리인 `numpy`, `pandas` 등의 기본 라이브러리를 import하고   train/test데이터를 업로드한다.

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

| PassengerId | Survived | Pclass | Name | Sex | Age | SibSp | Parch | Ticket | Fare | Cabin | Embarked |  |
|-|-|-|-|-|-|-|-|-|-|-|-|-|
| 0 | 1 | 0 | 3 | Braund, Mr. Owen Harris | male | 22.0 | 1 | 0 | A/5 21171 | 7.2500 | NaN | S |
| 1 | 2 | 1 | 1 | Cumings, Mrs. John Bradley (Florence Briggs Th... | female | 38.0 | 1 | 0 | PC 17599 | 71.2833 | C85 | C |
| 2 | 3 | 1 | 3 | Heikkinen, Miss. Laina | female | 26.0 | 0 | 0 | STON/O2. 3101282 | 7.9250 | NaN | S |
| 3 | 4 | 1 | 1 | Futrelle, Mrs. Jacques Heath (Lily May Peel) | female | 35.0 | 1 | 0 | 113803 | 53.1000 | C123 | S |
| 4 | 5 | 0 | 3 | Allen, Mr. William Henry | male | 35.0 | 0 | 0 | 373450 | 8.0500 | NaN | S |




