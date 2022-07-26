---
layout: post
title: JAVA 한글 파일명 깨짐
sitemap: false
categories: [study]
tags: [java]
---

# JAVA 한글 파일명 깨짐

> 파일 다운로드 기능을 구현할 때, 의도치 않게 한글명이 깨지는 경우가 있다. 

> 예를 들어, 민원접수_오늘날짜 로 파일명을 하드코딩 할 때, “민원접수” 한글 부분이 깨지는 현상.

> 그럴때는 아래와 같이 파일명을 지정해주면 한글이 깨지지 않고 파일이 다운 받아 진다. 

~~~java

String filename = "고객민원_" + todayStr;

filename = new String(filename.getBytes("UTF-8"), "ISO-8859-1");

~~~
