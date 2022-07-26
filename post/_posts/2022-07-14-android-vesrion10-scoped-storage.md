---
layout: post
title: 안드로이드 버전 10 이상 파일 저장 방식 변화 
sitemap: false
categories: [study]
tags: [android]
---

# 안드로이드 버전 10 이상 파일 저장 방식 변화 

## 문제 상황
> 앱 내 카메라로 사진 촬영 기능에서, 촬영한 사진이 화면에 보이지 않는 현상 발생. 
> 문제를 좀 더 살펴보니, 카메라로 찍은 사진이 내부에 저장이 되지 않고 저장이 안되니 당연히 서버에 전송이 되지 않던 상황. 
> 그럼 당연히 사진이 화면에 보이지 않음!

## 문제 원인
> 안드로이드 버전 10 이상부터 내부 파일 저장방식이 바뀜에 따라 촬영한 사진이 저장이 되지 않았음. 

## 해결 방법
> 안드로이드 버전 10 이상은 파일 저장 방식을 다르게 하도록 분기 처리!

### 이전 코드 
~~~java
file = new File(Environment.getExternalStoragePublicDirectory(Environment, DIRECTORY_DCIM), file_name);
String auth = activity.getApplicationContext().getPackageName() + FILE_PROVIDER;
String uri = FileProvider.getUriForFile(activity, auth, file);
~~~

### 이후 코드
~~~java
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.Q) {
    ContentValues values = new ContentValues();
    values.put(MediaStore.Images.Media.DISPLAY_NAME, file_name);
    values.put(MediaStore.Images.Media.MIME_TYPE, "image/png");
    values.put(MediaStore.Images.Media.RELATIVE_PATH, "DCIM/" + "YOUR_FOLDER");

    ContentResolver contentResolver = activity.getContentResolver();
    uri = contentResolver.insert(MediaStore.Images.Media.EXTERNAL_CONTENT_URI, values);

    file = new File(getRealPathFromURI(uri));
} else {
    file = new File(Environment.getExternalStoragePublicDirectory(Environment, DIRECTORY_DCIM), file_name);
    auth = activity.getApplicationContext().getPackageName();
    uri = FileProvider.getUriForFile(activity, auth, file);
}
~~~


<br>
<br>
<br>
<br>
<br>
<br>
<br>
[Stack Over FLow](https://stackoverflow.com/questions/57116335/environment-getexternalstoragedirectory-deprecated-in-api-level-29-java)

