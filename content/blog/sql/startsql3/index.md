---
title: 'SQL의 시작_세번째'
date: '2020-06-07'
category: 'SQL'
---

지난 시간에 이어서 다시 한 번 테이블 생성부터 데이터 입력과 조회까지 진행보려고한다.
# Database 생성
테이블을 만들기 전에 먼저 DB를 먼저 만든다.
```sql
create database jotang_db;

use jotang_db;
```
# 테이블 생성
```sql
create table person (
  id int not null auto_increment,
  title varchar(50) not null,
  contents text not null,
  created_at datetime default now() not null,
  updated_at datetime default now() not null,
  deleted_at datetime null,
  primary key ( id ) 
);
```

# 데이터 입력하기
```sql
insert into person (
id, title, contents, created_at, updated_at, deleted_at )
values (
null, 'jotang', '이미지url', 'jotang@jotang.com', 
'010-1111-2222', now(), now(), null);
```

# 데이터 조회하기
```sql
select * from person
````

# 테이블 정보보기
```sql
desc person
```
