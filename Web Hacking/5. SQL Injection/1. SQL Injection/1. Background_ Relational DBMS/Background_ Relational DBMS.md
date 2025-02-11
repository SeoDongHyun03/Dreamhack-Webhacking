# Database Management System(DBMS)

## DBMS(데이터베이스 관리 시스템)

**데이터베이스(Database)** : 컴퓨터가 **정보를 기록**하기 위해 사용하는 것(사람들이 필요로 하는 데이터들의 집합)  
**DBMS(DataBase Management System)** : 데이터베이스를 관리하는 애플리케이션  
(예시 : 웹 서버는 데이터베이스를 사용하여 **회원 정보, 글을 저장, 조회, 수정**한다.)  

위의 예시처럼 웹 서버는 **데이터베이스를 관리**하기 위해 **DBMS** 를 사용  

DBMS 는 데이터베이스에 정보를 **삽입, 수정, 삭제**를 한다.  

DBMS는 **관계, 비관계형**으로 분류하고, 아래의 표로 정리했다.  
|종류|설명|대표적인 DBMS|
|---|---|---|
|Relational (관계형)|행과 열의 집합인 **테이블 형식**으로 데이터를 저장함|MySQL, MariaDB, PostgreSQL, SQLite|
|Non-Relational(비관계형)|**키-값** 형태로 값을 저장함|MongoDB, CouchDB, Redis|

# Relational DBMS

## RDBMS

RDBMS(Relational DataBase Management System) 은 1970년에 Codds가 12가지 규칙을 정의하여 생성한 데이터베이스 모델(물론 실제로 구현된 RDBMS는 아래에 있는 최소 2개의 조건을 만족하는 DBMS이다.)  

1. 행과 열의 집합으로 구성된 **테이블** 형식으로 데이터를 **관리**하고, 
2. 데이터를 **조작**할 수 있는 **관계 연산자**를 제공

### 학생 명부

|학번(문자열)|이름(문자열)|생년월일(날짜)|번호(문자열)|
|---|---|---|---|
|20211234|드림이|2020.04.01|010-1337-1337|
|20211235|오리|2016.01.01|070-8864-1337|

### 드림과목 출석부

|학번(문자열)|04/05(ENUM)|04/12(ENUM)|04/19(ENUM)|
|---|---|---|---|
|20211234|출석|공결|출석|
|20211235|지각|결석|출석|

위의 두 테이블이 RDBMS의 예시다.  

여기서 각 학생들의 정보를 확인하기 위해 **학번(고유 키(Unique Key) : 특정 학생을 식별할 수 있는 속성)** 를 이용한다.  

<img src="3.jpg">

이 테이블도 RDBMS의 예시다.  

## SQL

**SQL(Structured Query Language)** : RDBMS의 데이터를 **정의**하고 **질의, 수정** 등을 하기 위해 고안된 언어  
-> 웹 어플리케이션이 **DBMS 와 상호작용**할 때 사용함  

SQL은 아래와 같이 행위에 따라 다양한 구조가 있다.  

|언어|설명|
|---|---|
|**DDL(Data Definition Language)**|데이터를 **정의**하기 위한 언어. **스키마, 데이터베이스**의 **생성/수정/삭제** 등의 행위를 수행|
|**DML(Data Manipulation Language)**|데이터를 **조작**하기 위한 언어. 실제 데이터베이스 내에 **존재하는 데이터**에 대해 **조회/저장/수정/삭제** 등의 행위를 수행|
|**DCL(Data Control Language)**|데이터베이스의 **접근 권한 등의 설정**을 하는 언어.(예 : **GRANT** : 권한을 부여. **REVOKE** : 권한을 박탈, ...)|

## SQL 예시

### DDL

우리가 데이터를 다루기 위해 **데이터베이스, 테이블을 생성**해야한다.  

이 때, **DDL** 을 사용한다.  

```SQL
CREATE DATABASE Dreamhack;
```

위는 **데이터베이스를 생성**하는 쿼리문

```SQL
USE Dreamhack;
# Board 이름의 테이블 생성
CREATE TABLE Board(
	idx INT AUTO_INCREMENT,
	boardTitle VARCHAR(100) NOT NULL,
	boardContent VARCHAR(2000) NOT NULL,
	PRIMARY KEY(idx) # 기본키 : NULL을 허용하지 않는 고유키
);
```

위는 앞서 생성한 데이터베이스에 **Board 테이블을 생성**하는 쿼리문  

### DML

DDL 에서 생성한 테이블에 데이터를 **추가, 조회, 수정**할 때 사용한다.  

```SQL
INSERT INTO 
  Board(boardTitle, boardContent, createdDate) 
Values(
  'Hello', 
  'World !',
  Now()
);
```

위는 Board 테이블에 데이터를 **삽입**하는 쿼리문  

```SQL
SELECT 
  boardTitle, boardContent
FROM
  Board
Where
  idx=1;
```

위는 Board 테이블의 데이터를 **조회**하는 쿼리문  

```SQL
UPDATE Board SET boardContent='DreamHack!' 
  Where idx=1;
```

위는 Board 테이블의 **컬럼(속성) 값을 변경**하는 쿼리문  

# 마치며
## 마치며
- **데이터베이스**: 데이터가 **저장되는 공간**
- **DBMS**: **데이터베이스를 관리**하는 어플리케이션
- **RDBMS**: **테이블 형태**로 저장되는 **관계형 DBMS**
- **SQL**: **RDBMS와 상호작용**할 때 사용되는 언어