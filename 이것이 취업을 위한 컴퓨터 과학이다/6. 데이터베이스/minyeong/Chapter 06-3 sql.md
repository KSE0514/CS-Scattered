# Chapter 06. 데이터베이스
- [Chapter 06. 데이터베이스](#chapter-06-데이터베이스)
- [06-3. SQL](#06-3-sql)
  - [데이터 정의 언어(DDL)](#데이터-정의-언어ddl)
    - [CREATE](#create)
    - [ALTER](#alter)
    - [DROP](#drop)
    - [TRUNCATE](#truncate)
  - [데이터 조작 언어(DML)](#데이터-조작-언어dml)
    - [INSERT](#insert)
    - [UPDATE와 DELETE](#update와-delete)
    - [SELECT](#select)
  - [트랜잭션 제어 언어(TCL)](#트랜잭션-제어-언어tcl)
    - [COMMIT, ROLLBACK - 트랜잭션 실행 결과](#commit-rollback---트랜잭션-실행-결과)
    - [SAVEPOINT](#savepoint)
  - [데이터 제어 언어(DCL)](#데이터-제어-언어dcl)
- [Q\&A](#qa)
  - [1. SELECT절의 명령어를 실행 순서대로 말해주세요.](#1-select절의-명령어를-실행-순서대로-말해주세요)
  - [2. 다음 코드를 보고 테이블에 남아있는 데이터와 실행 과정을 설명해주세요.](#2-다음-코드를-보고-테이블에-남아있는-데이터와-실행-과정을-설명해주세요)
  - [3. ALTER와 INSERT의 차이점을 설명해주세요.](#3-alter와-insert의-차이점을-설명해주세요)

# 06-3. SQL

SQL문에서는 세미콜론 기호(;)를 통해 끝을 표기!

## 데이터 정의 언어(DDL)

데이터 정의를 위한 SQL

| **종류** | **설명** |
| --- | --- |
| CREATE | 데이터베이스 혹은 데이터베이스 객체 생성 |
| ALTER | 데이터베이스 객체 갱신 (예 : 테이블에 필드 및 제약 조건 추가/삭제) |
| DROP | 데이터베이스 객체 삭제 (예 : 테이블이나 데이터베이스 삭제) |
| TRUNCATE | 테이블 구조를 유지한 채 모든 레코드 삭제 |
- **데이터베이스 객체** : 데이터베이스에서 정의될 수 있는 대상(테이블, 인덱스, 뷰 등)

### CREATE

데이터 베이스, 테이블, 뷰, 인덱스, 그 외 사용자 등 데이터베이스에서 관리될 수 있는 다양한 대상 정의(생성)

**데이터베이스와 테이블 만들기(실습)**

① 데이터베이스 만들기

```sql
CREATE DATABASE 데이터베이스_이름;
-- CREATE DATABASE mydb;

-- 데이터베이스 조회
SHOW DATABASES;

-- 이미 존재하거나 만들어진 특정 데이터베이스 사용할 때
USE 데이터베이스_이름;
-- USE mydb;
```

② 테이블 만들기

```sql
CREATE TABLE 테이블_이름 (
	필드_이름1, 필드_타입,
	필드_이름2, 필드_타입,
	...
	[CONSTRAINT 제약_조건_이름] PRIMARY KEY (필드_이름),
	[CONSTRAINT 제약_조건_이름] FOREIGN KEY (필드_이름) REFERENCES 테이블_이름2 (필드_이름),
	[CONSTRAINT 제약_조건_이름] UNIQUE (필드_이름),
);

-- 데이터베이스에 속한 전체 테이블 조회
SHOW TABLES;
SHOW TABLES FROM 데이터베이스_이름;

-- 테이블 조회
DESCRIBE 테이블_이름;
DESC 테이블_이름;
```

- 특정 필드가 지켜야 할 제약 조건 명시
    - 필드_타입의 우측 또는 CREATE TABLE문 하단에 키워드 명시
    
    | **키워드** | **제약 조건** |
    | --- | --- |
    | PRIMARY KEY | 특정 필드를 기본 키로 설정 |
    | UNIQUE | 특정 필드가 고유한 값을 갖도록 설정 (**고유 키(UNIQUE KEY)** : 중복되는 값을 가질 수 없지만 테이블 내에 여러 개 존재 가능, NULL 값 가질 수 있음) |
    | FOREIGN KEY | 특정 필드를 외래 키로 설정 |
    | DEFAULT 기본값 | 기본값 지정 |
    | NULL/NOT NULL | 특정 필드에 NULL 값을 허용/허용하지 않음 |

### ALTER

생성된 테이블에 필드 추가/수정/삭제(제약 조건 추가/수정/삭제)

```sql
-- 새로운 필드 추가
ALTER TABLE 테이블_이름 ADD COLUMN 필드_이름 필드_타입 [제약_조건];
-- ALTER TABLE posts ADD COLUMN new_field VARCHAR(50) NOT NULL;

-- 기존 필드 수정
ALTER TABLE 테이블_이름 CHANGE COLUMN 기존_필드_이름 새_필드_이름 필드_타입 [제약_조건];
-- ALTER TABLE posts CHANGE COLUMN new_field old_field VARCHAR(30) NOT NULL;

-- 기존 필드 삭제
ALTER TABLE 테이블_이름 DROP COLUMN 필드_이름;
-- ALTER TABLE posts DROP COLUMN old_field;

-- 외래 키 제약 조건 추가
ALTER TABLE 테이블_이름 [ADD CONSTRAINT 제약_조건_이름] ADD FOREIGN KEY (필드_이름) REFERENCES 참조_테이블_이름(참조_필드);
-- ALTER TABLE posts ADD FOREIGN KEY (user_id) REFERENCES users(user_id);

-- UNIQUE 제약 조건 추가
ALTER TABLE 테이블_이름 [ADD CONSTRAINT 제약 조건 이름] UNIQUE (필드_이름);
-- ALTER TABLE posts ADD UNIQUE (title);

-- NOT NULL 제약 조건 추가
ALTER TABLE 테이블_이름 MODIFY 필드_이름 필드_타입 NOT NULL;
-- ALTER TABLE users MODIFY email VARCHAR(100) NOT NULL;

-- 기본 키 설정(PRIMARY KEY로 사용 중인 필드가 없을 경우
ALTER TABLE 테이블_이름 ADD PRIMARY KEY (필드_이름);
-- ALTER TABLE posts ADD PRIMARY KEY (post_id);
```

### DROP

테이블이나 데이터베이스 삭제

```sql
DROP DATABASE 데이터베이스_이름;
DROP TABLE 테이블_이름;
```

### TRUNCATE

테이블의 구조를 유지한 채 테이블의 모든 레코드 삭제(테이블 자체를 삭제X)

```sql
TRUNCATE TABLE 테이블_이름;
```

- 테이블 구조를 유지하기 때문에 테이블 조회 가능, 필드 확인 가능

## 데이터 조작 언어(DML)

| **종류** | **설명** |
| --- | --- |
| SELECT | 테이블의 레코드 조회  |
| INSERT | 테이블의 레코드 삽입 |
| UPDATE | 테이블의 레코드 수정 |
| DELETE | 테이블의 레코드 삭제 |

### INSERT

```sql
INSERT INTO 테이블_이름(필드1, 필드2) VALUES (값1, 값2);

INSERT INTO 테이블_이름(필드1, 필드2, 필드3) VALUES
	(값1, 값2, 값3),
	(값1, 값2, 값3),
	(값1, 값2, 값3);
```

- 삽입할 값이 지정되지 않은 필드는 기본값 또는 NULL로 채워짐
- 무결성 제약 조건 무조건 지켜야 함 (CREATE TABLE/ALTER TABLE 에서 지정한 제약 조건)

### UPDATE와 DELETE

각각 레코드 수정/삭제

```sql
UPDATE 테이블_이름
	SET 필드1 = 값1, 필드2 = 값2, ...
	WHERE 조건식(생략_가능); 
	
DELETE 테이블_이름
	WHERE 조건식(생략_가능);
```

- WHERE절을 생략하면 모든 레코드 갱신, 삭제
    
    
    | 연산자 | 설명 |
    | --- | --- |
    | = (비교 연산자) | 같을 경우 참 |
    | > | 클 경우 참 |
    | < | 작을 경우 참 |
    | ≥ | 크거나 같을 경우 참 |
    | ≤ | 작거나 같을 경우 참 |
    | <> | 다를 경우 참 |
    | 조건식1 AND 조건식2 | 조건식1과 조건식2가 모두 만족할 경우 참 |
    | 조건식1 OR 조건식2 | 조건식1과 조건식2 둘 중 하나만 만족할 경우 참 |
    | NOT 조건식 | 조건식이 아닐 경우 참 |
    | IN | IN 뒤에 명시되는 값과 하나 이상이 일치할 경우 참 |
    - SET의 = : 대입 연산자

**외래 키 제약 조건 - ONUPATE, ONDELETE**

| **제약 조건** | **설명** |
| --- | --- |
| CASCADE | 참조하는 데이터도 함께 수정/삭제 |
| SET NULL | 참조하는 데이터를 NULL로 변경 |
| SET DEFAULT | 참조하는 데이터를 기본값으로 변경 |
| RESTRICT | 수정/삭제 허용 안 함  |
| NO ACTION | (MySQL 경우) RESTRICT와 동일하게 동작 |

```sql
CREATE TABLE posts (
	post_id INT PRIMARY KEY AUTO_INCREMENT,
	user_id INT,
	title VARCHAR(50) NOT NULL,
	content VARCHAR(50),
	created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
	FOREIGN KEY (user_id) REFERENCES users(user_id)
	ON UPDATE CASCADE
	ON DELETE SET NULL
);
```

### SELECT

테이블에 삽입된 레코드 조회, 정렬 또는 필터링 가능

(✨ 실행 순서 : FROM → WHERE → GROUP BY → HAVING → SELECT → ORDER BY → LIMIT)

```sql
SELECT 필드1, 필드2, // 하나 이상의 필드명, *은 모든 필드 조회 의미
	FROM 조회하려는_테이블_이름
	WHERE 조건식
	GROUP BY 그룹화할_필드
	HAVING 필터_조건
	ORDER BY 정렬할_필드
	LIMIT 레코드_제한;
```

- **패턴 검색** : 문자열 데이터에서 특정 패턴을 찾는 기능
    - LIKE 연산자와 와일드카드 문자(%, _) 사용
        - % : 0개 이상의 임의 문자와 일치
        - _ : 1개의 임의 문자와 일치
- **연산/집계 함수** : 조회된 레코드에 대한 특정 연산 수행/집계하는 함수
    - COUNT, SUM, AVG, MAX, MIN

**GROUP BY**

특정 필드를 기준으로 필드를 그룹화하기 위해 사용(~별, ~인)

- 연산/집계 함수와 함께 사용되는 경우 많음

**HAVING**

GROUP BY절로 그룹화된 결과에 대해 조건을 적용하기 위해 사용

**ORDER BY**

특정 필드를 기준으로 데이터 정렬

- ASC : 오름차순, 기본
- DESC : 내림차순

**LIMIT**

조회할 레코드 수 제한

- 상위 레코드만 조회(LIMIT 숫자)하거나 특정 번째로부터 떨어진 레코드부터 조회(offset, 숫자)

## 트랜잭션 제어 언어(TCL)

| **종류** | **설명** |
| --- | --- |
| COMMIT | 데이터베이스에 작업 반영 |
| ROLLBACK | 작업 이전의 상태로 되돌림 |
| SAVEPOINT | 롤백의 기준점 설정 |

### COMMIT, ROLLBACK - 트랜잭션 실행 결과

```sql
-- 여러 작업을 포함하는 트랜잭션을 실행하고 싶을 때
START TRANSACTION // 또는 BEGIN

UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
```

- **COMMIT** : 트랜잭션이 성공적으로 완료되어 트랜잭션에서 수행된 모든 변경 사항을 데이터베이스에 영구적으로 반영
    - MySQL에서는 매 SQL문이 자동으로 커밋
        - SET autocommit=1; (자동 커밋 기능 켜기)
        - SET autocommit=0; (자동 커밋 기능 끄기)
    - `START TRANSACTION` 또는 `BEGIN` 을 실행하면 자동 커밋이 꺼진 상태로 실행되어 COMMIT이나 ROLLBACK을 만나기 전까진 커밋되지 않음
- **ROLLBACK** : 트랜잭션에서 수행된 변경 사항을 취소하고, 데이터베이스를 트랜잭션 시작 전의 상태로 되돌림

### SAVEPOINT

ROLLBACK으로 되돌아갈 시점 지정

```sql
-- 되돌아갈 시점 지정
SAVEPOINT 세이프포인트_이름;

-- 세이브포인트_이름으로 되돌아가기
ROLLBACK TO SAVEPOINT 세이프포인트_이름;

-- 실행 흐름 예시
BEGIN;  

SAVEPOINT sp1;  
INSERT INTO users (id, name) VALUES (1, 'Alice');  -- (작업1)

SAVEPOINT sp2;  
INSERT INTO users (id, name) VALUES (2, 'Bob');    -- (작업2)

ROLLBACK TO SAVEPOINT sp1;  -- (sp1 이후 작업 취소)
ROLLBACK TO SAVEPOINT sp2;  -- ❌ 의미 없음! 에러 발생할 수 있 (sp2는 이미 사라졌음)

INSERT INTO users (id, name) VALUES (3, 'Charlie');  -- (작업3)

COMMIT; // 테이블에 Charlie만 남음
```

## 데이터 제어 언어(DCL)

DBMS는 서버와 같은 형태로 실행되기 때문에 RDBMS에서도 접속 가능한 사용자 계정을 생성(`CREATE USER`)하거나 삭제(`DROP USER`) 가능 → 사용자의 권한 관리

| **종류** | **설명** |
| --- | --- |
| GRANT | 사용자에게 권한 부여 |
| REVOKE | 사용자로부터 권한 회수 |

# Q&A
## 1. SELECT절의 명령어를 실행 순서대로 말해주세요.
가장 먼저 조회할 테이블을 지정하기 위해 **FROM**이 실행되고, 조건을 적용하기 위해 **WHERE**이 실행됩니다.  
만약 그룹별로 조회를 해야 한다면, **GROUP BY**가 실행되며, 그룹에 대한 조건을 필터링하는 **HAVING**이 이어집니다.  
이후 선택한 컬럼을 조회하는 **SELECT**가 실행되고, 결과를 정렬하는 **ORDER BY**와 조회된 레코드 수를 제한하는 **LIMIT**이 마지막으로 실행됩니다.

## 2. 다음 코드를 보고 테이블에 남아있는 데이터와 실행 과정을 설명해주세요.
```SQL
BEGIN;  

SAVEPOINT sp1;  
INSERT INTO users (id, name) VALUES (1, 'Alice');

SAVEPOINT sp2;  
INSERT INTO users (id, name) VALUES (2, 'Bob');

ROLLBACK TO SAVEPOINT sp2;
ROLLBACK TO SAVEPOINT sp1;

COMMIT;
```
테이블에 남아 있는 데이터는 없습니다.  

트랜잭션이 시작되면서 **SAVEPOINT sp1**이 설정됩니다.  
그 후, users 테이블에 (id=1, name='Alice') 데이터가 추가됩니다.  
이어서 **SAVEPOINT sp2**가 설정되고, users 테이블에 (id=2, name='Bob') 데이터가 추가됩니다.  
이때 **ROLLBACK TO SAVEPOINT sp2**가 실행되면서,
sp2 이후 실행된 모든 SQL이 취소되므로 "Bob"이 추가된 것이 롤백됩니다.  
즉, 테이블에는 "Alice"만 남아 있는 상태가 됩니다.  
그 후 **ROLLBACK TO SAVEPOINT sp1**이 실행됩니다.  
이 명령어는 sp1 이후 실행된 모든 SQL을 취소하므로, "Alice"도 삭제됩니다.  
결과적으로 테이블에는 아무 데이터도 남아 있지 않게 됩니다.

마지막으로 COMMIT이 실행되면서, 이 상태가 최종 확정됩니다.
따라서 트랜잭션이 종료된 후 테이블을 조회해 보면, 아무 데이터도 남아 있지 않습니다.

## 3. ALTER와 INSERT의 차이점을 설명해주세요.
ALTER는 테이블의 구조를 변경할 때 사용되는 명령어로, `ADD COLUMN`, `DROP COLUMN`, `MODIFY COLUMN`, `RENAME COLUMN`등을 통해 컬럼을 추가/삭제/변경 할 수 있습니다.  
반면, INSERT는 테이블에 새로운 데이터를 추가하기 위한 명령어로, INSERT INTO 테이블명 (필드명) VALUES (값)의 형태로 사용됩니다.