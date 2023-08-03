---
layout: post
title: 빅데이터분석기사 실기 작업형3 정리 
sitemap: false
categories: [study]
tags: [info]
description: >
  빅데이터분석기사 실기 작업형3 정리 ~!
---
# 빅데이터분석기사 실기 작업형3 정리


## T - 검정 표본
> **** P VALUE가 작으면 귀무가설 기각~


### 단일표본 (ttest_1samp)

- 표본집단의 평균으로 모집단의 평균을 검정
    - 귀무가설 : 모평균과 표본평균 같음
    - 대립가설 : 모평균과 표본평균 다름
    
    ```python
    # 자료가 수집된 지역의 평균 온도는 20도라고 한다. 
    # 수집된 데이터를 사용하여 양측 검정을 실시했을 때 p-value
    from scipy.stats import ttest_1samp
    
    # 데이터
    scores = [75, 80, 68, 72, 77, 82, 81, 79, 70, 74, 76, 78, 81, 73, 81, 78, 75, 72, 74, 79, 78, 79]
    
    # 모평균 가설검정
    mu = 75  # 검정할 모평균
    alpha = 0.05  # 유의수준
    
    # t-test를 사용하여 가설 검정
    t_statistic, p_value = ttest_1samp(scores, popmean = mu, alternative='greater')
    
    # 결과 출력
    print("t-statistic:", t_statistic)
    print("p-value:", p_value)
    
    if p_value < alpha:
        print("귀무가설을 기각합니다. 모평균은 75보다 큽니다.")
    else:
        print("귀무가설을 채택합니다. 모평균은 75보다 크지 않습니다.")
    ```
    

### 독립표본 (ttest_ind)

- 서로 다른 두 집단의 평균 차이를 검정
    - 귀무가설 : 두 집단의 평균에 차이가 없다
    - 대립가설 : 두 집단의 평균에 차이가 있다
    
    ```python
    # 약물을 복용한 그룹과 복용하지 않은 그룹의 평균 체온의 유의미한 차이 여부 
    
    from scipy import stats
    
    # 가설 설정
    # H0: 약물을 복용한 그룹과 복용하지 않은 그룹의 평균 체온은 유의미한 차이가 없다.
    # H1: 약물을 복용한 그룹과 복용하지 않은 그룹의 평균 체온은 유의미한 차이가 있다.
    
    # 데이터 수집
    group1 = [36.8, 36.7, 37.1, 36.9, 37.2, 36.8, 36.9, 37.1, 36.7, 37.1]
    group2 = [36.5, 36.6, 36.3, 36.6, 36.9, 36.7, 36.7, 36.8, 36.5, 36.7]
    
    # 가설검정
    t_statistic, p_value = stats.ttest_ind(group1, group2)
    
    print("검정통계량:", t_statistic)
    print("p-value:", p_value)
    
    # 유의성 검정
    alpha = 0.05  # 유의수준 설정
    
    if p_value < alpha:
        print("귀무가설을 기각합니다. 약물을 복용한 그룹과 복용하지 않은 그룹의 평균 체온은 유의미한 차이가 있습니다.")
    else:
        print("귀무가설을 채택합니다. 약물을 복용한 그룹과 복용하지 않은 그룹의 평균 체온은 유의미한 차이가 없습니다.")
    ```
    

### 대응표본 (ttest_rel)

- 동일한 집단의 사전/사후 평균 차이를 검정
    - 귀무가설 : μ >= 0 (사전사후 평균 같음)
    - 대립가설 : μ < 0  (사전사후 평균 같지않음)
    
    ```python
    # 고혈압 환자 치료 전후의 혈압
    import pandas as pd
    from scipy import stats
    df = pd.read_csv('bigData-main/high_blood_pressure.csv')
    print(df.head())
    
    df['diff'] = df['bp_post'] - df['bp_pre']
    
    #1
    print(round(df['diff'].mean(),2))
    
    #2 
    # st - 검정통계량, pv - p value(작아야 채택))
    # after값, before값 
    st, pv = stats.ttest_rel(df['bp_post'], df['bp_pre'], alternative="less")
    print(round(st,4))
    
    #3
    print(round(pv,4))
    
    #4 귀무가설 기각, 대립가설 채택 (0.0016 < 0.05)
    ```
    
### alternative flag값 설명 

