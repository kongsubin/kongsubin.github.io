---
layout: post
title: 안드로이드 버전 10 이상 파일 저장 방식 변화 
sitemap: false
categories: [study]
tags: [android]
---

# QUERY_ALL_PACKAGES 사용시, 권한 선언 필요 

구글 앱 등록 정책이 바꼈다. 

앱에서 "QUERY_ALL_PACKAGES" 권한을 사용하면, 

왜 저 권한이 필요한지와 해당 권한을 사용하는 앱 동영상을 녹화하여 구글 콘솔에 업로드 해야한다. 

만약 하지 않는다면 2022년 07월 20부터 앱 업데이트를 진행할수가 없따!!!!!!!!!!

내가 업데이트 하고자 하는앱은 금융 관련 앱이라, 백신 관련 툴로 인해 **QUERY_ALL_PACKAGES** 권한을 사용된다. 

방법은 다음과 같다. 

## 1. 민감한 권한 및 API에서 시작 버튼 클릭

![](/assets/img/android/QUERY_ALL_PACKAGES1.png)

## 2. 핵심 목적을 작성한다. 
   
> [QUERY_ALL_PACKAGES 퍼미션 권한 사용 이유]

1. @@@@@앱은 바이러스 백신 및 보안 기능이 추가된 앱으로 사용자의 안전한 앱 사용을 위해서 QUERY_ALL_PACKAGES 권한을 사용합니다

2. QUERY_ALL_PACKAGES 퍼미션 권한 추가로 불안전한 바이러스 앱 여부 또는 위변조 여부를 확인해 사용자에게 보안 알림을 알려주고 있습니다

3. @@@@@은 구글 정책에서 명시한 앱 기능 , 사기 예방, 보안 규정 준수 용도로 QUERY_ALL_PACKAGES 권한을 사용하고있습니다

4. 첨부 파일 앱 시연 동영상에서 바이러스 앱 탐지 기능을 수행하고 있습니다.

![](/assets/img/android/QUERY_ALL_PACKAGES2.png)

## 3. 사기예방, 보안, 규정준수 클릭. 

![](/assets/img/android/QUERY_ALL_PACKAGES3.png)

## 4. 마지막으로 동영상 찍어서 url을 올리면 된다. 

나는 참고로 앱 시작시, 백신 프로그램이 작동되는 짧은 영상을 올렸다. 

![](/assets/img/android/QUERY_ALL_PACKAGES4.png)


정책은 잊을만 하면 바뀌는 것 같다! 






<br>
<br>
<br>
<br>
<br>
<br>
<br>
[참고한 블로그](https://kkh0977.tistory.com/1789)
