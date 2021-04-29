---
layout: post
title: DML의 문법 - Modification of database(INSERT, UPDATE, DELETE)
sitemap: false
description: > 
Modification of database(INSERT, UPDATE, DELETE)의 활용 및 예제 
categories: [study]
tags: [database]
---

# Modification of database

- ## INSERT

    : 새로운 Tuple을 집어 넣는다. 

    - FK를 INSERT하기 위해서는 같은 값인 PK가 존재해야만 한다. 

    ~~~mysql
    INSERT INTO course
    VALUES ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
    
    --------------------------------------------------------------------------
    
    INSERT INTO course (course_id, title, dept_name, credits)
    VALUES ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
    
    --------------------------------------------------------------------------
    
    INSERT INTO student
    VALUES ('3003', 'Green', 'Finance', null);
    # => 값을 null로 넣겠다는 의미 
    ~~~

    ~~~mysql
    # "Make each student in the Music department who has earned more than 144 credit hours an instructor in the Music department with a salary of $18,000"
    
    INSERT INTO instructor
    		SELECT ID, name, dept_name, 18000
    		FROM student
    		WHERE dept_name = 'Music' AND total_cred > 144;
    
    # 쿼리 결과를 받아서 INSERT도 가능하다. 음대이고, 학점을 144 이상 들은 사람을 교수로 위임한다. 
    
    ~~~

  - ## UPDATE

    : 새로운 tuple이 아니라, 특정 값만 바꾸고 싶다.

    ~~~mysql
    # "Give a 5% salary raise to all instructors"
    UPDATE instructor
    SET salary = salary * 1.05
    
    # salary 5%인상시켜준다. 
    ~~~

    ~~~mysql
    # "Give a 5% salary raise to instructors whose salary is less than average"
    UPDATE instructor
    SET salary = salary * 1.05
    WHERE salary < (SELECT AVG(salary) FROM instructor);
    
    # salary 5%인상시켜주는데 평균 salart보다 낮은 사람만 인상시켜준다. 연산은 뒤에서 앞으로 진행이 된다. 
    ~~~

    ~~~mysql
    # "Recompute and update tot_creds value for all students"
    UPDATE student S
    SET tot_cred = (SELECT SUM(credits)
    								FROM takes, course
    								WHERE takes.course_id = course.course_id AND
    											S.ID= takes.ID AND
    										takes.grade <> 'F' AND
    											takes.grade IS NOT NULL);
    										
    # total_credit (전체들은 학점) 업데이트 하는 것 
    ~~~

    - ##### CASE Statement for Conditional Update 

      ~~~mysql
      # "Increase salaries of instructors whose salary is over $100,000 by 3%, and all others by a 5%"
      UPDATE instructor
      SET salary = CASE
      								WHEN salary <= 100000 THEN salary * 1.05
      								ELSE salary * 1.03
      							END
      # salary <= 100000 성립시, THEN 절 실행, 아닌 경우 ELSE 문 실행.
      
      # 같음 
      UPDATE instructor
      SET salary = CASE
      							WHEN salary > 100000 THEN salary * 1.03
      								ELSE salary * 1.05
      						END
      ~~~

    ~~~
      
    ~~~

  - ## DELETE 

    ~~~mysql
    "Delete all instructors"
    DELETE FROM instructor;
    # instructor안에 있는 TUPLE을 모두 다 지움
    
    --------------------------------------------------------------------------
    
    "Delete all instructors from the Finance department"
    DELETE FROM instructor
    WHERE dept_name = 'Finance';
    # WHERE 절 조건을 만족하는 Tuple을 다 지움
    
    --------------------------------------------------------------------------
    
    "Delete all tuples in the instructor relation for those instructors
    associated with a department located in the Watson building"
    DELETE FROM instructor
    WHERE dept_name IN (SELECT dep_name
    									FROM department
    										WHERE building = 'Watson');
    # building 이름이 Watson인 학부에 속한 모든 instructor를 지우기 
    
    --------------------------------------------------------------------------
    
    # "Delete all instructors whose salary is less than the average salary of instructors"
    DELETE FROM instructor
    WHERE salary < (SELECT AVG (salary)
    								FROM instructor);
    ~~~