```python
* 'two-sided': 표본의 기본 분포의 평균 주어진 모집단 평균(`popmean`)과 다릅니다. 
* 'less': 샘플의 기본 분포의 평균은 다음과 같습니다. 주어진 모집단 평균(`popmean`)보다 작음 (before값이 더 작음)
* 'greater': 샘플의 기본 분포의 평균은 다음과 같습니다. 주어진 모집단 평균(`popmean`)보다 큼
```

## 분산분석(ANOVA)

- 둘 이상의 집단 간 평균의차이를 분석
- F-test 검정 통계량 : 집단감분산 / 집단내분산

### One Way Anova (일원분산분석 = 일원배치법)

- 범주형 변수가 한개인경우
    - 귀무가설 : 모든 집단의 평균은 같다
    - 대립가설 : 적어도 한 집단의 평균은 다르다
    - F값이 충분히 클 경우 귀무가설을 기각
        - 공장 라이별 불량률은 동일하다.
    
    ```python
    # 귀무가설(H0): 세 그룹(A, B, C) 간의 평균 성적 차이가 없다.
    # 대립가설(H1 또는 Ha): 세 그룹(A, B, C) 간의 평균 성적 차이가 있다.
    
    # 각 그룹의 데이터
    groupA = [85, 92, 78, 88, 83, 90, 76, 84, 92, 87]
    groupB = [79, 69, 84, 78, 79, 83, 79, 81, 86, 88]
    groupC = [75, 68, 74, 65, 77, 72, 70, 73, 78, 75]
    
    import scipy.stats as stats
    
    # 일원배치법 수행
    f_value, p_value = stats.f_oneway(groupA, groupB, groupC)
    
    # F-value
    print(round(f_value,2))
    
    # p-value
    print(format(p_value,'.6f'))
    ```
    

### 이원산분석

- 범주형 변수가 두개 이상인 경우
    - 주효과 가설
    - 변수간 상호작용 가설
        - 귀무가설 : 두 변수간 상호작용이 없다
        - 대립가설 : 두 변수간 상호작용이 있다
            - 광고별 모델 선호도 및 만족도에 차이가 있는가?


## 독립성검정(카이제곱)

### 적합성 검정

- 범주는 상호배타적이어야한다.
    - 귀무가설 : 분포가 기대 분포와 동일하다
    - 대립가설 : 분포가 기대 분포와 같지 않다.
        - 3개의 서로 다른 공장 라인에서 불량품이 나오는 정도는 1:2:3이다
    
    ```python
    xo, xe = [26,26,26,], [23,18,37,]
    
    from scipy.stats import chisquare
    
    chir_square, p_value = chisquare(xo, xe) 
    ```
    

### 동질성 검정

- 두집단의 분포가 동일한지 검정
    - 귀무가설 : 두 집단의 분포는 같다
    - 대립가설 : 두 집단의 분포는 같지 않다.
        - 성별에 따라 선호하는 신제품 모델이 다르다

### 독립성 검정

- 두 변수 사이에 관계가 없는지, 독립적인지 검정
    - 귀무가설 : 두 변수는 연관성이 없다.   **(독립이다)**
    - 대립가설 : 두 변수는 연관성이 있다.   **(독립이 아니다)**
        - 공장 라인과 제품 등급은 연관성이 없다.
        - 당뇨와 비만은 연관성이 없다.
        
        |  | 당뇨 | 정상 |
        | --- | --- | --- |
        | 비만체중 | 10 | 10 |
        | 정상체중 | 15 | 65 |
    
    ```python
    obs = pd.DataFrame({'당뇨':[10,15], '정상':[10,65]})
    obs.index = ['비만체중', '정상체중']
    
    from scipy.stats import chi2_contingency
    
    chir_square, p_value = chi2_contingency(obs, corection=False)
    ```
    

## 샤피로 윌크

- 정규분포인지 아닌지
    - 귀무가설 : **데이터가 정규분포를 따른다**
    - 대립가설 : **데이터가 정규분포를 따르지 않는다**
    
    ```python
    from scipy import stats
    
    data = [75, 83, 81, 92, 68, 77, 78, 80, 85, 95, 79, 89]
    
    # Shapiro-Wilk 검정 수행
    statistic, p_value = stats.shapiro(data)
    
    # 결과 출력
    print("Shapiro-Wilk 검정 통계량:", statistic)
    print("p-value:", p_value)
    
    # 유의 수준 0.05에서의 검정 결과 확인
    alpha = 0.05
    if p_value > alpha:
        print("귀무 가설을 기각할 수 없다. 데이터는 정규 분포를 따름")
    else:
        print("귀무 가설을 기각한다. 데이터는 정규 분포를 따르지 않음")
    ```
