---
layout: post
title: SQL) MySQL
date: 2024-05-05 14:59 +0900
description: 
image: https://github.com/Kingsong97/Kingsong97.github.io/assets/161429740/6ef23fcf-9f44-4b77-bd22-684197eaee75
category: MySQL
tags: MySQL
published: true
sitemap: true
---

MySQL은 관계형 데이터베이스 관리 시스템(RDBMS) 중 하나로, 데이터를 구조화하여 저장하고 관리하는 소프트웨어입니다. MySQL은 SQL(Structured Query Language)을 사용하여 데이터베이스와 상호 작용합니다. SQL은 데이터를 검색, 삽입, 수정, 삭제 등을 할 수 있게 해주는 표준 프로그래밍 언어입니다.

# MySQL

**설치하기**

MySQL을 설치하려면 MAMP(MacOS, Apache, MySQL, PHP)와 같은 솔루션 스택을 사용할 수 있습니다. MAMP를 사용하면 쉽게 MySQL을 설치하고 로컬 환경에서 웹 개발을 할 수 있습니다.

**실행하기**
터미널을 열고 다음 명령어를 입력하여 MySQL을 실행할 수 있습니다.
````sql
cd /Applications/MAMP/Library/bin
./mysql -uroot -proot
````
### SQL 개념
SQL은 데이터베이스를 조작하기 위한 언어로, 구조화된 질의 언어입니다. 주요한 특징은 다음과 같습니다.

관계형 데이터베이스(Relational Database)를 조작하는 프로그래밍 언어
ANSI와 ISO에서 관계형 데이터베이스 표준 언어로 체택됨
비절차적 언어로, 프로그램에 처리 방법을 기술하지 않아도 됨
SQL 명령어 종류
SQL 명령어는 크게 데이터 정의 언어(DDL), 데이터 조작 언어(DML), 데이터 제어 언어(DCL)로 구분할 수 있습니다.

#### 데이터 정의 언어 (DDL)
- CREATE: 데이터베이스, 테이블, 뷰 등을 생성
- ALTER: 데이터베이스, 테이블의 구조를 변경
- DROP: 데이터베이스, 테이블, 뷰 등을 삭제
#### 데이터 조작 언어 (DML)
- SELECT: 테이블에서 데이터를 검색
- INSERT: 테이블에 새로운 데이터를 삽입
- UPDATE: 테이블에 저장된 데이터를 수정
- DELETE: 테이블에서 저장된 행을 삭제
#### 데이터 제어 언어 (DCL)
- COMMIT: 데이터의 변경을 확정
- GRANT: 데이터를 조작하는 권한을 사용자에게 부여
- REVOKE: 데이터를 조작하는 권한을 해제
- ROLLBACK: 데이터의 변경을 취소
#### 데이터베이스 관리
- MySQL에서 데이터베이스를 생성하고 관리하는 명령어는 다음과 같습니다.

### 데이터베이스 보기
````sql
SHOW DATABASES;
데이터베이스 생성
````

````sql
CREATE DATABASE 이름;
데이터베이스 삭제
````

````sql
DROP DATABASE 이름;
데이터베이스 선택
USE 이름;
테이블 관리
MySQL에서 테이블을 생성하고 관리하는 명령어는 다음과 같습니다.
````

테이블 생성
````sql
CREATE TABLE 테이블명 (
    열1 데이터_유형 [속성],
    열2 데이터_유형 [속성],
    ...
);
테이블 삭제
````

````sql
DROP TABLE 테이블명;
테이블 초기화
````

````sql
TRUNCATE TABLE 테이블명;
테이블 구조 보기
````

````sql
DESC 테이블명;
데이터 조작
````

Mysql에서 데이터를 검색하고 조작하는 명령어는 다음과 같습니다.

데이터 삽입
````sql
INSERT INTO 테이블명 (열1, 열2, ...) VALUES (값1, 값2, ...);
데이터 검색
````

````sql
SELECT 열1, 열2, ... FROM 테이블명 WHERE 조건;
데이터 수정
````

````sql
UPDATE 테이블명 SET 열1=값1, 열2=
````



