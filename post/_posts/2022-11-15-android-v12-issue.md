---
layout: post
title: Android v12로 업데이트시, 발생하는 이슈 모음
sitemap: false
categories: [study]
tags: [android]
description: >
  안드로이드 Target SDK Version 30 => 31로 업데이트함에 따라, 변경해야하는 사항 모음.
---

# Android v12로 업데이트시, 발생하는 이슈 모음

## 1. android:exported 명시
> Androidmanifest.xml 파일에 Activity, Service 또는 Broadcast receiver를 선언할 때 android:exported를 명시해야함. 

### android:exported란?
> 다른 애플리케이션의 구성요소에서 활동을 시작할 수 있는지를 설정하는 옵션이다. 

1. true : 모든 앱에서 활동에 액세스할 수 있으며 정확한 클래스 이름으로 활동을 시작할 수 있음
2. false : 활동은 같은 애플리케이션의 구성요소나 사용자 ID가 같은 애플리케이션, 권한이 있는 시스템 구성요소에서만 시작될 수 있음. (default 값)

### 관련 Post
- [네이버 로그인 SDK 4.2.6 => 5.2.0](https://kongsubin.github.io/post/study/2022-11-09-android-naver-sdk-v5.2.0/)
- [developer document](https://developer.android.com/guide/topics/manifest/activity-element?hl=ko)
- [안드로이드 android:exported 설명](https://m.blog.naver.com/websearch/221668354461)

<br><br><br>

## 2. PendingIntent 사용시, Flag 지정
> PendingIntent 생성시 "FLAG_IMMUTABLE" 또는 "FLAG_MUTABLE" Flag 추가해야함. 

### PendingIntent란?
> 가지고 있는 Intent 를 당장 수행하진 않고 특정 시점에 수행하도록 하는 특징을 갖고 있음. (이 '특정 시점'이라 함은, 보통 해당 앱이 구동되고 있지 않을 때이다.)
대표적으로 PendingIntent를 많이 사용하는 기능에는 Notification (푸시알림)이 있다. 

1. [FLAG_IMMUTABLE](https://developer.android.com/reference/android/app/PendingIntent?hl=ko#FLAG_IMMUTABLE) : 생성된 PendingIntent을 변경할 수 없음. 
2. [FLAG_MUTABLE](https://developer.android.com/reference/android/app/PendingIntent?hl=ko#FLAG_MUTABLE) : 생성된 PendingIntent을 변경할 수 있음.

### 관련 Post
- [PendingIntent Flag 지정해주기](https://kongsubin.github.io/post/study/2022-11-10-android-pendingintent-flag/)  
- [[Android] PendingIntent 개념 익히기](https://velog.io/@haero_kim/Android-PendingIntent-%EA%B0%9C%EB%85%90-%EC%9D%B5%ED%9E%88%EA%B8%B0)


- 