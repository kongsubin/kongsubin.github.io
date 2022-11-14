---
layout: post
title: Android Target SDK 31 이상일 때, FCM관련하여 변경되는 점. 
sitemap: false
categories: [study]
tags: [android]
description: >
  안드로이드 Target SDK Version 30 => 31로 업데이트함에 따라, PendingIntent 생성시 "FLAG_IMMUTABLE" 또는 "FLAG_MUTABLE" Flag 추가 
---

# Android Target SDK 31 이상일 때, FCM관련하여 변경되는 점. 


## 문제 상황
안드로이드 Target SDK Version을 30에서 31로 업데이트함에 따라, 다음과 같은 오류가 발생했다. 
~~~error
java.lang.IllegalArgumentException: .... : Targeting S+ (version 31 and above) requires that one of FLAG_IMMUTABLE or FLAG_MUTABLE be specified when creating a PendingIntent. Strongly consider using FLAG_IMMUTABLE, only use FLAG_MUTABLE if some functionality depends on the PendingIntent being mutable
~~~

## 문제 원인
[공식 문서](https://developer.android.com/about/versions/12/behavior-changes-12#pending-intent-mutability)를 찾아보니 Android12를 타겟팅 하는 경우에, 보안을 위해 PendingIntent 객체의 변경 가능 여부에 대하여 Flag값이 필수가 되었다. 

## 해결 방법
해당 문제를 해결하기 위해, 나는 3가지 과정을 거쳤다. 

1. PendingIntent를 생성하는 부분에 필수로 들어가야하는 "FLAG_IMMUTABLE" 또는 "FLAG_MUTABLE" Flag를 추가하기. 
2. Dependency 추가하기. 
3. 라이브러리 최신버전으로 업데이트하기. 


# 1. "FLAG_IMMUTABLE" 또는 "FLAG_MUTABLE" 추가하기
가장 먼저, PendingIntent를 생성하는 부분을 소스상에서 찾아 수정하였다.
에러 로그에 나와있듯이 별다른 특징이 없을 경우, FLAG_IMMUTABLE의 사용을 권장하고 있어 분기처리를 통해 FLAG_IMMUTABLE를 추가하였다. 

- [FLAG_IMMUTABLE](https://developer.android.com/reference/android/app/PendingIntent?hl=ko#FLAG_IMMUTABLE) : 생성된 PendingIntent을 변경할 수 없음. 
- [FLAG_MUTABLE](https://developer.android.com/reference/android/app/PendingIntent?hl=ko#FLAG_MUTABLE) : 생성된 PendingIntent을 변경할 수 있음.
  
~~~java
PendingIntent pendingIntent;
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.S){
   pendingIntent = PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_IMMUTABLE);
}else {
   pendingIntent = PendingIntent.getBroadcast(context, 0, intent, PendingIntent.FLAG_ONE_SHOT);
}
~~~


# 2. Dependency 추가하기
첫번째 방법을 진행하였음에도, 똑같은 에러가 발생하였다. 

구글링 하던중 StackoverFlow에 나와있는 모듈 수준의 buiild.gralde에 다음의 Dependency를 추가하는 방법을 시도하였다.

~~~
implementation 'androidx.work:work-runtime-ktx:2.7.1'
~~~


# 3. 라이브러리 최신버전으로 업데이트하기
두번째 방법까지 시도했음에도, 똑같은 에러가 발생하였다. 

마지막이라 생각하고 라이브러리 최신화 적용을 하였다. 

Before
~~~xml
   implementation 'com.google.firebase:firebase-core:16.0.6'
   implementation 'com.google.firebase:firebase-messaging:17.3.4'
   implementation 'com.google.firebase:firebase-dynamic-links:16.1.8'
~~~

After
~~~xml
   implementation 'com.google.firebase:firebase-core:21.1.1'
   implementation 'com.google.firebase:firebase-messaging:23.1.0'
   implementation 'com.google.firebase:firebase-dynamic-links:21.1.0'
~~~

확인해보니 *firebase-messaging 라이브러리를 한참 옛날 것*을 사용하고 있었다. ~~(이러니 에러가 나는 것도 당연.......)~~

최신 버전으로 업데이트하니, deprecated된 함수들이 존재하여 바꿔주었다. 

Deprecated 목록
- FirebaseInstanceId
- getInstanceId()
- InstanceIdResult

## 이전 코드
~~~java
import com.google.firebase.iid.FirebaseInstanceId;
import com.google.firebase.iid.InstanceIdResult;

...

FirebaseInstanceId.getInstance().getInstanceId().addOnCompleteListener(new OnCompleteListener<InstanceIdResult>() {
   @Override
   public void onComplete(@NonNull Task<InstanceIdResult> task) {
      if (task.isSuccessful()) {
         ...
         String token = task.getResult().getToken();
         ...
      }
      ...
   }
...
~~~

## 이후 코드 
~~~java
import com.google.firebase.messaging.FirebaseMessaging;

...

FirebaseMessaging.getInstance().getToken().addOnCompleteListener(new OnCompleteListener<String>() {
   @Override
   public void onComplete(@NonNull Task<String> task) {
      if (task.isSuccessful()) {
         ...

         String token = task.getResult();

         ...
      }
   }
...
~~~

추가적으로 FCM과 관련된 라이브러리 외에, KaKao SDK 라이브러리에서도 동일한 에러가 발생하여, 해당 라이브러리도 업데이트하였다.

[KaKao SDK Release Note](https://developers.kakao.com/docs/latest/ko/sdk-download/android-v1)를 확인해보니, Android 12 대응을 위한 v1.30.7 버전이 새로 나왔음을 확인하였고, 해당 버전으로 업데이트를 진행하였다. 




#### 앱에서 사용하고 있는 라이브러리를 정기적으로 업데이트해야겠다고 생각했다... 
#### 단순히 30 => 31로 가는것인데 많은 라이브러리를 업데이트하고 소스 수정하고.... 대공사를 해야하는게 왜이렇게 많은 것인지 정말...






