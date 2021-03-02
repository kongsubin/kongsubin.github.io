---
layout: post
title: gradle version 추가 (MAC os)
description: >
  mac os에 gradle version 추가 하기. 
sitemap: false
categories: [study]
tags: [gradle]
---
## gradle version 추가 (MAC os)



### 1. 여기서 원하는 버전 다운 받는다. 

https://gradle.org/releases/

### 2. 다운 받은 파일을 원하는 곳에 위치 시킨다.

    ~~~
    mv 6.6 /usr/local/Cellar/gradle/6.6
    ~~~

### 3. 아래의 경로로 들어가서 path 변경시켜준다. 

    ```
    cd /usr/local/bin/
    ```
    ~~~
    vi gradle
    ~~~

### 4. 원하는 버전의 gradle의 실행파일이 있는 위치를 경로로 추가한다.

    ~~~
    JAVA_HOME="${JAVA_HOME:-/usr/local/opt/openjdk/libexec/openjdk.jdk/Contents/Home}" exec "/usr/local/Cellar/gradle/6.6/bin/gradle"  "$@" 
    ~~~

### 5. 마지막으로 버전을 확인해본다. 

    ~~~
    gradle --version
    ~~~



## 설치된적이 있는 그래들 리스트 보기 

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


