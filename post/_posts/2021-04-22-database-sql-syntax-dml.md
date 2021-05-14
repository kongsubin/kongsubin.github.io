---
layout: post
title: DML의 기본 문법 - SELECT, FROM, WHERE
sitemap: false
# description: > 
# DML 무엇인지에, 그리고 기본적인 DML의 문법을 살펴본다. 
categories: [study]
tags: [database]
---

# DML - Data Manipulation Language

=> DB에서 Data를 가져오거나, 변경, 삭제, 추가할 때 사용한다. 

## DML의 기본 문법 

- ### SELECT

  - ##### 기본적으로 ALL이 장착되어 있다.

    ~~~sql
    SELECT ALL dept_name
    FROM instructor
    ~~~

    

  - ##### DISTINCT : 중복을 제외하고 보여준다. 

    ~~~sql
    SELECT DISTINCT dept_name
    FROM instructor;
    ~~~

    

  - ##### asterisk : “all attributes”

    ~~~sql
    SELECT * FROM instructor;
    ~~~

    

  - ##### literal with FROM clause

    ~~~sql
    SELECT 'A' FROM instructor
    --: instructor table의 tuple 개수만큼 A를 찍어준다. 
    --A is printed as much as the number of tuples in the instructor table.
    ~~~

    

  - ##### 기본연산이 가능하다 : +,  –, *,  /

    ~~~sql
    SELECT ID, name, salary/12
    FROM instructor
    ~~~

- ### FROM

  : 뒤에 relations를 나열한다 

  ~~~sql
  SELECT name, course_id
  FROM instructor , teaches
  WHERE instructor.ID = teaches.ID
  
  --"Find the names of all instructors who have taught some course and the course_id"
  ~~~

  - ##### AS (RENAME)

    ~~~sql
    SELECT DISTINCT T.name
    FROM instructor AS T, instructor AS S
    WHERE T.salary > S.salary AND S.dept_name = 'Comp. Sci.'
    
    --"Find the names of all instructors who have a higher salary than some instructor in ‘Comp. Sci.’"
    ~~~

- ### WHERE

  : 조건 연산자이다. 

  - ##### AND, OR, and NOT

  - ##### BETWEEN : range 

    ~~~sql
    SELECT name
    FROM instructor
    WHERE salary BETWEEN 90000 AND 100000
    
    --90000과 100000사이. "Find the names of all instructors with salary between $90,000 and $100,000"
    ~~~

  - 2개의 조건을 동시에 건다. 

    ~~~sql
    SELECT name, course_id
    FROM instructor, teaches
    WHERE (instructor.ID, dept_name) = (teaches.ID, 'Biology');
    
    --instructor.ID=teaches.ID AND dept_name='Biology'
    ~~~

