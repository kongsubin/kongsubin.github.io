---
layout: post
title: 자바 버젼 충돌 에러
description: >
  Howdy! This is an example blog post that shows several types of HTML content supported in this theme.
sitemap: false
categories: [study]
tags: [error]
---

# 자바 버젼 충돌 에러 

~~~bash
Error: LinkageError occurred while loading main class edu.handong.csee.isel.Main java.lang.UnsupportedClassVersionError: edu/handong/csee/isel/Main has been compiled by a more recent version of the Java Runtime (class file version 59.0), this version of the Java Runtime only recognizes class file versions up to 58.0
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


나 같은 경우에는 java 버전은 14이고, gradle 버전이 6.8이다. 근데 JVM버전이 15이여서 그런 것 같다. 따라서 gradle 버전 6.6을 설치하여 자바 JVM버전을 맞춰준다. 

~~~
➜  ~ java --version
openjdk 14.0.1 2020-04-14
OpenJDK Runtime Environment (build 14.0.1+7)
OpenJDK 64-Bit Server VM (build 14.0.1+7, mixed mode, sharing)
➜  ~ gradle --version

------------------------------------------------------------
Gradle 6.8
------------------------------------------------------------

Build time:   2021-01-08 16:38:46 UTC
Revision:     b7e82460c5373e194fb478a998c4fcfe7da53a7e

Kotlin:       1.4.20
Groovy:       2.5.12
Ant:          Apache Ant(TM) version 1.10.9 compiled on September 27 2020
JVM:          15.0.1 (Oracle Corporation 15.0.1+9)
OS:           Mac OS X 10.14.6 x86_64
~~~



설치된적이 있는 그래들 리스트 보기 
~~~
➜  ~ brew info gradle
gradle: stable 6.8
Open-source build automation tool based on the Groovy and Kotlin DSL
https://www.gradle.org/
/usr/local/Cellar/gradle/6.8 (11,240 files, 259.2MB) *
  Built from source on 2021-01-13 at 11:01:42
From: https://github.com/Homebrew/homebrew-core/blob/HEAD/Formula/gradle.rb
License: Apache-2.0
==> Dependencies
Required: openjdk ✔
==> Analytics
install: 35,887 (30 days), 127,559 (90 days), 568,222 (365 days)
install-on-request: 35,476 (30 days), 124,628 (90 days), 545,988 (365 days)
build-error: 0 (30 days)
~~~



환경변수 설정
~~~
vi ~/.bash_profile
~~~

적용하기
~~~
source ~/.bash_profile
~~~

확인
~~~
echo $PATH
~~~



## gradle 버전 추가 하기

https://gradle.org/releases/

1. 여기서 원하는 버전 다운 받는다. 

2. 다운 받은 파일을 원하는 곳에 위치 시킨다.

~~~
mv 6.6 /usr/local/Cellar/gradle/6.6
~~~

3. 아래의 경로로 들어가서 path 변경시켜준다. 

```
cd /usr/local/bin/
```

~~~
vi gradle
~~~

4. 원하는 버전의 gradle의 실행파일이 있는 위치의 경로 추가한다.

~~~
JAVA_HOME="${JAVA_HOME:-/usr/local/opt/openjdk/libexec/openjdk.jdk/Contents/Home}" exec "/usr/local/Cellar/gradle/6.6/bin/gradle"  "$@" 
~~~

5. 마지막으로 버전을 확인해본다. 

~~~
gradle --version
~~~



그래도 여전히 에러남. 그래들 버전문제가 아니었나보다.
그래서 그냥 프로젝트의 build.gradle 파일에

~~~
sourceCompatibility = 11 
targetCompatibility = 11
~~~

자바버전을 고정시켜주었다. 

그러니까 실행이된다. 

진작 이럴걸. 몇시간을 날린거야;