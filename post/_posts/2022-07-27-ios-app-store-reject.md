---
layout: post
title: [IOS 앱 심사 리젝 해결] - 2.1.0 Performance App Completeness 
sitemap: false
categories: [study]
tags: [ios]
---

# 앱 심사 리젝 

> ### 2.1.0 Performance : App Completeness

<br>

## 첫번째 심사 리젝 

ios 앱 심사를 올린 지 얼마되지 않고 앱 심사 거절 회신이 돌아왔다.

![](/assets/img/ios/ios_reject_first.png)

회신된 답변에 함께 첨부된 파일을 보니, 앱 스플래시 화면에서 넘어가지 않았다. 

그래서 모든 테스트를 다시 진행하고 배포를 다시하여 업로드 하였다. 

<br>
<br>
<br>

## 두번째 심사 리젝

하루 뒤, 다시 심사 리젝 회신이 돌아왔다. 

![](/assets/img/ios/ios_reject_second.png)

리젝된 이유도 첫번째 심사와 같았다.   

앱 스플래시 화면에서, 서버와 통신이 정상적으로 이루어지지 않는다는 점이다. 

<br>
<br>
<br>

# 원인 분석

애플쪽에서 우리의 서버와 통신이 되지 않아 앱 실행 자체가 되지 않는다. 

하지만 우리쪽에서 테스트 했을 때 앱을 사용하는데 문제가 전혀 없다. 

한가지 예상할 수 있는 점은 우리의 서버가 해외 접속에 대해 보수적이라 애플의 접속을 허용하지 않는 것 같다. 

<br>
<br>
<br>

# 해결 방법

[애플 앱 심사 가이드라인](https://developer.apple.com/app-store/review/)을 보면, 재현이 힘든 환경에서는 Demo Video로 진행할 수 있다고 안내되어있다. 

![](/assets/img/ios/ios_app_store_guidelines.png)

하드웨어가 필요한 앱이 영상을 찍어 심사를 진행하는 것 같긴한데, 일단 시도는 해보았다.....(밑져야 본전!)

구글 번역기를 사용하여, 우리쪽 서버의 상황을 설명하고 앱 작동하는 영상을 간단하게 찍어 회신하였다. 

<br>
<br>
<br>

# 결과

다행히 애플에서 심사를 통과시켜줬다!!

일단 이번에는 넘어가긴 했지만, 애플이 계속 데모 영상으로 앱 심사를 대체 해줄지는 미지수이기에 왜 애플의 접속을 튕겨내는지 원인 파악이 필요할 듯 싶다. 

<br>
<br>
<br>





