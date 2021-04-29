---
layout: post
title: DML의 문법 - Nested Subqueries
sitemap: false
description: > 
Nested Subqueries의 활용 및 예제 
categories: [study]
tags: [database]
---

# Nested Subqueries

  : 쿼리가 다른 쿼리의 sub로 포함되는 것을 말한다.

  : sub 쿼리는 이름을 붙여줘야한다. (AS를 사용)

  - ####  FROM 뒤에

    : subquery의 결과가 relation이어야 한다. 

    ~~~mysql
    SELECT D.dept_name, D.avg_salary
    FROM ( 	SELECT dept_name, AVG(salary) AS avg_salary
    				FROM instructor
    				GROUP BY dept_name) AS D
    WHERE D.avg_salary > 42000;
    
    # avg_salary가 42000이상인 애들
    ~~~

  - #### WHERE 뒤에

    : B <operation> (subquery) 

  - #### SELETE 뒤에 

    : subquery의 결과가 attribute로 single value이어야 한다. 

    쿼리의 결과가 single value인 것을 **Scalar subquery**라고 한다. 

    ~~~mysql
    "List all departments along with the number of instructors in each department"
    SELECT dept_name, (	SELECT COUNT(*)
    					FROM instructor
    					WHERE department.dept_name = instructor.dept_name) AS num_instructors
    FROM department;
    
    # 학부에 소속된 교수님들의 수를 보여주는 쿼리이다. 
    ~~~


  - #### WITH - MYSQL에서 지원하지 않는 문법이다. 

    : 쿼리를 변수로 선언하는 것이다. 

    ~~~mysql
    # "Find all departments with the maximum budget"
    WITH max_budget (value) AS
    		(	SELECT MAX(budget)
    			FROM department)
    SELECT department.dept_name
    FROM department, max_budget
    WHERE department.budget = max_budget.value;
    
    # 위와 같이 쓰면, max_budget으로 변수로서 서브 쿼리를 사용할 수 있다. 
    
    SELECT department.dept_name
    FROM department
    WHERE department.budget = (SELECT MAX(budget) FROM department);
    ~~~

