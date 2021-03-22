---
layout: post
title: 플라스크 시작하기 
description: >
  flask 처음 시작하기 
sitemap: false
categories: [study]
tags: [flask]
---

## why flask??

### 1. 간결하다. 

   파일 하나로 구성된 짧은 코드만으로도 완벽하게 동작하는 웹을 만들 수 있다. 

   ~~~python
   from flask import Flask
   app = Flask(__name__)
   
   @app.route("/")
   def hello():
       return "Hello World!"
   
   if __name__ == "__main__":
       app.run()
   ~~~

   

### 2. 확장성 있는 설계가 가능하다. 

   프레임워크 내부에 폼과 데이터베이스를 처리하는 기능을 가진 장고에 비해, 플라스크는 폼과 데이터베이스를 처리하는 기능을 내부적으로 가지고 있지 않는다. 

   대신에..!!!! 확장 모듈을 사용하여 이를 보완한다. 

   개발자가 필요한 모듈을 그때그때 추가하여 사용할 수 있어 가벼운 프로젝트에 적합하다.

   

### 3. 자유로운 프레임워크!

   플라스크는 최소한의 규칙만 가지고 있기 때문에 개발의 자유도는 다른 프레임워크보다 높음!

   

참고 
> https://wikidocs.net/81039