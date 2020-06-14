---
title: 'AWS & EC2 & mySQL'
date: '2020-06-14'
category: 'web'
---

오늘은 AWS ec2 인스턴스에 mysql서버를 설치하여 외부에서 접속하는 과정에 대해서 알아보려고 합니다.

먼저
1. aws ec2 구축
2. ec2에 mysql server 설치
3. mysql 외부접속 설정
4. workbench로 접속해보기

# ec2 구축하기
![](https://images.velog.io/images/jotang/post/2dff6f0e-a83c-41e0-8186-39d43b7ab900/image.png)

인스턴스 시작을 눌러봅니다.

![](https://images.velog.io/images/jotang/post/9b86ffd9-6b91-4598-b7a2-35cd4a46b55b/image.png)

우분투를 선택을 하고, 다른 설정들은 일단 넘어가도록 합니다.

![](https://images.velog.io/images/jotang/post/a0551d59-7a98-499c-beda-17690d040a44/image.png)

기존의 key pair가 있다면, 기존에 있는 key pair로 진행을 하고, 없다면 key pair를 생성하고, 다운로드 한 후에 진행합니다.
**(pem 파일은 잘 관리하여야합니다!!)**

ec2는 무사히 구축하였으니, ec2에 접속하여 mysql server를 설치해보도록 하겠습니다.

# mySQL server 설치하기

터미널을 켜고 아까 다운로드 했던 pem 파일이 있는 디렉토리로 이동을 합니다.

먼저 pem 파일의 권한을 변경을 진행해주고

>chmod 400 [다운받은 pem 이름].pem

그리고 아래의 명령어를 통해 ec2에 접속합니다.
>ssh -i [다운받은 pem 이름].pem ubuntu@[public IP]

![](https://images.velog.io/images/jotang/post/1f8b3022-a3e9-4275-bb78-ba2873325c02/image.png)

접속 완료!!

우분투에서 패키지 관리를 하는 apt를 업데이트 해줍니다
```
$sudo apt update
```

apt를 이용하여 mysql 설치를 진행합니다.
```
$sudo apt install mysql-server
```
설치 과정에서 y/n 물으면 y해줍니다.

```
$dpkg -l | grep mysql-server
````
을 통해 잘 설치되었는지 확인합니다.

![](https://images.velog.io/images/jotang/post/8474d18d-7289-4f01-b106-55e656e25065/image.png)

```
$sudo mysql -uroot -p
````
를 통해 mysql에 접속도 해봅니다.
(password 입력은 그냥 enter를 통해 넘어갑니다.)

접속이 안된다면 
```
$sudo systemctl restart mysql.service
````
를 통해서 재시작을 해줍니다.


# 외부접속 설정
디렉토리를 이동합니다.
```
cd /etc/mysql/mysql.conf.d
```

![](https://images.velog.io/images/jotang/post/cf03b11e-583b-411b-9245-e7514069058b/image.png)

`mysqld.cnf` 파일에서 bind-address를 찾아 변경해줍니다.
```
$sudo vi mysqld.cnf
```
알파벳 o를 누르면 수정을 할 수 있습니다.

![](https://images.velog.io/images/jotang/post/b1e6a6b5-1280-4609-a95c-c1c3f6530136/image.png)

#을 붙여서 주석처리를 하고, 
`esc -> :wq` 를 통해 수정화면에서 나옵니다.

이제 외부에서 접속할 계정을 만들기 위해 mysql에 접속하여 계정을 생성하고, 권한을 부여해줍니다.

```sql
create user '유저이름'@'%' identified by '비밀번호';
```
```sql
grant all privileges on *.* to '유저이름'@'%' with grant option;
```

이후 
```
$sudo systemctl restart mysql.service
```
를 통해 restart해줍니다.

마지막으로 aws로 가서 보안그룹에서 3306 포트에 접속할 수 있도록 추가해줍니다.
![](https://images.velog.io/images/jotang/post/2aa0b0ee-223b-475f-a424-98bdaaef15de/image.png)

규칙추가를 누르고

![](https://images.velog.io/images/jotang/post/76e55810-ae71-4e99-a4fa-03fbe472bbf0/image.png)

![](https://images.velog.io/images/jotang/post/c73e185b-9034-4826-a08a-e6ac9ea6a119/image.png)

mysql/aurora 를 선택해줍니다.

그리고 규칙추가!!

workbench로 확인하기전에 테이블도 한번 만들어 봅니다~

# workbench로 접속하기
workbench를 실행하고, + 버튼을 눌러 정보를 입력한다.
* hostname에는 public IP를 넣는다
* username에는 아까 생성한 유저이름을 넣고, 아래에 password까지 입력한다.

![](https://images.velog.io/images/jotang/post/c9f06ee0-9ea5-45b6-b7c0-8730c4881f83/image.png)

그리고 test connection을 해본다.

![](https://images.velog.io/images/jotang/post/bbb77e27-6efe-4b50-8314-bac9c23f9c09/image.png)

떴다면 성공!!!!

상단에
connection name을 정해주고 ok를 누른다!

![](https://images.velog.io/images/jotang/post/c635e950-0c25-4946-afd5-bfaa1fda3871/image.png)

접속을 해서 아까 만든 테이블이 조회가 잘되는지 insert가 잘되는지 확인해보며 마무리한다.

![](https://images.velog.io/images/jotang/post/cb7496ad-dc1d-44e4-a695-bd113e9ffc9b/image.png)


