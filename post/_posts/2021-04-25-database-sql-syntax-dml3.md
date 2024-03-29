---
layout: post
title: DML의 문법 - Aggregate functions
sitemap: true
# description: > 
# Aggregate functions - AVG, MIN, MAX, SUM, COUNT, GROUP BY, HAVING
categories: [study]
tags: [database]
published: true
---

# Aggregate functions, aggregation

  - #### AVG: average value

    ~~~sql
    --"Find the average salary of instructors in the Computer Science department"
    
    SELECT AVG(salary)
    FROM instructor
    WHERE dept_name = 'Comp. Sci.';
    
    --Comp. Sci.소속인 instructor의 Salary의 평균을 구하여라 
    ~~~

  - #### MIN: minimum value

    ~~~sql
    SELECT MIN(price) AS min_price 
    FROM products;
    
    --가장 높은 가격 가져오기
    ~~~

    ~~~sql
    SELECT MIN(name) AS min_name 
    FROM products;
    
    --정렬 첫번째 상품명 가져오기
    ~~~

  - #### MAX: maximum value

    ~~~sql
    SELECT MAX(price) AS max_price 
    FROM products;
    
    --가장 높은 가격 가져오기
    ~~~

    ~~~sql
    SELECT MAX(name) AS max_name 
    FROM products;
    
    --정렬 마지막 상품명 가져오기
    ~~~

  - #### SUM: sum of values

  - #### COUNT: number of values

    ~~~sql
    --"Find the total number of instructors who teach a course in the Spring 2018 semester"
    
    SELECT COUNT(DISTINCT ID)
    FROM teaches
    WHERE semester = 'Spring' AND year = 2018;
    
    --2018년도 봄학기의 과목 개수를 구하여라 
    ~~~

    ~~~sql
    --"Find the number of tuples in the teaches relation"
    
    SELECT COUNT (*)
    FROM teaches;
    --teaches의 table의 총 Tuple의 수를 return한다. 
    ~~~

  - #### Group By

    ~~~sql
    --"Find the average salary of instructors in each department"
    SELECT dept_name, AVG(salary) AS avg_salary
    FROM instructor
    GROUP BY dept_name;
    ~~~

  - #### HAVING 

    ~~~sql
    --"Find the names and average salaries of all departments whose average salary is greater than 65000"
    
    SELECT dept_name, AVG(salary) AS avg_salary
    FROM instructor
    GROUP BY dept_name
    HAVING AVG(salary) > 65000;
    
    --HAVING : 조건을 붙여준다 
    ~~~
