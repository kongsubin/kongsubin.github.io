---
layout: post
title: adb를 이용한 안드로이드 apk 설치 
sitemap: false
categories: [study]
tags: [android]
description: >
  컴퓨터에 있는 apk 파일을 usb를 이용해서 단말기에 바로 설치하기!
---

# adb를 이용한 안드로이드 apk 설치

## 1. 명령어 경로 설정해주기.
~~~bash
    export PATH=$PATH:/Users/${사용자}/Library/Android/sdk/platform-tools
~~~

<br>
<br>


## 2. 디바이스 목록 확인
apk 설치 가능한 디바이스 목록이 나온다. 
~~~bash
    ➜  adb devices
    List of devices attached
    ${디바이스ID}     device
    emulator-5554   device
~~~

<br>
<br>

## 3. 설치하기
~~~bash
➜  adb -s ${디바이스ID} install -r kongsub.apk 
Performing Streamed Install
Success
~~~

