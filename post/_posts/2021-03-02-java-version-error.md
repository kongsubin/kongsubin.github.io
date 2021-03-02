---
layout: post
title: 자바 버젼 충돌 에러
description: >
  자바 class 파일 버전 충돌 
sitemap: false
categories: [study]
tags: [error]
---

# 자바 버젼 충돌 에러 

~~~bash
Error: LinkageError occurred while loading main class edu.handong.csee.isel.Main 
java.lang.UnsupportedClassVersionError: edu/handong/csee/isel/Main has been compiled by a more recent version of the Java Runtime 
(class file version 59.0), this version of the Java Runtime only recognizes class file versions up to 58.0
~~~

위의 에러 결과를 그대로 해석하자만 다음과 같다. 

> java.lang.UnsupportedClassVersionError : "edu / handong / csee / isel / Main"이 최신 버전의 Java 런타임 (클래스 파일 버전 59.0)에 의해 컴파일되었습니다. 이 버전의 Java 런타임은 최대 58.0의 클래스 파일 버전 만 인식합니다.

이는 빌드한 자바 버전보다 낮은 버전의 자바 컴파일러에서 실행시 발생할 때 나타난다. 


### 다음은 자바 버전과 클래스 파일 버전 매칭 정보이다. 
~~~
45 = Java 1.1
46 = Java 1.2
47 = Java 1.3
48 = Java 1.4
49 = Java 5
50 = Java 6
51 = Java 7
52 = Java 8
53 = Java 9
54 = Java 10
55 = Java 11
56 = Java 12
57 = Java 13
58 = Java 14
59 = Java 15

~~~


#### 컴퓨터에 설치된 자바 버전 목록 보기 

~~~
/usr/libexec/java_home -V
~~~


#### 프로젝트의 build.gradle 파일에 아래의 코드를 추가하였다. 

~~~
sourceCompatibility = 11 
targetCompatibility = 11
~~~

> 다음의 의미는 프로젝트를 build할 때, 자바버전을 11로 고정시켜준다는 의미이다. 

