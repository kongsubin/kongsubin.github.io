---
layout: post
title: Python Server 구동시키기 + npm 모니터링
sitemap: false
categories: [study]
tags: [flask]
---
# Python Server 구동시키기 + npm 모니터링

<br>
<br>

## Python Server
Python Server의 구조는 다음과 같이 구성되어 있다. 

~~~bash
tree
.
├── __init__.py
├── static
│   ├── e.json
│   └── index.html
├── template
└── test.py
~~~

<br>
<br>

~~~bash
vi test.py
~~~

웹 서버를 구동하기 위해서, 다음을 import해준다. 
그리고, 기본 값으로(root) index.html을 설정해준다. 
~~~bash
from flask import redirect, url_for, send_from_directory, render_template
~~~
![](https://images.velog.io/images/kongsub/post/c55d986d-5910-4b7f-973e-6873bc4673bf/image.png)
![](https://images.velog.io/images/kongsub/post/30f9eed9-7ebe-41e4-8706-75b5bb078032/image.png)

파이썬 웹 서버를 구동시키는 코드이자 mysql db와 연결된 flask python 코드이다. 

test.py 코드 
~~~python
import pymysql
from flask import Flask
from flask import request
from flask import jsonify
from flask import redirect, url_for, send_from_directory, render_template
import json

conn = pymysql.connect(host='^^',
                        user='^^',
                        password='^^',
                        db='MOA',
                        charset='utf8')
curs = conn.cursor()

if conn.open:
    print('connected');

app = Flask(__name__)

# Routes
@app.route('/', methods=['GET'])
def root():
    return app.send_static_file('index.html')

@app.route('/')
def static_prox(path):
    return app.send_static_file(path)

@app.route('/test', methods = ['POST'])
def postJsonHandler():
    print (request.is_json)
    content = request.get_json()
    print (content)
    id = content['id']
    print ("#####id:",id)
    sql = "select * from users"
    curs.execute(sql)
    rows = curs.fetchall()
    print(rows)
    data = {'id':'yebin'}
    return content

app.run(host='0.0.0.0', port= 3306)
~~~
여기서 3306포트는 mysql 포트번호이다. 

<br>
<br>

## 파이썬 서버 계속 실행시켜놓기

### 1. nvm설치하기
~~~bash
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash
~~~

### 2. nvm 실행
~~~bash
. ~/.nvm/nvm.sh
~~~

### 3. npm 설치
~~~bash
nvm install 10.15.3
~~~

### 4. pm2설치
~~~bash
npm install -g pm2
~~~

### 5. 파이썬 서버 실행시키키
~~~bash
pm2 start -x --interpreter /usr/bin/python3.5 test.py
~~~
![](https://images.velog.io/images/kongsub/post/70b65c57-9f66-41e4-8b7c-3e5d7c9ecc20/image.png)

### 실행시키고 있는 서버 리스트 확인하기
~~~bash
pm2 list
~~~
![](https://images.velog.io/images/kongsub/post/de5deb93-3171-4679-a52b-3bfb052657c0/image.png)

### 모니터 켜기
~~~bash
pm2 monit
~~~
![](https://images.velog.io/images/kongsub/post/fcc7a734-6641-4287-955a-c5802a58639f/image.png)

### 서버 멈추기
~~~bash
pm2 stop test
~~~

### 서버 죽이기
~~~bash
pm2 delete
~~~

<br>
<br><br>
<br><br>
<br><br>
<br>



















