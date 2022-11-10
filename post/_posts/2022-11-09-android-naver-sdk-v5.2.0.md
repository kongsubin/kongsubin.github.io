---
layout: post
title: Android 네이버 로그인 SDK 4.2.6 => 5.2.0 (JAVA)
sitemap: false
categories: [study]
tags: [android]
description: >
  안드로이드 Target SDK Version 30 => 31로 업데이트함에 따라, 네아로 버전 업데이트. 
---

# 네이버 로그인 SDK 4.2.6 => 5.2.0
> 네아로 SDK이란, 서드파티 애플리케이션에서 네이버 로그인 기능을 제공하는 라이브러리이다. 

## 상황 
[Google의 권고사항](https://developer.android.com/google/play/requirements/target-sdk#pre12)에 따라 운영중이던 SDK Version을 30에서 31로 update하였다.

31로 Targeting을 하니, Androidmanifest.xml 파일에 android:exported를 명시를 해주어야했다.

하지만 우리 앱에서 사용하는 네아로 v4.2.6에는 android:exported를 명시되지 않았고, 이는 v5.0.0 부터 대응이 된다고 [naver login release notes](https://github.com/naver/naveridlogin-sdk-android/wiki/%EB%A6%B4%EB%A6%AC%EC%A6%88-%EB%85%B8%ED%8A%B8)에 나와있었다. 

따라서, 업데이트가 필수적인 상황이었고 이왕 업데이트할거 최신 버전으로 업데이트 하기로 결정을 내렸다. 

## 수정 사항 
- OAuthLogin deprecated 
* &rightarrow; NaverIdLoginSDK
<br>

- OAuthLogin.getInstance() deprecated 
* &rightarrow; NaverIdLoginSDK.INSTANCE
<br>

- OAuthLogin.init deprecated 
* &rightarrow; NaverIdLoginSDK.initialize
<br>

- OAuthLogin.startOauthLoginActivity deprecated 
* &rightarrow; NaverIdLoginSDK.authenticate
<br>

- OAuthLoginHandler deprecated 
* &rightarrow; OAuthLoginCallback
<br>

### 기존 코드
~~~java 
    private static OAuthLogin naverLoginInstance;
    private String token;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
        ...
        naverLoginButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                naverLoginInstance = OAuthLogin.getInstance();
                naverLoginInstance.init(this, OAUTH_CLIENT_ID, OAUTH_CLIENT_SECRET, OAUTH_CLIENT_NAME);
                naverLoginInstance.startOauthLoginActivity(this, naverLoginHandler);
            }
        });
        ... 
    }

    private OAuthLoginHandler naverLoginHandler = new OAuthLoginHandler() {
        @Override
        public void run(boolean success) {
            String tokenType = "";
            String errorCode = "";
            String errorDesc = "";
            if (success) {
                token = naverLoginInstance.getAccessToken(this);
                tokenType = naverLoginInstance.getTokenType(this);
                Toast.makeText(this, "네이버 로그인에 성공하였습니다.", Toast.LENGTH_SHORT).show();
            } else {
                errorCode = naverLoginInstance.getLastErrorCode(this).getCode();
                errorDesc = naverLoginInstance.getLastErrorDesc(this);
                Toast.makeText(this, "네이버 로그인에 실패했습니다.", Toast.LENGTH_SHORT).show();
            }
        }
    }
~~~


### 변경 코드
~~~java
    private static NaverIdLoginSDK naverLoginInstance;
    private String token;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
        ...
        naverLoginButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                naverLoginInstance = NaverIdLoginSDK.INSTANCE;
                naverLoginInstance.initialize(this, OAUTH_CLIENT_ID, OAUTH_CLIENT_SECRET, OAUTH_CLIENT_NAME);
                naverLoginInstance.authenticate(this, launcher, mAuthLoginCallback);
            }

        });
        ... 
    }

    private ActivityResultLauncher<Intent> launcher = registerForActivityResult(
        new ActivityResultContracts.StartActivityForResult(),
        new ActivityResultCallback<ActivityResult>() {
            @Override
            public void onActivityResult(ActivityResult result) {
                if(result.getResultCode() == RESULT_OK){
                    token = naverLoginInstance.getAccessToken();
                    Toast.makeText(this, "네이버 로그인에 성공하였습니다.", Toast.LENGTH_SHORT).show();
                }else if(result.getResultCode() == RESULT_CANCELED){
                    Toast.makeText(mBaseContext, "네이버 로그인에 실패했습니다.", Toast.LENGTH_SHORT).show();
                }
            }
        }
    );

    private OAuthLoginCallback mAuthLoginCallback = new OAuthLoginCallback(){
        @Override
        public void onSuccess() {
            token = naverLoginInstance.getAccessToken();
            Toast.makeText(this, "네이버 로그인에 성공하였습니다.", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onFailure(int i, @NonNull String s) {
            Toast.makeText(mBaseContext, "네이버 로그인에 실패했습니다.", Toast.LENGTH_SHORT).show();
        }

        @Override
        public void onError(int errorCode, @NonNull String message) {
            onFailure(errorCode, message);
        }
    };

~~~





참고
- [네이버 공식 개발 문서](https://developers.naver.com/docs/login/android/android.md)