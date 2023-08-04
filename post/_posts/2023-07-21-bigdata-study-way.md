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

![](/assets/img/blog/bigdata/task1_list.png)

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

문제는 크게 **"값"**을 구하는 문제, **"분류"**를 하는 문제로 나뉜다. 

문제에서 **~의 값을 예측하시오. 하면 "Regressor" 모델**을 사용하면 된다.
> EX. 자동차 보험비를 예측하시오, 점수를 예측하시오 등

문제에서 **~분류하시오, 여부를 구하시오 하면 "Classifier" 모델**을 사용하면 된다. 
> EX. 남여 성별 예측하기, 참여할 확률을 구하기 등

가끔 문제에서 확률을 구하라고 할 때도 있는데 이는 "Classifier" 모델을 사용하여 그 분류값이 나올 확률을 구하면 된다. 

나는 책을 통해 문제를 푸는 방법에 대해 익혔고, 자료방에 올라온 문제를 통해 연습을 했다. 

- [2023 빅데이터 분석 기사 실기 (필답형+작업형)](https://www.aladin.co.kr/shop/wproduct.aspx?ItemId=283045706)

<img src="https://image.aladin.co.kr/product/28304/57/cover500/8965403197_2.jpg" height="100"/>

- [빅분기 자료방 바로가기](https://github.com/lovedlim/BigDataCertificationCourses)

![](/assets/img/blog/bigdata/task2_list.png)

문제를 풀면서, 단계를 익혀가고 각 단계에서 필요로하는 함수 및 코딩 능력을 연습하면 된다. 

작업형2는 크게 다음 4단계를 따른다. 

## 1. 데이터 탐색 
이 단계에서는 데이터 파일을 읽어 **데이터의 정보를 확인**하면 된다. 
행열은 어떻게 구성되어 있는지, 범주형 변수는 몇개인지 등 문제를 풀기위해 데이터에 대한 탐색을 하면 된다. 

<details>
<summary>예제</summary>
<div markdown="1">

### 예제 문제 
문제 : 고객 891명에 대한 학습용 데이터를 이용하여 생존여부를 예측하는 모형만들기 

이를 평가용 데이터에 저장하여 승객의 생존 여부 예측값을 다음과 같은 형식의 csv 파일로 생성하기

모델의 성능은 ROC-AUC 평가지표에 따라 매겨짐

제출 형식
> PassengerId, Servivied
892, 0
893, 1
   

### 1. 데이터 가져오기
   ```python
   
   import pandas as pd

   # 문제에서 제시해줌
   print("데이터 가져오기") 
   x_train = pd.read_csv('./bigData-main/x_train.csv', encoding='CP949')
   x_test = pd.read_csv('./bigData-main/x_test.csv', encoding='CP949')
   y_train = pd.read_csv('./bigData-main/y_train.csv', encoding='CP949')
   print(x_train.head())
   print(x_test.head())
   print(y_train.head())
   ```

### 2. 행과 열 확인하기
   ```python
   
   print("\n행과 열을 바꾸어 보기")
   print(x_train.head().T)
   print(x_test.head().T)
   print(y_train.head().T)

   print("\n행열 확인하기")
   print(x_train.shape)
   print(x_test.shape)
   print(y_train.shape)
   ```

### 3. 요약정보 및 기초통계량 확인
   ```python
   
   print("\n요약정보 확인")
   print(x_train.info())

   # 5. 기초통계량 확인하기 
   print("\n기초통계량 확인하기")
   print(x_train.describe().T)
   ```

</div>
</details>

## 2. 데이터 전처리
데이터 전처리 단계에서는 필요없는 데이터를 삭제하거나, 이상치나 결측치를 채우는 등의 **데이터 변환작업**을 하면 된다. 

1. 모델링에 필요하지 않은 불필요한 칼럼을 삭제.
   ex. id, 이름 등
2. 결측치 채우기 
3. 이상치 제거 
   ex. age정보 인데 값이 마이너스인 것들.
4. 범주형 변수 인코딩 작업 - 명목형변수(숫자 외의 값)가 있으면 값으로 대체하기. 
   ex. 남 -> 0, 여 -> 1 
5. 스케일링 작업 - 굳이 하지 않아도 되긴하나, 표준화 크기로 변환 후의 모델의 성능이 좋을 때가 있었다. 

<details>
<summary>예제</summary>
<div markdown="1">

### 1. 불필요한 컬럼 삭제 
   ```python
   # 테스트 데이터인 x_test도 값을 예측하는 과정에 사용하므로 전처리 필요

   # cust_id 칼럼은 종속병수인 성별을 예측하는 정보가 아니라 key 역할이므로 삭제
   # 단, 최종 제출에는 사용되는 컬럼이므로 따로 저장
   x_test_cust_id = x_test['cust_id']

   # cust_id 삭제
   x_train = x_train.drop(columns = ['cust_id'])
   x_test = x_test.drop(columns = ['cust_id'])
   y_train = y_train.drop(columns = ['cust_id'])

   # 컬럼이 삭제된 상위 5개 행 확인
   print(x_train.head())
   print(x_test.head())
   print(y_train.head())
   ```

### 2. 결측치 처리
   ```python
   # 환불금액의 칼람에 2,295건의 결측치 존재
   print(x_train.isnull().sum())
   # 이는 환불금액 결측치는 이력이 없는 경우에 발생할 것으로 예상할 수 있음 -> 0으로 대체
   x_train['환불금액'] = x_train['환불금액'].fillna(0)
   x_test['환불금액'] = x_test['환불금액'].fillna(0)
   # 결측치가 조치되었는지 확인
   print(x_train['환불금액'].isnull().sum())
   print(x_test['환불금액'].isnull().sum())
   ```
### 3. 이상치가 있는경우, 제거 또는 변경 작업 진행 

### 4. 범주형 변수를 인코딩하기 
   ```python
   # 주구매상품 칼럼에서 중복을 제외한 값들을 확인
   print(x_train['주구매상품'].unique())
   # 주구매지점 칼럼에서 중복을 제외한 값들을 확인
   print(x_train['주구매지점'].unique())
   # 주구매지점 칼럼에서 중복을 제외한 값들의 개수 세기 
   print(x_train['주구매지점'].unique().size)
   # 인코딩할 수가 많을 경우, 라벨 인코딩을 하는 것이 효과적임

   from sklearn.preprocessing import LabelEncoder 
   encoder = LabelEncoder()
   # 주구매상품에 대해 라벨 인코딩을 수행하고, 주구매상품 칼람으로 다시 저장
   x_train['주구매상품'] = encoder.fit_transform(x_train['주구매상품'])
   # 라벨 인코딩 결과를 호가인하기 위해, 상위 10개 행을 확인
   print(x_train['주구매상품'].head(10))
   # 주구매상품 컬럼에 대한 라벨 인코딩의 변환 순서 확인
   print(encoder.classes_)
   # 테스트 데이터도 라벨 인코딩 수행
   x_test['주구매상품'] = encoder.fit_transform(x_test['주구매상품'])

   # 주구매지점 라벨 인코딩 수행 
   x_train['주구매지점'] = encoder.fit_transform(x_train['주구매지점'])
   # 라벨 인코딩 결과를 호가인하기 위해, 상위 10개 행을 확인
   print(x_train['주구매지점'].head(10))
   # 주구매지점 컬럼에 대한 라벨 인코딩의 변환 순서 확인
   print(encoder.classes_)
   # 테스트 데이터도 라벨 인코딩 수행
   x_test['주구매지점'] = encoder.fit_transform(x_test['주구매지점'])
   ```

### 5. 표준화 크기로 변환 
   ```python
   # 크기변환 전, x_train 세트의 기초 통계량 확인
   print(x_train.describe().T)
   from sklearn.preprocessing import StandardScaler
   scaler = StandardScaler()
   # 표준크기변환 수행 후 x_train 칼럼명 그래도 사용
   x_train = pd.DataFrame(scaler.fit_transform(x_train), columns=x_train.columns)
   # 테스트 데이터도 크기변환 
   x_test = pd.DataFrame(scaler.fit_transform(x_test), columns=x_test.columns)
   # 크기변환 후, x_train 세트의 기초 통계량 확인
   print(x_train.describe().T)
   ```

</div>
</details>

## 3. 학습 및 평가  
**모델을 만들어 학습**하고 train 데이터를 분리하여, **모델의 성능 확인**을 하는 단계이다. 
하이퍼파라미터 튜닝 작업을 해도 되긴하나, 시간문제상 거의 어렵다고 보면된다. 

1. 주어진 train 데이터를 Test, Train로 split
2. 모델 만들기 > 나는 XGB와 RandomForest 모델을 만들었다. 
3. 1번 스텝에서 쪼갰던 데이터를 사용하여 문제에서 제시한 평가 지표로 2개의 모델 성능을 비교
4. 3번 단계에서 성능이 더 높은 모델을 선정 후 결과값 도출

<details>
<summary>예제</summary>
<div markdown="1">

### 1. 데이터 분리하기 
   ```python
   from sklearn.model_selection import train_test_split

   X_TRAIN, X_TEST, Y_TRIAN, Y_TEST = train_test_split(x_train, y_train, test_size=0.2)
   # 분리된 데이터의 행렬 구조 확인
   print(X_TRAIN.shape)
   print(X_TEST.shape)
   print(Y_TRIAN.shape)
   print(Y_TEST.shape)
   ```

### 2. 데이터 학습 및 모델 생성 
   ```python
   # XGBClassifer : 일반적으로 성능이 잘나옴 
   from xgboost import XGBClassifier 
   from sklearn.ensemble import RandomForestClassifier
   # XGB
   model1 = XGBClassifier()
   model1.fit(X_TRAIN, Y_TRIAN)

   # RandomForest
   model2 = RandomForestClassifier()
   model2.fit(X_TRAIN, Y_TRIAN)
   ```

### 3. 결과 예측 및 모델 평가 
   ```python
   # 승객이 사망할 확률 : pd.DataFrame(model.predict_proba(x_test))[0]
   # 승객이 생존할 확률 : pd.DataFrame(model.predict_proba(x_test))[1]

   # 학습이 완료된 모델을 통해 Y_TEST 예측. 평가지표 계산용
   Y_TEST_PREDICT1 = pd.DataFrame(model1.predict(X_TEST))
   Y_TEST_PREDICT2 = pd.DataFrame(model2.predict(X_TEST))

   # 평가 비교
   from sklearn.metrics import roc_auc_score
   # model1
   print(roc_auc_score(Y_TEST, Y_TEST_PREDICT1))
   # model2
   print(roc_auc_score(Y_TEST, Y_TEST_PREDICT2))
   ```

### 4. 모델 선정 
   ```python
   y_test_predict = pd.DataFrame(model2.predict(x_test)).rename(columns={0:'Survived'})
   print(pd.DataFrame(y_test_predict).head())
   ```

</div>
</details>

## 4. 제출
마지막으로 문제에서 제시한 형태로 DataFrame을 만들어 **csv로 제출**하면 된다. 

index의 경우 default 값이 true이므로 Fasle로 지정해줘야한다. 
문제에서 index를 필요로 한다면 지정할필요 없지만, 보통 index는 Fasle이다. 

그리고 문제에서 csv 파일명을 알려주니, 제대로 확인해서 감점받는 일은 없게 해야한다. 

<details>
<summary>예제</summary>
<div markdown="1">

   ```python
      print(pd.concat([x_test_passenger_id, y_test_predict], axis=1))

      final = pd.concat([x_test_passenger_id, y_test_predict], axis=1)

      final.to_csv('data/result.csv', index=False)
   ```

</div>
</details>

# 작업형 3
작업형 3은 기출 자료가 아예 없다. 그래서 자료방에 올라온 내용을 토대로 정리하면서 공부했다. 
- [빅분기 자료방 바로가기](https://github.com/lovedlim/BigDataCertificationCourses)

아래는 내가 작업형 3을 대비하여 정리한 내용이다. 이제 6회 기출이 생겼으니, 7회 준비자들은 기출을 토대로 준비하면 좋을 듯 하다!
- [정리 내용](https://kongsubin.github.io/post/study/2023-08-03-bigdata-task3/)