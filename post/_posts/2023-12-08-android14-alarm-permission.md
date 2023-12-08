---
layout: post
title: Android v14로 업데이트시 알림 권한 이슈 
sitemap: true
categories: [study]
tags: [android]
description: >
  안드로이드 버전 14가 출시되면서 생겨난 문제점과 그 이유!
  java.lang.SecurityException: Caller com.kong.sub needs to hold android.permission.SCHEDULE_EXACT_ALARM or android.permission.USE_EXACT_ALARM to set exact alarms.
published: true
---

# Android v13 업데이트시, Permission 변경점

## 상황
안드로이드 14버전이 나오면서 해당 버전에서 앱을 실행하면 앱이 스플래시 화면에서 넘어가지 않는 현상이 발견!!

디버그 모드로 앱 로그를 확인해보니 아래와 같은 내용이 있었따.
~~~
java.lang.SecurityException: Caller com.kong.sub needs to hold android.permission.SCHEDULE_EXACT_ALARM or android.permission.USE_EXACT_ALARM to set exact alarms.
~~~ 

## 알림 권한
### v13 이전 타겟팅 
SCHEDULE_EXACT_ALARM 권한이 사전 부여가 됐었음.

### v13 이상 타겟팅 
아래 API를 사용시, 자체적으로 앱은 사용자에게 SCHEDULE_EXACT_ALARM 권한을 얻어야함.
~~~
setExact()
setExactAndAllowWhileIdle()
setAlarmClock()
~~~

* 만약 해당 API를 사용하는데 권한이 없으면 SecurityException이 발생

#### ?. 기존앱이 깔려있는 상태에서 v14로 업데이트시
- SCHEDULE_EXACT_ALARM이 이미 사전에 권한이 부여되어있음.  


<br>
<br>
<br>


## 적용하기.

1. canScheduleExactAlarms()로 권한을 확인

~~~kotlin
val alarmManager: AlarmManager = context.getSystemService<AlarmManager>()!!
when {
   // If permission is granted, proceed with scheduling exact alarms.
   alarmManager.canScheduleExactAlarms() -> {
       alarmManager.setExact(...)
   }
   else -> {
       // Ask users to go to exact alarm page in system settings.
       startActivity(Intent(ACTION_REQUEST_SCHEDULE_EXACT_ALARM))
   }
}
~~~

2. 권한 확인 후 리시버에서 처리

~~~kotlin
override fun onResume() {
   …  
   if (alarmManager.canScheduleExactAlarms()) {
       // Set exact alarms.
       alarmManager.setExact(...)
   }
   else {
       // Permission not yet approved. Display user notice and revert to a fallback  
       // approach.
       alarmManager.setWindow(...)
   }
}
~~~


<br>
<br>
<br>
<br>
<br>

참고
- [안드로이드 공식 문서](https://developer.android.com/about/versions/14/changes/schedule-exact-alarms?hl=ko)