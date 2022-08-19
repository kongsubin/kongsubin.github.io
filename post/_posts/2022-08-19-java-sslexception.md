---
layout: post
title: javax.net.ssl.SSLException Received fatal alert protocol_version
sitemap: false
categories: [study]
tags: [error, java]
---

# javax.net.ssl.SSLException Received fatal alert protocol_version


# 문제 상황
프로젝트 운영 중에, 외부 API 통싱할 때, 아래의 에러가 발생하는 현상이 있었다. 

> javax.net.ssl.SSLException: Received fatal alert: protocol_version

<br>
<br>

# 문제 원인
찾아보니, http 통신할 때, 요구하는 SSL 프로토콜버전을 충족시키지 못하여 나는 에러였다.  
<br>

자바 버전에 따라서 기본적으로 지원해주는 SSL 프로토콜은 다음과 같다. 


|JAVA version|SSL/TLS Default|Other Supported Version|
|------|---|---|
|Java6 | TLS1.0 | TLS1.1, SSLv3.0* |
|Java7 | TLS1.0 | TLS1.1, TLS1.0, SSLv3.0* |
|Java8 | TLS1.2 | TLS1.1, TLS1.0, SSLv3.0* |


우리 프로젝트는 자바 버전 6을 사용하고 있고, 요청하려는 외부 API는 TLS1.2 수준을 요구한다. 


<br>
<br>


# 해결 방법

인터넷에 검색해보니, 해결방법이 크게 3가지가 나왔다. 
1. 프로젝트 초기화 클래스에 버전 세팅해주기
~~~java
java.lang.System.setProperty("https.protocols", "TLSv1,TLSv1.1,TLSv1.2");
~~~
2. Tomcat 등 WAS 설정 파일에 SSL 버전 설정해주기
~~~sh
-Dhttps.protocols=TLSv1,TLSv1.1,TLSv1.2
~~~
3. 자바 버전 8이상으로 업데이트  

가장 간단해 보인 1번 방법을 사용하였지만, 적용이 되지 않았다. (아마 API호출하는 부분이 따로 동작하기 때문이라고 예상함...)

따라서 외부 API호출 시 아래의 코드를 활용하여 에러를 해결하였다. 
~~~java
//api 호출 시 사용하는 HTTP
{
    ...
    CloseableHttpResponse httpclient = null;
    httpclient = buildHttpClient();
    ...
}

private static CloseableHttpClient buildHttpClient() {
    System.setProperty("https.protocols", "TLSv1.2");
    SSLContext ctx = null;
    try {
        ctx = SSLContexts.custom().useProtocol("TLSv1.2").build();
    } catch (KeyManagementException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    } catch (NoSuchAlgorithmException e) {
        // TODO Auto-generated catch block
        e.printStackTrace();
    }
    CloseableHttpClient httpClient = HttpClientBuilder.create().setSslcontext(ctx).build();
    return httpClient;
}
~~~


<br>
<br>
<br>
<br>
<br>
<br>
<br>


[스택오버플로우](https://stackoverflow.com/questions/16541627/javax-net-ssl-sslexception-received-fatal-alert-protocol-version)

[참고한 블로그](https://juns-lee.tistory.com/entry/Java-SSLTLS-%EC%A7%80%EC%9B%90-%EB%B2%84%EC%A0%84%EA%B3%BC-%EB%94%94%ED%8F%B4%ED%8A%B8-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C-%EB%B3%80%EA%B2%BD)