---
title: 'SQL의 시작_두번째'
date: '2020-05-31'
category: 'SQL'
---

# SQL이란?
**Structured Query Language** 의 약자.
RDBMS와 소통하는 프로그래밍 언어이다.

### 질의 언어(query)
DB를 상대로 데이터를 조회, 입력, 수정, 삭제하기 위한 모든 것은 질의에 속한다.

### 집합적 언어
SQL은 데이터를 상대하는 언어이다. 데이터들은 테이블에 저장되어 있고, 이러한 테이블들은 목적과 성격에 맞는 데이터를 모아 놓은 데이터 저장소이다. 
SQL은 특정 조건에 부합한다면 이를 충족하는 데이터 전체를 읽거나 삭제하거나 수정, 입력하는 기능을 수행한다. 즉 데이터를 한 건씩 처리하는 것이 아니라, 조건에 맞는 데이터 전체를 한꺼번에 처리하기 때문에 **집합적 언어**라고 한다.

# SQL의 종류
## 1. DDL
**Data Definition Language, 데이터 정의어**

RDBMS에는 테이블 외에도 뷰, 인덱스, 시퀀스 등 여러 데이터베이스 객체가 있다. 이러한 객체들을 생성, 삭제, 수정하는데 사용하는 SQL.

* **CREATE** : 객체를 생성

* **DROP** : 객체를 삭제

- **ALTER** : 객체를 변경, 수정

- **TRUNCATE TABLE** : 테이블에 있는 모든 데이터를 삭제

- **RENAME** : 객체의 이름을 변경

## 2. DML
**Data Manipulation Language, 데이터 조작어**

가장 많이 사용되는 SQL문

- **SELECT** : 테이블이나 뷰에서 데이터를 조회

- **INSERT** : 데이터를 입력

- **UPDATE** : 기존에 저장되어있는 데이터를 수정

- **DELETE** : 테이블에 있는 데이터를 삭제

- **MERGE** : 조건에 따라 INSERT 나 UPDATE를 수행


#### TRUNCATE TABLE 와 DELETE 차이
TRUNCATE TABLE은 DDL로 테이블에 있는 모든 데이터를 삭제하고, 그걸로 끝이 난다. 실수로 지우더라도 되돌릴 수 없다.
반면, DELETE는 조건에 맞는 데이터를 선별하여 삭제하므로, 잘못 삭제되었다고 해도 삭제 이전 시점으로 복원이 가능하다. 따라서 데이터를 삭제할 때는 DELETE문을 사용한다.

## 3. TCL
**Transaction Control Language, 트랜잭션 제어어**

- **COMMIT** : DML로 변경된 데이터를 DB에 적용

- **ROLLBACK** : DML로 변경된 데이터를 변경 이전 상태로 되돌림

## 4. DCL
**Data Control Language, 데이터 제어어**

- **GRANT** : 객체에 대한 권한을 할당

- **REVOKE** : 객체에 할당된 권한을 회수

# 테이블 생성!
DDL의 CREATE문을 사용해 테이블을 생성할 수 있다
```sql
CREATE TABLE table_name(
	column_name1 datatype [NOT] NULL,
    	column_name2 datatype [NOT] NULL,
        ...
        PRIMARY KEY ( column_list)
	);
```
- table_name : 테이블 이름을 명시
- column_name1, column_name2 : 칼럼의 이름을 명시
- datatype : 칼럼의 데이터 유형을 명시
- [NOT] NULL : 해당하는 칼럼의 null을 허용할지 여부를 명시, 생략하면 허용한다는 뜻

#### 테이블 이름은 누가봐도 어떤 용도의 테이블인지 알 수 있도록 네이밍을 해야한다.

# 칼럼의 데이터형
![](https://images.velog.io/images/jotang/post/b13f6ee6-7eb3-4f7f-ab5d-08a3014b331c/image.png)

대표적인 데이터타입은
`문자형`, `숫자형`, `날짜형`이다.

### 문자형
- CHAR(n) : 고정 길이 문자, 최대 2000byte
- VARCHAR2(n) : 가변 길이 문자, 최대 4000byte

n은 숫자를 의미, 예를 들어 VARCHAR2(10)이라면 VARCHAR2형 문자를 10byte 사용하겠다는 뜻.

CHAR와 VARCHAR2의 차이는, 한 글자만 넣게 되더라도, CHAR는 1byte가 아닌 10byte를 차지하는 반면, VARCHAR2는 1byte만 차지한다.
즉, VARCHAR2는 실제 데이터에 따라 크기가 정해지는 반면, CHAR는 테이블 생성 시 설정한 크기로 고정된다.

따라서, 특별한 경우를 제외하고는 VARCHAR2로 만든다.

### 숫자형
- NUMBER[(p, [s])] : p(1~38, defalut는 38)와 s(-84~127, defalut는 0)는 십진수 기준, 최대 22byte.

NUMBER형으로 데이터 칼럼을 만들면 웬만한 숫자는 모두 저장할 수 있다고 한다.

위에서 보이는 [ ]대괄호는 생략이 가능하다는 표시, 크기를 지정하지 않으면 38자리 숫자까지 입력이 가능하다. 38자리라는 것은 유효숫자 개수가 38이라는 것을 의미

s역시 생략이 가능하고, 기본값은 0. s는 소수점 이하 유효숫자 자리 수를 지정해준다. 고정 소수점 숫자를 지정할 때 사용하고, p만 명시한다면, 부동 소수점 숫자를 사용하는 것

### 날짜형
- DATE : BC 4712년 1월 1일부터 9999년 12월 31일까지 년, 월, 일, 시, 분, 초 입력 가능

### NULL
데이터가 없음을 의미.

해당 칼럼에 값이 들어가지 않을 수 있다고 정의하기 위함. NOT NULL이라면 반드시 해당 칼럼에는 값이 들어가야한다

### PRIMARY KEY
테이블당 1개만 만들 수 있다.
칼럼 1개로 만들 수 있고, 여러 칼럼을 결합하여 만들 수도 있다. 

기본 키를 구성하는 칼럼이 1개뿐인 경우에는 칼럼 정의 시 PRIMARY KEY 구문을 넣어 생성할 수 있다.
```sql
Column_name1 VARCHAR(10) NOT NULL PRIMARY KEY,
...
```

기본 키 칼럼에는 반드시 NOT NULL을 명시해야한다. 기본 키에 속하는 칼럼이 여러 개인 경우, 모든 칼럼을 정의한 뒤 마지막에 다음과 같이 명시한다.
```sql
...
PRIMARY KEY ( column1, column2, ...)
````


# 테이블 생성 실습
```sql
// 경로 이동
cd /usr/local/mysql/bin/
  
// mysql 시작하기
./mysql -uroot -p
```
![](https://images.velog.io/images/jotang/post/a1fd17ba-2962-466d-8e46-a70f37aaa06a/image.png)



to be continue