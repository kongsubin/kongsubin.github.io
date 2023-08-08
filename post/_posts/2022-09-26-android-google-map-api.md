---
layout: post
title: Android Google MAP API 사용하기 (Kotlin)
sitemap: true
categories: [study]
tags: [android]
published: true
---

# Android Google MAP 사용하기

> 안드로이드에서 구글 MAP API를 사용하는 방법을 알아보자! 

# 구글 지도 API 키 발급하기.

## 1. 프로젝트 생성. 

구글 지도 API를 생성하기 위해, 우선 프로젝트를 먼저 생성한다. 

[API 키 생성](https://developers.google.com/maps/documentation/android-sdk/get-api-key) 접속 후 사용자 인증 정보 페이지로 이동하기 클릭.

![](/assets/img/android/google_map_api/connet_google_api.png)

프로젝트 만들기를 클릭하여 프로젝트 생성을 한다. 
![](/assets/img/android/google_map_api/create_project1.png)

프로젝트 명 입력 후 저장한다. 
![](/assets/img/android/google_map_api/create_project2.png)


## 2. android API 사용설정하기.

프로젝트가 생성이 되었으면 이제 android API 사용을 설정해준다. 

사용설정된 API 및 서비스 > API 서비스 사용 설정탭을 들어간다. 
![](/assets/img/android/google_map_api/create_api_android1.png)

Maps SDK for Android 클릭한다. 
![](/assets/img/android/google_map_api/create_api_android2.png)

저장 클릭을 하면 다음과 같이 리스트에 정보가 뜨는 것 을 볼 수 있다. 
![](/assets/img/android/google_map_api/create_api_android3.png)
![](/assets/img/android/google_map_api/create_api_android4.png)

## 3 API 생성하기.

API사용 설정까지 끝마쳤으면 API를 생성하기만 하면 된다. 

사용자 인증 정보 만들기 클릭 > API 키 클릭한다. 
![](/assets/img/android/google_map_api/create_api1.png)

생성된 키는 추후에 local.properties에 추가해야하므로 클립보드에 복사해 둔다. 
![](/assets/img/android/google_map_api/create_api2.png)

생성된 키 설정 다시 들어간다. 
![](/assets/img/android/google_map_api/create_api3.png)

애플리케이션 제한사항 "Android" 설정한다. 
![](/assets/img/android/google_map_api/create_api4.png)

항목추가 버튼 클릭 후, 패키지명 및 SHA1 키 값 등록을 한다. 
![](/assets/img/android/google_map_api/create_api5.png)

저장 버튼 클릭하면 API 설정이 모두 끝이 났다. 
![](/assets/img/android/google_map_api/create_api6.png)

<br>
<br>
<br>
<br>

### 자 여기까지 API 생성을 하였다. 그럼 실제로 API를 사용하여 구글맵을 에뮬레이터에 띄워보자!!

<br>
<br>
<br>
<br>

# 구글 지도 띄우기.

## 1. Google Maps Activity 추가

구글 맵을 띄울 Activity를 하나 추가한다.

new > Activity > Google Maps Activity 추가를 하면, 필요한 라이브러리나 플러그인이 자동으로 생성이 된다. 

![](/assets/img/android/google_map_api/google_map_activity.png)

## 2. 위치 라이브러리 의존성 추가

모듈 수준 gradle.build파일에 위치정보 라이브러리를 추가하여 현재 위치 정보도 사용할 수 있게 한다. 

~~~kotlin
    // Google Maps Activity 추가시, 자동으로 추가되어있음. 
    implementation 'com.google.android.gms:play-services-maps:18.1.0'
    // 위치 정보 추가
    implementation 'com.google.android.gms:play-services-location:20.0.0'
~~~

## 3. local.properties 설정하기.

이제 local.properties에 API KEY를 설정하여 구글 맵을 사용해보자!

### gitignore 설정하기.
> local.properties가 원격저장소에 올라가는 것을 방지하기 위해, 우선 gitignore을 적용해준다. 

~~~kotlin
### Android ###
# Gradle files
.gradle/
build/

# Local configuration file (sdk path, etc)
local.properties
~~~

### local.properties 설정하기.
> gitignore을 적용했다면, local.properties에 아까 복사해둔 Google API Key 값을 설정해준다. 

~~~kotlin
GOOGLE_MAPS_API_KEY="클립보드에 복사했던 API KEY 붙여넣기"
~~~

### build.gradle 설정하기.
> 프로젝트 내에서 local.properties에 설정한 값을 사용할 수 있게 모듈 수준의 build.gradle파일을 설정한다. 

~~~kotlin
Properties properties = new Properties()
properties.load(project.rootProject.file('local.properties').newDataInputStream())

android {
    defaultConfig {
        // ...

        buildConfigField "String", "GOOGLE_MAPS_API_KEY", properties['GOOGLE_MAPS_API_KEY']
    }
}

~~~

### manifest 파일 설정하기.
> 마지막으로 manifest파일에 구글 키 alias를 설정해준다. 

~~~kotlin
<application>
    <meta-data
            android:name="com.google.android.geo.API_KEY"
            android:value="${GOOGLE_MAPS_API_KEY}" />
</application>
~~~


<br>
<br>

## 4. 프로젝트 구동 후 확인하기. 
프로젝트 구동 후, map 지도가 다음과 같이 뜬다면 성공한 것이다!!!

![](/assets/img/android/google_map_api/result.png)