---
title: 'SQL 기본 문법'
date: '2020-03-30'
category: 'SQL'
---

오늘은 SQL에 대해서 알아보려고 한다.
깊게 알아보지는 못하더라도, MySQL을 사용해보면서 기본 문법에 대해서 알아보려고 한다.

##### 아주 간단한 문법만 알아보는 것이기 때문에, 자세한 내용은 구글링을 이용....

# SQL이란?

### Structured Query Language

관계형 데이터베이스 관리 시스템(RDBMS)의 데이터를 관리하기 위해 설계된 특수 목적의 프로그래밍 언어이다

SQL의 특징이라고 말하는 것이 **어떠한 언어보다도 쉽고**, **굉장히 중요**하다는 것이다.

TABLE은 row와 column으로 이루어져 있다.
- **row** : 가로, 행, 데이터 하나하나 그 자체를 의미
- **column** : 세로, 열, 데이터의 타입을 의미


# MySQL설치
[mySQL 사이트](https://dev.mysql.com/downloads/mysql/)

홈페이지로 가서 운영체제에 맞게 다운로드 후에 설치를 진행한다.
![](https://images.velog.io/images/jotang/post/e9d22e88-4de9-4189-874c-1f456d3b139b/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.27.14.png)

`시스템 환경설정에대하여` 를 클릭하면 MySQL이 있는 것을 확인할 수 있다.

start 버튼을 누른 후에, 터미널을 켜고 아래의 명령어를 실행한다.

```js
// 경로 이동
cd /usr/local/mysql/bin/
  
// mysql 시작하기
./mysql -uroot -p
```
`-u 다음 root는 root라는 사용자로 접속하겠다는 뜻`

비밀번호를 입력하고 나면 실행된다.
아래의 화면과 같이 나온다면 성공!

![](https://images.velog.io/images/jotang/post/7b7a746f-6736-4914-8f82-fac1dcf05fa4/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%208.31.44.png)

연관된 데이터(TABLE)들을 그룹화 한 것을 DataBase 또는 **Schema(스키마)** 라고 한다.

이러한 스키마가 많아지게 되고, 보관할 곳이 필요해지는데 스키마르르 보관하는 곳이 **데이터베이스 서버** 이다.

데이터베이스 서버는 자체적인 보안체계를 가지고 있다.

### DATABASE 만들기

데이터베이스 생성
``` 
CREATE DATABASE DB이름;
```

데이터베이스 삭제
```
DROP DATABASE DB이름;
```

데이터베이스의 생성이 잘됐는지 확인
```
SHOW DATABASES;
```

데이터베이스를 사용한다고 하는 명령어
```
USE DB이름
```

# 데이터베이스 만들기
##### 구글에 검색을 믿고 가야합니다.

### 데이터베이스의 특징

- column의 데이터타입을 강제할 수 있다.

- **PRIMARY KEY**
  - 데이터베이스의 **메인 키로 사용**하는 것을 의미
  - 데이터간의 키 중복을 방지한다.
  - 데이터의 식별자로 사용된다.
  - 값은 고유해야한다. (중복은 안된다.)

```js
CREATE TABLE topic(
    -> id INT(11) NOT NULL AUTO_INCREMENT,
    // 숫자 11자리, null은 안돼, 자동으로 증가
    -> title VARCHAR(100) NOT NULL,
    // 글자수는 100자, null은 안돼
    -> description TEXT NULL,
    // text는 글자수가 많음, null ok
    -> created DATETIME NOT NULL,
    // 날짜표시, null은 안돼
    -> author VARCHAR(30) NULL,
    // 글자수는 30자, null ok
    -> profile VARCHAR(100) NULL,
    // 글자수는 100자, null ok
    -> PRIMARY KEY(id));
    // 이 테이블의 고유한 키는 id 
```

# CRUD
### CRUD는 DataBase의 본질이다.

### CREATE
반드시 가지고 있고, 중요한 속성

`DESC topic;` 테이블 생김새를 알 수 있음
![](https://images.velog.io/images/jotang/post/aa70a14d-84f1-46f4-923a-6079ecf3d1c8/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.30.46.png)

```
INSERT INTO topic (title,description,created,author,profile) VALUES('MongoDB','MongoDB is ...',NOW(),'jotang','developer');
```
 - topic 다음에는 **Field값**이 순서대로 들어간다.
 
 - VALUES에는 순서대로 **데이터(내용)**가 들어간다.


### READ
CREATE와 마찬가지고 반드시 가지고 있고, 중요한 속성이다.

`SELECT * FROM topic;` 
입력되어있는 모든 데이터를 가져온다.

![](https://images.velog.io/images/jotang/post/2a9f475e-6796-4fc0-9b86-bdb374579ed6/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.34.30.png)

- `SELECT id,title,created,author FROM topic;`
원하는 데이터만 가져옴

- ` SELECT id,title,created,author FROM topic WHERE author='jotang';`
**WHERE** : 원하는 것만

![](https://images.velog.io/images/jotang/post/9aa8a47e-486d-45ce-9a1d-f1fa219cfa42/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA%202020-03-30%20%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE%209.45.09.png)

- `SELECT id,title,created,author FROM topic WHERE author='jotang' ORDER BY id DESC;`
**ORDER** : 정렬기능 (DESC는 id값이 높은 것 부터)

- ` SELECT id,title,created,author FROM topic WHERE author='jotang' ORDER BY id DESC LIMIT 2;`
**LIMIT** : 데이터의 갯수를 제한 (LIMIT 2는 두 개의 데이터만 출력)

데이터 양이 1억개가 넘어갈 때, 전체를 가져오게되면 컴퓨터는 과부하에 걸린다. 따라서 필요한 정보를 찾아서 볼 줄 알아야한다.


### UPDATE

`UPDATE topic SET description='Oracle is ...', title='Oracle' WHERE id=2;`
**WHERE id=2** => id 2번을 수정하겠다

WHERE를 빠뜨리면 재앙이 올 수 있다.(전부 다바뀜...)

### DELETE
`DELETE FROM topic WHERE id=5;`

DELETE 역시 WHERE은 필수...인생이 망가질 수 있음


##### SQL의 기본 문법에 대해서 알아보았다.
##### 관계형 데이터베이스로 나누고, 테이블을 관리하는 것은 다음에 차근 알아보도록 하겠다.
