---
layout: post
title: JAVA ERROR
sitemap: true
categories: [study]
tags: [java]
published: true
---
# JAVA ERROR

## Java에서 자주 발생하는 에러와 해결방안들...

### 1. Cannot find symbol or connot resolve symbol 
> 지정된 변수나 메서드를 찾을 수 없다는 뜻이다.
선언되지 않는 변수나 메서드를 사용하거나 변수 또는 메서드의 이름을 잘못 사용한 경우, 발생한다.

### 2. ';' expected
> 세미콜론을 누락했다는 뜻이다.

### 3. Exception in thread "main" java.lang.NoSuchMethodError: main
> main 메서드를 찾을 수 없다는 뜻이다.
실제로 클래스 내에 main메서드가 존재하지 않거나, 
메서드의 선언부 "public static void main(String[] args)"에 오타가 존재하는 경우에 발생한다.

### 4. Exception in thread "main" java.lang.NoClassDefFoundError:Hello
> Hello라는 크래스를 찾을 수 없다는 뜻이다. 
만약 Hello.java가 정상적으로 컴파일 되었다면, 클래스파일 "Hello.class"가 있어야 한다. 
그런데 클래스파일이 존재하는데 똑같은 에러가 계속 난다면 클래스 패스(Classpath)의 설정이 바르게 되었는지 확인해야한다. 

### 5. illegal start of expression 
> 문법적 오류가 있다는 것이다.
괄호"(" or "{"를 열고 닫지 않거나, 수식이나 if문, for문 등에 문법적 오류가 있을 때,
또는 public이나 static과 같은 키워드를 잘못 사용한 경우에 발생한다. 

### 6. class, interface, or enum expected
> 키워드 class나 interface, ecnum이 없다이지만, 
주로 괄호 "{" or "}"의 개수가 일치하지 않을 때 발생하는 에러이다. 



<br><br>
<br><br><br>
<br><br><br>
<br>


참고 서적 : 자바의 정석 





