---
layout: post
title: DML의 문법 - Set membership (SOME, ALL, EXISTS)
sitemap: false
description: > 
Set membership (SOME, ALL, EXISTS)의 활용 및 예제 
categories: [study]
tags: [database]
---


# Set membership (SOME, ALL, EXISTS)

  - #### IN

    ~~~mysql
    # "Find courses offered in Fall 2017 and in Spring 2018"
    
    SELECT DISTINCT course_id
    FROM teaches
    WHERE semester = 'Fall' AND year= 2017 AND
    			course_id IN (SELECT course_id
    						  FROM teaches
    						  WHERE semester = 'Spring' AND year= 2018);
    										
    # 가을학기이고, 2017년도에 가르친 수업 중, 봄학기이고 2018년도에도 가르친 수업 즉, 
    # course_id를 서브 쿼리에 들어있는 것들만 가져온다. 
    ~~~

    ~~~mysql
    # "Find the total number of unique students who have taken course sections taught by the instructor with ID 10101"
    
    SELECT COUNT(DISTINCT ID)
    FROM takes
    WHERE (course_id, sec_id, semester, year) IN(SELECT course_id, sec_id, semester, year
    											 FROM teaches
    										     WHERE teaches.ID= 10101);
    
    : ID가 10101인 instructor가 가르치는 course를 듣는 학생들의 수를 유니크하게 세어라.  
    ~~~

  - #### NOT IN

    ~~~mysql
    # "Name all instructors whose name is neither “Mozart” nor Einstein”"
    SELECT DISTINCT name
    FROM instructor
    WHERE name NOT IN ('Mozart', 'Einstein');
    
    # 이름 중에, Mozart, Einstein이 이름이 아닌 사람만 보여준다. 
    ~~~

  - #### SOME

    : at least one이라도 있으면 true

    : SOME = IN과 비슷함, attr끼리 OR로 비교연산하는 느낌

    ~~~mysql
    # "Find names of instructors with salary greater than that of SOME (at least one) instructor in the Biology department"
    SELECT DISTINCT T.name
    FROM instructor AS T, instructor AS S
    WHERE T.salary > S.salary AND S.dept name = 'Biology';
    
    # 같은 쿼리 
    SELECT name
    FROM instructor
    WHERE salary > SOME (SELECT salary
    					 FROM instructor
    					 WHERE dept_name = 'Biology');
    
    # Biology학부 소속인 instructor의 salary들 중 하나라도 낮다면 true
    ~~~


  - #### ALL

    : 모든게 만족되어야함. 

    : ALL = NOT IN과 비슷함, attr끼리 AND로 비교연산한다고 생각하면 된다. 

    ~~~mysql
    # "Find the names of all instructors whose salary is greater than the salary of all instructors in the Biology department"
    SELECT name
    FROM instructor
    WHERE salary > ALL (SELECT salary
    					FROM instructor
    					WHERE dept name = 'Biology');
    
    # Biology학부 소속인 instructor의 salary들 모두보다 크면 trues
    ~~~


  - #### EXISTS, is Not Empty? Yes => true, No => false

    : 서브 쿼리의 결과가 존재하면 true를 반환한다. 

    : **EXISTS(**서브 쿼리**)**는 서브 쿼리의 결과가 "한 건이라도 존재하면" **TRUE** 없으면 **FALSE**를 리턴

    ~~~mysql
    SELECT course_id
    FROM section AS S
    WHERE semester = 'Fall' AND year = 2017 AND
    							EXISTS (SELECT *
    									FROM section AS T
    									WHERE semester = 'Spring' AND year= 2018
    										AND S.course_id = T.course_id);
    
    # “Find all courses taught in both the Fall 2017 semester and in the Spring 2018 semester”
    # => 2017년도 가을학기에도 제공되었고, 2018년도 봄학기에도 제공된 수업들의 목록 
    ~~~

  - #### NOT EXISTS, is Empty? Yes => true, No => false

    : NOT EXISTS (서브 쿼리) 는 서브 쿼리의 결과가 "한 건이라도 존재하면" **FALSE** 없으면 **TRUE**를 리턴

    ~~~mysql
    # "Find all students who have taken all courses offered in the Music department"
    SELECT DISTINCT S.ID, S.name
    FROM student AS S
    WHERE NOT EXISTS (SELECT course_id
    				  FROM course
    				  WHERE dept_name = 'Music'
    					AND course_id NOT IN(SELECT T.course_id
    										FROM takes AS T
    										WHERE S.ID = T.ID));
    
    
    # => 학생을 찾는데, Music학부에서 제공되는 모든 수업을 들은 학생들. 
    ~~~

  - #### IN vs EXISTS (NOT IN vs NOT EXIST)

    : 비슷하지만 다르다. 

    EXIST는 Tuple들 간에 존재하는지 안하는지만 확인한다. 속도가 빠르며 TUPLE의 수가 클때 용이하다. 

    IN 뒤에 따라오는 서브 쿼리는 SET으로 취급한다. Tuple의 수가 적을때 용이하다  

  - #### EXCEPT - MYSQL에서 지원하지 않는 문법이다. 

  - ##s## UNIQUE - MYSQL에서 지원하지 않는 문법이다. 

    : UNIQUE evaluates to “true” if a given subquery contains no duplicates

    ~~~mysql
    SELECT T.course_id
    FROM course AS T
    WHERE UNIQUE ( SELECT R.course_id
    				FROM section AS R
    				WHERE T.course_id= R.course_id AND R.year = 2017);
    
    # 서브 쿼리에 duplication이 있다면 false, 없으면 true
    ~~~
