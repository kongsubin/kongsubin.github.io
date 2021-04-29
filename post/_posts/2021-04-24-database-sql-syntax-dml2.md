---
layout: post
title: DML의 문법 - Set, String, Order operations
sitemap: false
description: > 
NULL values, Set operations, String operations, ordering
categories: [study]
tags: [database]
---

# SQL 문법 

- ## NULL values

  : unknown, does not exist, NULL이 수식에 들어가 있으면 그 결과는 항상 NULL이다. 

  - ##### IS NULL : NULL인것만

    ~~~mysql
    # "Find all instructors whose salary is null"
    
    SELECT name
    FROM instructor
    WHERE salary IS NULL;
    ~~~

  - ##### IS NOT NULL : NULL이 아닌 것만

    ~~~mysql
    SELECT name
    FROM instructor
    WHERE salary IS NOT NULL;
    ~~~

- ## Set operations

  : 자동으로 중복을 제거해준다. 만약 중복제거를 원하지 않는다면, ALL을 붙이면됨. 

  : UNIION ALL, INTERSECT ALL, EXCEPT ALL 

  - ##### UNION - 합집합 

    ~~~mysql
    # "Find courses that ran in Fall 2017 or in Spring 2018"
    
    (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    UNION
    (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018)
    ~~~

  - ##### INTERSECT - 교집합 - MySQL에서 지원하지 않음. 

    ~~~mysql
    # "Find courses that ran in Fall 2017 and in Spring 2018"
    
    SELECT LT.course_id
    FROM (SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017)
    			AS LT
    			JOIN (SELECT course_id FROM section WHERE semester = 'Spring' AND year = 2018) 
                AS RT
    			ON LT.course_id=RT.course_id;
    ~~~

  - ##### EXCEPT - 차집합 - MySQL에서 지원하지 않음. 

    ~~~mysql
    # "Find courses that ran in Fall 2017 but not in Spring 2018"
    
    SELECT course_id FROM section WHERE semester = 'Fall' AND year = 2017
    AND course_id NOT IN
    (SELECT course_id FROM section
    WHERE semester = 'Spring' AND year = 2018);
    ~~~

- ## String operations

  : case sensitive, 알파벳 소문자 대문자 구분함. 

  - ##### LIKE : string을 매칭시키는 것 

  - ##### (%) : any substring

    ~~~mysql
    # "Find the names of all instructors whose name includes the substring “ri”"
    
    SELECT name
    FROM instructor
    WHERE name LIKE '%ri%';
    # 이름에 ri가 들어 있는 name가져오기 
    ~~~

  - ##### (_) : any character

  - ##### (\\) : 이건 문자에요 라고 알려주는 것

    ~~~mysql
    LIKE '100 \%' ESCAPE '\'
    
    LIKE '100 \%'
    # 100%가 있는 것들 
    ~~~

  - ##### 예제 모음 

    ~~~
    'Intro%' : intro로 시작하는 모든 애들 
    '%Comp%' : Comp가 들어있는 모든 애들
    '_ _ _' : 3글자인 애들
    '_ _ _ %' : 3글자 이상인 애들 
    ~~~

  - ##### Case Sensitive

    ~~~
    upper, lower 함수를 사용한다. 
    ~~~

    ~~~mysql
    SELECT dept_name 
    FROM department 
    WHERE LOWER(dept_name) like '%sci%';
    ~~~

- ## ordering

    : 순서 

  - ##### ORDER BY 

    ~~~mysql
    "List in alphabetic order the names of all instructors"
    
    SELECT DISTINCT name
    FROM instructor
    ORDER BY name;
    ~~~

  - ##### DESC - 역순 

    ~~~mysql
    ORDER BY name DESC
    ~~~

