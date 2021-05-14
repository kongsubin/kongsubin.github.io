---
layout: post
title: Data Definition Language
sitemap: false
# description: > 
# DDL이 무엇인지에, 그리고 문법들은 무엇이 있는지에 대해 살펴본다 
categories: [study]
tags: [database]
---

# DDL - Data Definition Language

>s DB를 정의하고, DB에 table을 만들고, 삭제할 때 사용하는 언어이다. 즉, Table을 정의하고 구조를 나타내는데 사용되는 언어이다. 

- ### CREATE

  ~~~sql
  CREATE TABLE instructor(
          ID CHAR(5),
          name VARCHAR(20),
          dept_name VARCHAR(20),
          salary NUMERIC(8,2)
  )
  --: Table 이름이 instructor이고, 4개의 attr을 가진다. 
  ~~~

  - ##### PK, FK 지정

    ~~~sql
    CREATE TABLE student (
            ID VARCHAR(5),
            name VARCHAR(20) NOT NULL,
            dept_name VARCHAR(20),
            tot_cred NUMERIC(3,0),
            PRIMARY KEY (ID),
            FOREIGN KEY (dept_name) REFERENCES department
    );
    --: ID를 PK로 지정한다. 
    --FK로 department에 있는 dept_name을 가져온다. 
    
    CREATE TABLE student (
            ID VARCHAR(5) PRIMARY KEY,
            name VARCHAR(20) NOT NULL,
            dept_name VARCHAR(20),
            tot_cred NUMERIC(3,0),
            FOREIGN KEY (dept_name) REFERENCES department
    );
    --: 위와 같음. 대신, PRIMARY KEY 를 다른 방식으로 선언함. 
    ~~~

    ~~~sql
    CREATE TABLE takes (
            ID VARCHAR(5),
            course_id VARCHAR(8),
            sec_id VARCHAR(8),
            semester VARCHAR(6),
            year NUMERIC(4,0),
            grade VARCHAR(2),
            PRIMARY KEY (ID, course_id, sec_id, semester, year),
            FOREIGN KEY (ID) REFERENCES student,
            FOREIGN KEY (course_id, sec_id, semester, year) REFERENCES section
    );
    --: PK가 여러개가 될 수 있음. PK는 5개의 attr의 조합으로 이루어진다. 
    ~~~

  - ##### DEFAULT

    : DEFAULT값을 지정해준다. 

    ~~~sql
    CREATE TABLE neighbors(
            name CHAR(30) PRIMARY KEY,
            addr CHAR(50) DEFAULT '123 Sesame St.',
            phone CHAR(16)
    );
    --: DEFAULT 값을 지정해 줄 수 있다. 
    ~~~

    

  - ##### NOT NULL

    : NULL값을 허용하지 않음. 

    ~~~sql
    CREATE TABLE neighbors(
            name CHAR(30) PRIMARY KEY,
            addr CHAR(50) DEFAULT '123 Sesame St.',
            phone CHAR(16) NOT NULL
    );
    -- phone 값을 null을 허용하지 않는다.
    ~~~

- ### ALTER

  : table의 attr을 추가하거나, 삭제할 때 사용한다. 

  - ##### ADD

    ~~~sql
    ALTER TABLE r ADD A D;
    --table r에 이름은 A이고, Domain은 D인 attr(colunm)을 추가한다. 
    
    ALTER TABLE time_slot_backup ADD remark VARCHAR(20);
    --time_slot_backup table에 도메인이 VARCHAR(20)이고, 이름이 remark인 컬럼을 추가한다. 
    ~~~

  - ##### DROP

    ~~~sql
    ALTER TABLE r DROP A;
    --table r에 이름은 A인 attr을 삭제한다. 
    
    ALTER TABLE time_slot_backup DROP remark;
    --time_slot_backup table에 attr 이름이 remark인 컬럼을 삭제한다. 
    ~~~

- ### DROP

  : table을 삭제할 때 사용한다. 

  ~~~sql
  DROP TABLE r;
  --table r을 삭제한다. 
  
  DROP TABLE time_slot_backup;
  --time_slot_backup table을 삭제한다. 
  ~~~

