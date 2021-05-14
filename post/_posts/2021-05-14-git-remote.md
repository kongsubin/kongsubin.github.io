---
layout: post
title: 리모트 저장소 추가 및 삭제, Pull Fetch, 
sitemap: false
categories: [study]
tags: [git]
---

# 리모트 저장소

리모트 저장소란 인터넷이나 네트워크 어딘가에 존재하는 저장소를 말한다. EX) Github 

## 현재 프로젝트에 등록된 리모트 저장소를 확인

~~~bash
$ git remote -v
~~~

## 리모트 저장소 추가하기

~~~bash
$ git remote add <단축이름> <url> 
~~~



## 리모트 저장소를 Pull 하거나 Fetch 하기

: 저장소에서 데이터를 가져온다. 

### Fetch

~~~bash
$ git fetch <remote>
~~~

: 로컬에는 없지만, 저장소에 있는 모든 데이터를 가져온다. 하지만 자동으로 Merge는 되지 않는다. ㄴ

### pull

~~~bash
$ git pull <remote>
~~~

: 저장소에 있는 데이터를 모두 가져오고 Merge또한 자동으로 된다. 

: fetch + pull 


### 리모트 저장소 살펴보기

: 리모트 저장소의 URL과 추적하는 브랜치를 출력함. 

git pull 명령을 실행할 때 mater 브랜치와 Merge할 브랜치가 무엇인지 보여줌.

~~~bash
$ git remote show origin
~~~


### 리모트 저장소에 Push 하기

: 저장소로 데이터를 보낸다. 

~~~bash
$ git push origin master
~~~







## 리모트 저장소 이름을 바꾸거나 리모트 저장소를 삭제하기

### 이름바꾸기

~~~bash
$ git remote rename pb paul
$ git remote
origin
paul
~~~

: pb ==> paul

### 삭제하기

~~~
$ git remote remove paul
$ git remote
origin
~~~



