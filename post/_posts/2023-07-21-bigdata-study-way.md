---
layout: post
title: 빅데이터분석기사 실기 공부 방법 
sitemap: false
categories: [study]
tags: [info]
description: >
  빅데이터분석기사 비전공자 실기 공부 방법~!
---

# 빅데이터분석기사 실기 공부법

# 작업형 1
작업형 1은 자료방에 올라온 문제들을 반복 풀면서 공부했다. 

- [빅분기 자료방 바로가기](https://github.com/lovedlim/BigDataCertificationCourses)
  ![](assets/img/blog/bigdata/task1_list.png)

문제를 풀어서 답을 구한다기 보다는, 

iloc사용법, sum과 len 차이점, merge 등 pandas를 활용하는 코딩연습을 하는 느낌으로 풀었다. 

손에 잘 안익는 문제들을 반복해서 풀었고, 반복해서 풀다보니 자연스럽게 함수 사용법이 익혀졌다!

잘 안익혀지거나, 헷갈리는 부분들은 따로 정리하면서 공부했다. 

<details>
<summary>정리한 내용 중 일부</summary>
<div markdown="1">

### 조건에 맞는 데이터 갯수

```python
# true를 세는 방식 (이상치, nan체크할때 주로 사용)
sum(data['sex'] == 'F')

# 조건에 맞는 data만 남겨서 구하기 
len(data[[data['sex'] == 'F']])
```

### merge

```python
import pandas as pd
data1 = pd.read_csv('bigData-main/basic1.csv')
data3 = pd.read_csv('bigData-main/basic3.csv')

# basic1 데이터와 basic3 데이터를 'f4'값을 기준으로 병합
data = pd.merge(left = data1 , right = data3, how = "left", on = "f4")
```

### 분할 (동일한 개수로 나이 순으로 3그룹으로 나누기)

```python
# 분할 기준 보기 
print(pd.qcut(data_new['age'], q=3))

# 구간 분할 
data_new['range'] = pd.qcut(data_new['age'], q=3, labels=['g1', 'g2', 'g3'])
print(data_new)

# 수량 비교 
print(data_new['range'].value_counts())
```

</div>
</details>




# 작업형 2
작업형2는 최소 35점 이상을 받아갈 정도로 연습해야한다. 사실 40점 만점을 받는게 가장 좋지만 모델의 성능이 너무 안좋으면 5점이 감점이 될 수도 있다...

문제는 크게 "값"을 구하는 문제, "분류"를 하는 문제로 나뉜다. 

문제에서 ~의 값을 예측하시오. 하면 "Regressor" 모델을 사용하면 된다.
> EX. 자동차 보험비를 예측하시오, 점수를 예측하시오 등

문제에서 ~분류하시오, 여부를 구하시오 하면 "Classifier"모델을 사용하면 된다. 
> EX. 남여 성별 예측하기, 참여할 확률을 구하기 등

가끔 문제에서 확률을 구하라고 할 때도 있는데 이는 "Classifier"모델을 사용하여 그 분류값이 나올 확률을 구하면 된다. 

나는 책을 통해 문제를 푸는 방법에 대해 익혔고, 자료방에 올라온 문제를 통해 연습을 했다. 

- [2023 빅데이터 분석 기사 실기 (필답형+작업형)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=283045706)
- [빅분기 자료방 바로가기](https://github.com/lovedlim/BigDataCertificationCourses)

문제를 풀면서, 단계를 익혀가고 각 단계에서 필요로하는 함수 및 코딩 능력을 연습하면 된다. 

작업형2는 크게 다음 4단계를 따른다. 

### 1. 데이터 탐색 
이 단계에서는 데이터 파일을 읽어 데이터의 정보를 확인하면 된다. 
행열은 어떻게 구성되어 있는지, 범주형 변수는 몇개인지 등 문제를 풀기위해 데이터에 대한 탐색을 하면 된다. 

### 2. 데이터 전처리
데이터 전처리 단계에서는 필요없는 데이터를 삭제하거나, 이상치나 결측치를 채우는 등의 데이터 변환작업을 하면 된다. 

1. 모델링에 필요하지 않은 불필요한 칼럼을 삭제.
   ex. id, 이름 등
2. 결측치 채우기 
3. 이상치 제거 
   ex. age정보 인데 값이 마이너스인 것들.
4. 범주형 변수 인코딩 작업 - 명목형변수(숫자 외의 값)가 있으면 값으로 대체하기. 
   ex. 남 -> 0, 여 -> 1 
5. 스케일링 작업 - 굳이 하지 않아도 되긴하나, 표준화 크기로 변환 후의 모델의 성능이 좋을 때가 있었다. 


### 3. 학습 및 평가  
모델을 만들고 train 데이터를 분리하여, 모델의 성능 확인을 하는 단계이다. 
하이퍼파라미터 튜닝 작업을 해도 되긴하나, 시간문제상 거의 어렵다고 보면된다. 

1. 주어진 train 데이터를 Test, Train로 split
2. 모델 만들기 > 나는 XGB와 RandomForest 모델을 만들었다. 
3. 1번 스텝에서 쪼갰던 데이터를 사용하여 문제에서 제시한 평가 지표로 2개의 모델 성능을 비교
4. 3번 단계에서 성능이 더 높은 모델을 선정 후 결과값 도출

### 4. 제출
마지막으로 문제에서 제시한 형태로 DataFrame을 만들어 csv로 제출하면 된다. 

