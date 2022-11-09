---
layout: post
title: Android PDF Viewer 
sitemap: false
categories: [study]
tags: [android]
---

# Android PDF Viewer 

## 문제 상황
안드로이드에서 PDF Viewer 기능 사용 시, 에러로 인한 PDF Viewer 기능이 제대로 작동하지 않음. 

> 에러 내용 
~~~cmd
open failed: EACCES (Permission denied)
~~~

## 문제 원인
안드로이드 API 29 (Q) 이상 부터, 보안상의 이유로 외부 저장소에 접근하는 것이 불가능하게 됨.

따라서 외부 저장소에 파일을 쓰고 읽는 것이 거부됨. 

## 해결 방법 
1. android:preserveLegacyExternalStorage="true" 선언 
2. MANAGE_EXTERNAL_STORAGE 권한 사용 
3. MediaStore or SAF(Storage Access Framework) 사용


### 1. android:preserveLegacyExternalStorage="true"
AndroidManifest.xml file에 아래와 같이 선언하면 임시적으로 해결이 된다. 
~~~xml

<uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
<uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" />

<application
        ...
        android:preserveLegacyExternalStorage="true"
        ...
~~~

하지만 이는 임시적인 방편일뿐 문제를 완벽하게 해결하지 못한다. 기각!

### 2. MANAGE_EXTERNAL_STORAGE 권한 사용 
AndroidManifest.xml file에 아래와 같이 권한을 주면 된다.  
~~~xml
<uses-permission android:name="android.permission.MANAGE_EXTERNAL_STORAGE"/>
~~~

하지만 이 방법은 구글에게 해당 권한을 사용하는 합당한 이유를 상세히 알려야한다. 

하지만 우리 앱은 그런 합당한 이유가 없으므로 기각!


### 3. MediaStore or SAF(Storage Access Framework) 사용
MediaStore나 SAF는 외부 저장소 파일에 접근 할 수 있게 도와주는 기능이다. 

### MediaStore 
이미지, 동영상 파일 처리시 주로 사용

### SAF
txt, pdf 파일 등을 탐색하고 여는 작업을 간편하게 해주는 프레임워크. 


문제 없이 사용할 수 있는 유일한 방법이다. 

그래서 SAF를 사용하여 PDF Viewer기능을 처리하였다.
<br><br>


### 로직
1. PDF 다운로드 URL을 URLConnection을 이용하여 input stream을 연다. 
2. SAF를 이용하여 파일을 생성한 뒤, output stream을 연다. 
3. input stream으로 read한 데이터를 output stream에 write 한다. 
4. intent로 pdf uri를 넘겨 pdf viewer를 실행한다. 

크게 달라진점은, 그냥 외부 저장소에 직접 접근하여 파일을 생성하냐, SAF를 이용하여 파일을 생성하냐의 차이이다. 

### 기존 코드
~~~java 

    private String pdfDownloadUrl = "https://kongsub.co.kr/download/test.pdf";
    private String filename = "test.pdf";
    private String filePath;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    
        ...
        pdfViewerButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                // pdf 다운로드 - 비동기처리 
                final DownloadTask downloadTask = new DownloadTask();
                downloadTask.execute();
            }
        });
        ... 
    }

    private class DownloadTask extends AsyncTask<String, String, Boolean> {
        @Override
        protected Boolean doInBackground(String... params) {
            try{
                // PDF 다운로드 URL을 URLConnection을 이용하여 input stream을 연다. 
                URL url = new URL(pdfDownloadUrl);      
                URLConnection connection = url.openConnection();
                connection.connect();

                InputStream input = new BufferedInputStream(url.openStream(), SIZE);
                
                // 외부 저장소 경로를 가져와, 해당 경로에 파일을 만들어 output stream을 연다. 
                File externalPath = Environment.getExternalStoragePublicDirectory(Environment.DIRECTORY_DOWNLOADS);
                File externalFile = new File(externalPath, filename);  // filename - test.pdf

                OutputStream output = new FileOutputStream(externalFile);
                
                // input stream으로 read한 데이터를 output stream에 write 한다. 
                byte data[] = new byte[1024];
                int size = 0;
                while ((size = input.read(data)) != -1) {    // read함수 - 데이터를 모두 읽은 경우 -1을 반환
                    output.write(data, 0, size);
                }
                output.flush();
                output.close();
                input.close();
                
                filePath = externalFile.getAbsolutePath();
            
            }catch (Exception e) {
                
            }
        }
        
        // pdf viewer open 
        @Override
        protected void onPostExecute(Boolean result) {
            super.onPostExecute(result);
            // intent로 pdf uri를 넘겨 pdf viewer를 실행한다. 
            Uri fileUri = Uri.parse(filePath);
            Intent pdfViewerIntent = new Intent(Intent.ACTION_VIEW);
            pdfViewerIntent.putExtra(MediaStore.EXTRA_OUTPUT, fileUri);
            pdfViewerIntent.setDataAndType(uri, "application/pdf");
            pdfViewerIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
            startActivity(pdfViewerIntent);
        }
    }
