---
layout: post
title: Android v13로 업데이트시 권한 이슈 
sitemap: true
categories: [study]
tags: [android]
description: >
  안드로이드 Target SDK Version 31 => 33로 업데이트함에 따라, 변경된 Permission 
published: true
---

# Android v13 업데이트시, Permission 변경점

## 상황
안드로이드 13버전이 나오면서 앱 권한과 관련하여 몇가지 변경사항이 있다. 


## 1. PUSH 알림 권한
### v13 이전
Notification Channel이 생성이 되거나 Notification Channel이 생성된 이후 앱이 런타임 될 때, 

POST_NOTIFICATIONS 권한을 시스템이 자동으로 앱에 추가하고 권한 요청 알림도 띄워주었다. 

### v13
앱 런타임시, 자체적으로 앱은 사용자에게 PUSH NOTIFICATION 권한을 얻어야한다. 

만약, 사용자가 권한을 거부할 경우 앱이 종료된다.

#### ?. target이 12인 앱이 v13에 다운받아졌을땐?
- Notification Channel이 생성될 때 권한 요청을 한다. 

#### ?. target이 13이고 앱이 v12이하에 다운받아졌을땐?
- POST_NOTIFICATIONS권한이 이미 on이므로 상관 없음. 

#### ?. 기기를 v12에서 v13으로 없데이트한 경우에는?
- POST_NOTIFICATIONS권한이 v12였을때 이미 수락상태이므로 상관 없음. 

<br>
<br>
<br>

## 2. READ_EXTERNAL_STORAGE 권한 세분화
READ_EXTERNAL_STORAGE 권한은 사용자 앱 저장소에 접근하여 읽는 권한이다. 

해당 권한이 안드로이드 v13 이후부터 3가지로 세분화 되었다.

세분화된 권한 중 앱에 필요한 권한만 추가하여 사용하면 된다. 

### v13 이전
- READ_EXTERNAL_STORAGE

### v13 
- READ_MEDIA_IMAGES
- READ_MEDIA_AUDIO
- READ_MEDIA_VIDEO


## 코드 적용하기.

### 1. AndroidManifest에 권한을 추가해준다. 

이전
~~~xml
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE"/>
~~~

이후
~~~xml
<!-- TargetSdkVersion 33 Notification Permission -->
<uses-permission android:name="android.permission.POST_NOTIFICATIONS"/>
<uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" android:maxSdkVersion="32"/> 
~~~


### 2. 사용자에게 권한 요청하는 로직을 분기 처리를 한다. 
~~~java
    private String[] permissions;
    private List<String> deniedPermissions = new ArrayList<String>();

    @Override
	protected void onCreate(Bundle savedInstanceState) {
        
        ...

        // 필수 요청 권한 목록
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
            permissions = new String[]{
                Manifest.permission.READ_PHONE_STATE,
                Manifest.permission.READ_MEDIA_IMAGES,
                Manifest.permission.POST_NOTIFICATIONS
            };
        }else {
            permissions = new String[]{
                Manifest.permission.READ_PHONE_STATE,
                Manifest.permission.READ_EXTERNAL_STORAGE
            };
        }

        // 권한 획득 여부 확인
        boolean isNeedPermission = false;
        for (String permission : permissions) {
            if (ContextCompat.checkSelfPermission(this, permission) != PackageManager.PERMISSION_GRANTED) {
                deniedPermissions.add(permission);
                isNeedPermission = true;
            }
        }

        // 권한 요청하기 
        if(isNeedPermission) requestPermisions();

        ...
    }

    private void requestPermissions(){

		PermissionListener permissionlistener = new PermissionListener() {
			@Override
			public void onPermissionDenied(ArrayList<String> deniedPermissions) {
				// 권한 거부시 action
			}

			@Override
			public void onPermissionGranted() {
                // 권한 승인시 action 
			}
		};

        // 권한 요청 
		if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.TIRAMISU) {
			TedPermission.with(this)
					.setPermissionListener(permissionlistener)
					.setPermissions(
							deniedPermissions.toArray(new String[0]);
					)
					.check();
		} else {
			TedPermission.with(this)
					.setPermissionListener(permissionlistener)
					.setPermissions(
							deniedPermissions.toArray(new String[0]);
					)
					.check();
		}
    }
    
~~~


<br>
<br>
<br>
<br>
<br>

참고
- [Migrating My App To Android 13](https://medium.com/tech-takeaways/migrating-my-app-to-android-13-f5ad0649d23d)