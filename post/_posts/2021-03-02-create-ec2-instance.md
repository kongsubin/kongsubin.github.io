---
layout: post
title: AWS EC2 Instance 생성하기 
sitemap: false
categories: [study]
tags: [aws]
---
# AWS EC2 Instance 생성하기 

# AWS EC2란?
: 아마존 웹 서비스는 아마존닷컴의 클라우드 컴퓨팅 사업부다. 아마존 웹 서비스는 다른 웹 사이트나 클라이언트측 응용 프로그램에 대해 온라인 서비스를 제공하고 있다.

# 로그인 하기
1. 콘솔에 로그인하기를 누른다.
![](https://images.velog.io/images/kongsub/post/407d8c86-18a4-494b-85f5-b47ce375364d/image.png)

2. 루트 사용자 누르고 로그인하기. 구글 계정으로 로그인 가능하다.
![](https://images.velog.io/images/kongsub/post/a52ae5f4-90d0-456a-8fab-5ed79eaa63a5/image.png)

# 설정
1. 리젼 설정
서울로 설정해준다.
![](https://images.velog.io/images/kongsub/post/f1b7661a-2379-4aa2-97c7-b5e88f190383/image.png)

2. "ec2"를 검색창에 검색한다.
![](https://images.velog.io/images/kongsub/post/9a845cc5-b05f-4f99-85a5-08cfb46f0f33/image.png)

# 인스턴스 생성
1. 인스턴스 생성을 누른다.
![](https://images.velog.io/images/kongsub/post/4a3437c4-1a40-49a1-a967-44042e7c99b4/image.png)

2. 서버 운영체제를 선택한다.
![](https://images.velog.io/images/kongsub/post/538c44e4-800d-439a-a2dd-04e3f407ae1e/image.png)

모아의 경우, 아래의 운영체제를 선택하였다. 
![](https://images.velog.io/images/kongsub/post/dddb69c0-9e08-4c53-b51c-6dd7cad4a1b8/image.png)

3. 1년 무료 티어를 선택하고 "검토 및 시작버튼"을 누른다. 
![](https://images.velog.io/images/kongsub/post/edce7d61-46d8-4f7b-becf-7b7585c251e8/image.png)

4. 시작하기 버튼을 눌러 인스턴스를 생성한다.
![](https://images.velog.io/images/kongsub/post/71492dec-d754-439d-98f5-cdb555130ab6/image.png)

5. 키 페어 생성
시작하기 버튼을 누르면 다음과 같이 키 페어 생성이 뜬다.
기존 키페어를 사용하고 싶은 경우, 기존 키 페어 선택을 누르면 된다.
모아의 경우 새 키 페어를 생성하였다.

![](https://images.velog.io/images/kongsub/post/758797f3-adfa-4ef1-b5bd-c14fda23ad47/image.png)

키 페어 이름을 입력하고, 키 페어 다운로드를 한다.
키 페어는 인스턴스 생성하는 과정에서만 다운받을 수 있으므로, 다운로드 후 소실 되지 않도록 잘 관리해야한다.
그리고 타인에게 공유하거나 인터넷에 배포하는 것을 조심해야한다. 

5. 인스턴스 시작버튼을 누르면 다음과 같이 화면이 뜬다.
![](https://images.velog.io/images/kongsub/post/e0b4b318-066e-4eb6-99ac-64bc5124247f/image.png)

# 보안 및 그룹 편집
1. 보안 그룹에 들어가준다.
![](https://images.velog.io/images/kongsub/post/9a8a539b-e820-487c-a3e1-c6905220bbf1/image.png)

2. 편집하고자 하는 보안 그룹을 선택한 후 인바운드 규칙 편집을 눌러준다.
![](https://images.velog.io/images/kongsub/post/f16ed583-af1a-4dc0-9b3d-0933cd1117cd/image.png)

3. 추가하고 싶은 규칙을 추가해 준다.
![](https://images.velog.io/images/kongsub/post/a1aa0b24-9272-4f86-9a12-96f42144c0af/image.png)

4. 규칙 저장 버튼을 누른다.
![](https://images.velog.io/images/kongsub/post/60c760d5-bf36-42b8-951a-843f1f36cb80/image.png)

# 인스턴스 시작 및 중지
마우스 오른쪽 버튼을 눌러 인스턴스를 시작 및 중지시킬 수 있다.
![](https://images.velog.io/images/kongsub/post/bfd50a6b-286d-4d22-8e6c-b6bee39e2a4b/image.png)

