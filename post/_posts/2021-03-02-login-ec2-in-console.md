---
layout: post
title: EC2서버에 Console로 로그인 하기 
sitemap: false
categories: [study]
tags: [aws]
---
# EC2서버에 Console로 로그인 하기 


<br>
<br>

# 로그인하기
~~~bash
ssh - i "key pem이 있는 파일 경로" ec2-user@public_ip_address 
~~~
![](https://images.velog.io/images/kongsub/post/e97c2a04-9143-47c1-92cf-735281207472/image.png)

로그인이 성공하면 다음과 같은 화면이 뜬다.
![](https://images.velog.io/images/kongsub/post/2b7ac581-3ce0-4850-9606-0a288f38989a/image.png)

이제 ec2서버에 Httpd를 설치해보겠다.


<br>
<br>

# Httpd
: 서버에서 웹 페이지 전송 서비스를 제공해주는 서버 프로그램
1. yum update를 실행한다.
~~~bash
sudo yum update
~~~
![](https://images.velog.io/images/kongsub/post/3af44b3d-9456-4aeb-8ae6-1aaaee8204f4/image.png)


<br>

2. httpd를 설치한다.
~~~bash
sudo yum install httpd
~~~
설치 완료 시 /var/www/ 디렉토리(폴더)가 생성된다.
들어가보자
~~~bash
cd /var/www/
~~~
![](https://images.velog.io/images/kongsub/post/9ed04e2b-8c4c-4e7d-a3fa-5a8fb3905cd1/image.png)

<br>

3. /var/www 디렉토리 권한 설정해준다. 
~~~bash
sudo groupadd www
sudo usermod -a -G www ec2-user
~~~
종료 후 다시 접속
~~~bash
groups
~~~
위의 명령어를 실행하면 www디렉토리가 포함된걸 확인 할 수 있다. 
![](https://images.velog.io/images/kongsub/post/fb73f4b0-23c8-4f48-ad7a-9768502149f3/image.png)

<br>

4. 권한 주기
~~~bash
sudo chown -R root:www /var/www
sudo chown -R ec2-user:apache /var/www
sudo chmod 2775 /var/www
~~~
![](https://images.velog.io/images/kongsub/post/3abe0af6-1439-4fff-8083-40bfea99949e/image.png)

~~~bash
find /var/www -type d -exec sudo chmod 2775 {} \;
find /var/www -type f -exec sudo chmod 0664 {} \;
~~~
![](https://images.velog.io/images/kongsub/post/2bef50f3-ac1f-45eb-a435-df602ac5b700/image.png)

<br>


5. httpd 서비스 실행시키기. 
~~~bash
sudo service httpd start
~~~
![](https://images.velog.io/images/kongsub/post/bdc531c4-0dab-4bfd-9985-831ab0595758/image.png)

<br>
<br>

# 보안 및 그룹 편집하기.
인바운드 규칙에 HTTP를 추가해준다. 
![](https://images.velog.io/images/kongsub/post/5c120ea8-eea4-409b-a60f-4de3ed121212/image.png)

<br>
<br>

# index.html
![](https://images.velog.io/images/kongsub/post/397d6a2d-1e12-42dc-848e-30c6614e1c99/image.png)

![](https://images.velog.io/images/kongsub/post/5c270333-422a-4a95-8363-02f06b1ed7d1/image.png)