~~~

### 변경 코드
~~~java

    private final CREATE_FILE_CODE = 5050;
    private String pdfDownloadUrl = "https://kongsub.co.kr/download/test.pdf";
    private String filename = "test.pdf";
    private Uri fileUri;
        
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        
        ...
        pdfViewerButton.setOnClickListener(new OnClickListener() {
            @Override
            public void onClick(View v) {
                // saf로 파일을 생성한다.  
                Intent createFileIntent = new Intent(Intent.ACTION_CREATE_DOCUMENT);
                createFileIntent.addCategory(Intent.CATEGORY_OPENABLE);
                createFileIntent.setType("application/pdf");
                createFileIntent.putExtra(Intent.EXTRA_TITLE, filename); // filename - test.pdf
                startActivityForResult(createFileIntent, CREATE_FILE_CODE); 
            }
        });
        ... 
    }

    // 생성한 파일의 Url 결과 받기. 
    @Override
    protected void onActivityResult(int requestCode, int resultCode, Intent data){
        super.onActivityResult(requestCode, resultCode, data);
     
        if(requestCode == CREATE_FILE_CODE){
            // SAF로 생성한 파일 Uri 정보를 받는다. 
            fileUri = intent.getData(); 
            
            // pdf 다운로드 - 비동기처리 
            final DownloadTask downloadTask = new DownloadTask();
            downloadTask.execute();
        
        }else if(requestCode==1){
            ...
        }
    }

    private class DownloadTask extends AsyncTask<String, String, Boolean> {
        @Override
        protected Boolean doInBackground(String... params) {
            try{
                // PDF 다운로드 URL을 URLConnection을 이용하여 input stream을 연다. 
                URL url = new URL(pdfDownloadUrl);      
                URLConnection connection = url.openConnection();
                connection.connect();

                InputStream input = new BufferedInputStream(url.openStream(), SIZE);
                
                // SAF를 이용하여 생성한 file uri를 가지고 output stream을 연다. 
                ParcelFileDescriptor pfd = pfd = getContentResolver().openFileDescriptor(fileUri, "w");   
                FileOutputStream output = new FileOutputStream(pfd.getFileDescriptor());
                
                // input stream으로 read한 데이터를 output stream에 write 한다. 
                byte data[] = new byte[1024];
                int size = 0;
                while ((size = input.read(data)) != -1) {    // read함수 - 데이터를 모두 읽은 경우 -1을 반환
                    output.write(data, 0, size);
                }
                output.flush();
                output.close();
                input.close();
                
            }catch (Exception e) {
                
            }
        }
        
        // pdf viewer open 
        @Override
        protected void onPostExecute(Boolean result) {
            super.onPostExecute(result);
            Intent pdfViewerIntent = new Intent(Intent.ACTION_VIEW);
            pdfViewerIntent.putExtra(MediaStore.EXTRA_OUTPUT, fileUri);
            pdfViewerIntent.setDataAndType(uri, "application/pdf");
            pdfViewerIntent.addFlags(Intent.FLAG_GRANT_READ_URI_PERMISSION);
            startActivity(pdfViewerIntent);
        }
    }
~~~


<br><br><br>

참고
- [Android Developer Document](https://developer.android.com/guide/topics/providers/document-provider?hl=ko)
- [안드로이드 - SAF(Storage Access Framework)로 파일 읽고 쓰는 방법](https://codechacha.com/ko/android-storage-access-framework/)
- [StackOverFlow - Exception 'open failed: EACCES (Permission denied)' on Android](https://stackoverflow.com/questions/8854359/exception-open-failed-eacces-permission-denied-on-android)
- 









