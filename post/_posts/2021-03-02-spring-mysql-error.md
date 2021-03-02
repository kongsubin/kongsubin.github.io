---
layout: post
title: spring과 mysql 연결시 스프링 빈이 없다는 에러 
description: >
  Caused by: org.springframework.beans.factory.BeanCreationException: Error creating bean with name 'jpaMappingContext': Invocation of init method failed; nested exception is javax.persistence.PersistenceException: [PersistenceUnit: default] Unable to build Hibernate SessionFactory; nested exception is org.hibernate.exception.JDBCConnectionException: Unable to open JDBC Connection for DDL execution
sitemap: false
categories: [study]
tags: [error][spring]
---

# spring과 mysql 연결시 스프링 빈이 없다는 에러 

## 1. 첫번째 확인
application.properties확인
> username과 password가 잘 매칭되어 있는지 확인한다. 

> Timezone이 잘 설정되어 있는지 확인한다. Timezone을 꼭 설정해줘야만 에러가 나지 않는다. "serverTimezone=Asia/Seoul"

~~~java
spring.jpa.hibernate.ddl-auto=none
spring.mvc.view.prefix=/WEB-INF/views/
spring.mvc.view.suffix=.jsp
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/database_test?serverTimezone=Asia/Seoul
# DB username
spring.datasource.username=root
# DB password
spring.datasource.password=1234
# JPA 쿼리문을 보여줌
spring.jpa.show-sql=true
~~~


## 2. 두번째 확인 
#### 인텔리제이에 들어가서 우측 상단에 있는 database로 들어간다. 
![](/assets/img/spring/mysql1.png){:.lead data-width="400"}


#### +버튼에서 data source를 클릭 한 후, mysql를 누른다. 
![](/assets/img/spring/mysql2.png){:.lead data-width="400"}

#### 아래의 창이 뜨는데, 아래의 창에서, timezone, user, password를 모두 설정한 뒤, Test Connection을 누른다. 
![](/assets/img/spring/mysql3.png){:.lead data-width="400"}

#### 다음과 같이 Test Connection이 성공했다면 apply and ok를 누른다. 
![](/assets/img/spring/mysql4.png){:.lead data-width="400"}




참고한 블로그 
https://victorydntmd.tistory.com/321