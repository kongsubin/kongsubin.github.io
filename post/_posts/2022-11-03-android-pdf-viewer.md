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

## 문제 원인
안드로이드 API 29 (Q) 이상 부터, 내부 저장소에 접근하는 것이 불가능하게 됨.
따라서 외부 저장소에 파일을 쓰고 읽는 것이 거부됨. 

## 해결 방법 
1. MediaStore 사용
2. SAF(Storage Access Framework) 사용

### MediaStore 
- 이미지, 동영상 파일 처리시 사용

### SAF
- txt, pdf 파일 등을 처리시 사용 

PDF는 SAF를 사용하는게 일반적이므로 SAF를 사용하여 PDF Viewer기능을 처리하였다. 

로직
1. PDF 다운로드 URL을 URLConnection을 이용하여 input stream을 연다. 
2. SAF를 이용하여 파일을 생성한 뒤, output stream을 연다. 
3. input stream으로 read한 데이터를 output stream에 write 한다. 
4. intent로 pdf uri를 넘겨 pdf viewer를 실행한다. 

크게 달라진점은, 그냥 외부 저장소에 접근하여 파일을 생성하냐, SAF를 이용하여 파일을 생성하냐의 차이이다. 

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
                
                // 외부 저장소에 파일을 만들어 output stream을 연다. 
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
                startActivityForResult(cIntent, CREATE_FILE_CODE); 
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










