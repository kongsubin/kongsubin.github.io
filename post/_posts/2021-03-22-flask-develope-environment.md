---
layout: post
title: 플라스크 개발환경 
description: >
  flask 개발 환경 구축하기!
sitemap: false
categories: [study]
tags: [flask]
---

### 1. 가상 환경 만들기 

conda를 사용하여 가상환경을 만들어 볼 것이다. 

명령어 : conda create --name [가상환경 이름] python==[원하는 python버전]

~~~bash
conda create --name myproject python==3.8
~~~

![](/assets/img/flask/flask-vm/1.png){:.lead data-width="400"}

설치되는 패키지 확인 후 y입력



### 2. 가상환경 리스트 조회

~~~bash
conda info --envs
~~~
![](/assets/img/flask/flask-vm/2.png){:.lead data-width="100"}


### 3. 가상환경 활성화 (activate)

명령어 : source activate [가상환경 이름]

~~~bash
source activate myproject
~~~
![](/assets/img/flask/flask-vm/3.png){:.lead data-width="100"}


### 4. 가상환경 비활성화 

~~~bash
source deactivate
conda deactivate
~~~
![](/assets/img/flask/flask-vm/4.png){:.lead data-width="100"}
![](/assets/img/flask/flask-vm/5.png){:.lead data-width="100"}



### 5. 가상환경 삭제하기 (설치된 패키지까지 모두 삭제)

명령어 : conda remove --name [가상환경 이름] --all

~~~bash
conda remove --name myproject -all
~~~


### 6. 가상환경에 flask 설치 

~~~bash
conda install flask 
~~~



### 7. 설치 확인

~~~bash
project flask --version

Python 3.8.0
Flask 1.1.2
Werkzeug 1.0.1
~~~


****pip위치 정보
~~~bash
(myproject) ➜  project ls -l `which pip`
-rwxrwxr-x  1 yunsubin  staff  261  3 22 15:23 /Users/yunsubin/miniconda3/envs/myproject/bin/pip
(myproject) ➜  project ls -l `which pip2`
-rwxr-xr-x  1 yunsubin  admin  286  6 16  2020 /usr/local/bin/pip2
(myproject) ➜  project ls -l `which pip3`
-rwxrwxr-x  1 yunsubin  staff  261  3 22 15:23 /Users/yunsubin/miniconda3/envs/myproject/bin/pip3
~~~






참고 
https://joytk.tistory.com/14
https://dailyheumsi.tistory.com/33













