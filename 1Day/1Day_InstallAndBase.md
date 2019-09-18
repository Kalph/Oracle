목차
==============
[설치](#설치)<br/>
[SQL Developer](#sql-developer)<br/><br/>

## <설치>

<br/>

### 오라클 데이터베이스

<br/>

[https://www.oracle.com/kr/downloads/](https://www.oracle.com/kr/downloads/) <br/>

-> Oracle Database 11g Release 2 Express Edition for Windows 64를 다운 후 설치한다.

<br/>

설치 시 경로는 D:\Oracle -> Oracle 폴더를 미리 만들어둔다.

비밀번호는 oracle로 지정해둔다.

<br/>

이후 CMD -> sqlplus -> sys as sysdba -> 비밀번호를 입력하여 오라클이 정상적으로 설치되었는지 확인한다.

<br/>

![sample1](/Image/Day1_Img/sample1.png)

<br/>

## SQL Developer

<br/>

[https://www.oracle.com/tools/downloads/sqldev-v192-downloads.html](https://www.oracle.com/tools/downloads/sqldev-v192-downloads.html)

<br/>

Windows 32-bit/64-bit 설치 진행

<br/>

초기 경로 - java가 설치 된 경로로 잡아준다.아마 9.0이하 버전부터만 호환되므로 그 이상의 자바 버전이라면 에러 발생함. 하위 버전으로 자바 재설치 필요.

<br/>

### 데이터베이스

<br/>

**데이터** - 관찰 결과로 나타난 값.  질적 또는 양적 값을 말한다. <br/>

**정보** - 데이터를 기반으로 의미를 부여한 것. 데이터를 정리한 것을 말한다.

**ex) 이 불꽃의 온도는 1300도이다. - 데이터**

**ex) 이 불꽃은 매우 뜨겁다. - 정보**

<br/>

**데이터베이스는 - 유용한 데이터의 집합, 지속적으로 유지 관리해야 하는 데이터의 집합을 말한다.**

<br/>

**DBMS** - 데이터베이스에서 데이터 추출, 조작, 정의, 제어등을 도와주는 관리 프로그램

우리가 사용하는건 여러 DBMS중 Oracle이다.

<br/>

[[DB기초] DBMS 개념과 종류 및 장단점 분석](https://coding-factory.tistory.com/78)

<br/>

### SQL

<br/>

DQL - 데이터 검색 - SELECT

DML - 데이터 조작 - INSERT, UPDATE, DELETE

DDL - 데이터 정의 - CREATE, DROP, ALTER

TCL - 트랜젝션 제어 - COMMIT, ROLLBACK

<br/>

## SQL Developer

<br/>

f9 - 실행 (해당 명령구문 줄에 대고 입력)

ctrl + f10 - 전체실행

Alt-F10 - 새워크시트

ctrl <-> - 워크시트 좌우 옮기기

f5 - 전체실행

<br/>

초기 SQL Developer 실행 후 좌 상단의 Oracle 접속에 마우스 우 클릭 -> 새 접속을

통해 새로운 접속을 한다 이름은 sys as sysdba - 비밀번호는 oracle

이후 테스트 -> 저장 -> 접속 순서로 진행한다.

<br/>

새 접속을 통해 sys as sysdba를 만들고 비밀번호 oracle을 통해 워크시트를 만든다.

<br/>

해당 sys는(오라클 DB 관리자 계정)으로 모든 권한을 보유하고 있다.(DB 생성 및 삭제 가능)


```sql
-- 관리자 계정에 접속 - 사용자를 생성 후 권한을 부여한다.

-- myRoom이란 유저를 생성 후 IDENTIFIED를 통해 비밀번호를 부여하였다.
-- F9를 통해 구문을 실행한다.
CREATE USER myRoom IDENTIFIED BY myRoom; -- 이 구문을 두 번 실행시 이미 유저가 있기에 오류가 발생함.
-- RESOURCE(객체(생성,수정,삭제), 데이터(입력,수정,조회,삭제)), CONNECT(접속) 권한을 부여한다.
GRANT RESOURCE, CONNECT TO myRoom;
```

<br/>

유저를 생성했다면 myRoom으로 새 접속을 하여 새 워크시트를 생성한다.

<br/>

SQL Plus - CLi 툴

SQl Developer - GUI 툴

<br/>


[[DB] DDL, DML, DCL 이란?](brownbears.tistory.com)

<br/>

```sql
SELECT * FROM LIVINGROOM; -- LivingRoom의 테이블의 모든 정보를 조회 ( *은 모든 정보를 의미 )
-- SELECT : 데이터베이스의 데이터를 조회하거나 검색하기 위한 명령어이다.
-- SELECT 가 아닌 select로 작성해도 상관 X - 대소문자를 구분하지 않음
-- 회사에서 따라 대문자와 소문자를 작성하는 방식이 다를 수 있음.
-- 드래그 후 alt + ' 을 통해 해당 구문의 대소문자를 한번에 변경 가능

 -- LIVINGROOM 테이블의 해당 ROOM1~3까지의 정보 조회
SELECT ROOM1,ROOM2,ROOM3 FROM LIVINGROOM;

-- 컬렘에 산술연산을 할 수도 있음
SELECT ROOM_COUNT * 10 FROM LIVINGROOM;

-- 현재 날짜에서 ROOM-DATE를 뺄 수도 있음
SELECT SYSDATE-ROOM_DATE FROM LIVINGROOM;
-- SYSDATE 는 현재 날짜 명령어
SELECT ROOM_DATE -'90/01/01' FROM LIVINGROOM;

-- 컬럼 별칭을 통해 ROOM1의 별칭을 정해줄 수 있음
SELECT ROOM1 룸1 FROM LIVINGROOM;
SELECT ROOM1 "룸1" FROM LIVINGROOM;
SELECT ROOM1 AS 룸1 FROM LIVINGROOM;
SELECT ROOM1 AS "룸1" FROM LIVINGROOM;
-- 위와 같은 다양한 방식이 있음.

-- 리터럴을 이용하면 테이블에 존재하는 데이터로 사용 가능
SELECT ROOM1 '방' AS "룸1" FROM LIVINGROOM; 

-- WHERE를 통해 LIVINGROOM 테이블에서 ROOM_NAME가 A1인 행의 ROOM1을 골라냄
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM_NAME = 'A1';

-- A1이 아닌 행들을 골라냄 != 대신 <> 와 ^=으로 대체 가능
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM_NAME != 'A1';

-- AND 논리 연산자 이용
-- LIVINGROOM 테이블에서 ROOM_NAME이 'A1' 아닌 행들 중 ROOM_PRICE가 10000을 넘는 
-- 행들의 ROOM1을 골라냄
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM_NAME != 'A1' AND ROOM_PRICE > 10000;

-- LIVINGROOM 테이블에서 ROOM_NAME이 A1과 A2가 아닌 행들 중 ROOM_DATE가 15년01월01일 미만의
-- 행들의 ROOM1을 골라냄
-- 다중 조건을 사용할 경우 괄호를 쳐 주어 순서를 구분하였다. 
SELECT ROOM1 FROM LIVINGROOM 
WHERE (ROOM_NAME != 'A1' OR ROOM_NAME != 'A2')
AND ROOM_DATE < '15/01/01';

-- BETWEEN AND를 이용하여 LIVINGROOM 테이블의 10000 이상 50000이하의 행들의 ROOOM1을 골라냄
SELECT ROOM1, ROOM2
WHERE LIVINGROOM
WHERE ROOM_PRICE BETWEEN 10000 AND 50000;
-- WHERE ROOM_PRICE NOT BETWEEN 10000 AND 50000; -- NOT이 붙으면 10000미만 50000초과의 의미
-- WHERE NOT ROOM_PRICE BETWEEN 10000 AND 50000;

-- LIVING 테이블에서 ROOM_NAME이 A로 시작하는 행들의 ROOM1을 골라냄
SELECT ROOM1 
FROM LIVINGROOM
WHERE ROOM_NAME LIKE 'A%';

-- A로 끝나는 경우를 의미
SELECT ROOM1 
FROM LIVINGROOM
WHERE ROOM_NAME LIKE '%A';

-- 1이 포함된 경우를 의미
SELECT ROOM1 
FROM LIVINGROOM
WHERE ROOM_NAME LIKE '%1%';

-- LIVINGROOM 테이블에서 ROOM_NAME의  
-- 3번째 자리가 1로(_의 개수가 2개이므로 _은 글자수를 의미) 시작하는 행들의 ROOM1을 골라냄
SELECT ROOM1
FROM LIVINGROOM
WHERE ROOM_NAME LIKE '__1%';

-- 앞글자가 2자리이며 뒷자리가 한 자리인경우를 의미 ex) 101_A같은 경우를 골라내기 위한 경우
-- ESCAPE를 사용함.
SELECT ROOM1
FROM LIVINGROOM
WHERE ROOM_NAME LIKE '__@_%' ESCAPE '@';
```

<br/>
