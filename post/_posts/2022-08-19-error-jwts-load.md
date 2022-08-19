---
layout: post
title: jwts 라이브러리 사용시, Class could not be found Error
description: Unable to load class named [io.jsonwebtoken.impl.DefaultJwtBuilder] from the thread context, current, or system/application ClassLoaders. All heuristics have been exhausted. Class could not be found. Have you remembered to include the jjwt-impl.jar in your runtime classpath?
sitemap: false
categories: [study]
tags: [error, java]
---
# jwts 라이브러리 사용시, Class could not be found Error

# 문제 상황
jwt 라이브러리 사용시 아래 에러가 뿜어지는 현상이 나타났다. 

> Unable to load class named [io.jsonwebtoken.impl.DefaultJwtBuilder] from the thread context, current, or system/application ClassLoaders. All heuristics have been exhausted. Class could not be found. Have you remembered to include the jjwt-impl.jar in your runtime classpath?

<br>
<br>

# 문제 원인
확인해보니 dependency 주입을 잘못해서 나는 에러였다. 

<br>
<br>

# 해결 방법
[Jwts Github](https://github.com/jwtk/jjwt#install-jdk-gradle)에 설명되어있는 dependency를 참고하여 의존성을 주입시켰다. 
~~~xml
implementation 'io.jsonwebtoken:jjwt-api:0.11.5'
runtimeOnly 'io.jsonwebtoken:jjwt-impl:0.11.5'
runtimeOnly 'io.jsonwebtoken:jjwt-jackson:0.11.5'
~~~

