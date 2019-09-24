목차
=========
* [DDL](#DDL)</br>
* [CREATE](#CREATE)</br>
* [제약 조건](#제약-조건)</br>
* [PRIMARY KEY(기본키), FOREIGN KEY(외래키)](#PRIMARY-KEY-기본키-FOREIGN-KEY-외래키)</br></br>

## DDL

<br/>

- 데이터 정의어.

- 데이터베이스를 정의하는 언어이며 데이터리를 생성, 수정, 삭제하는 

   등의 전체의 형태를 결정하는 언어이다.

- CREATE, ALTER, DROP, TRUNCATE

<br/>

[SQL의 종류 DDL, DML, DCL 이란?](https://server-talk.tistory.com/159) <br/>

<br/>

## CREATE

<br/>

- 테이블, 인덱스, 뷰 등 다양한 객체를 생성하는 구문

- 속성과 속성에 관한 제약, 기본 키 및 외래 키를 정의하는 명령어다.

- DROP를 이용해 제거 가능

<br/>

- 조건 : 관리자 계정으로 test 사용자 계정 생성, ROSOURCE, CONNECT TO 권한 부여.

- 이후 해당 test 사용자 계정의 워크시트를 만들어 진행.

<br/>

```sql
-- 테이블 만들기
CREATE TABLE TEST(
    MYROOM VARCHAR2(10),
    MYROOM_NAME VARCHAR2(15),
    MYROOM_GRADE VARCHAR2(10),
    VIP_ROOM VARCHAR(15),
    ROOM_DATE DATE DEFAULT SYSDATE -- 입력 값이 없거나 DEFAULT일 경우의 컬럼값 지정
);
-- 즉 DATE 부분에 아무런 값이 없거나 DEFAULT를 지정해줄 경우 현재 시간이 들어간다는 의미

-- 컬럼에 주석 달기
COMMENT ON COLUMN TEST.MYROOM IS '나의 방';
COMMENT ON COLUMN TEST.MYROOM_NAME IS '나의 방 이름';
COMMENT ON COLUMN TEST.MYROOM_GRADE IS '나의 방 등급';
COMMENT ON COLUMN TEST.VIP_ROOM IS '특별한 방';
COMMENT ON COLUMN TEST.ROOM_DATE IS '방에 들어온 시간';

-- 코멘트 확인
SELECT * FROM USER_COL_COMMENTS
WHERE TABLE_NAME = 'TEST';

-- 테이블 확인
SELECT * FROM USER_TABLES;

-- 행 삽입
INSERT INTO TEST VALUES('1번 방','김치방','A','N','2017-11-11');
INSERT INTO TEST VALUES('2번 방','사과방','B','N',DEFAULT);
INSERT INTO TEST VALUES('3번 방','복숭아방','S','Y',SYSDATE);
INSERT INTO TEST(MYROOM,MYROOM_NAME,MYROOM_GRADE) VALUES('4번 방','배추방','C');

-- 삽입된 행 확인
SELECT * FROM TEST;

-- 4번째 배추방 같은 경우
-- 4번 방	배추방	C	(null)	19/09/24
-- 위와 같이 추가되 있음을 확인 가능
```

<br/>

## 제약 조건

<br/>

제약조건은 컬럼에 대한 속성을 정의하는 것이다.

* 데이터에 대한 무결성을 보장하기 위한 용도로 사용된다.

- 부적절한 자료가 입력되는 것을 방지하기 위해 각 컬럼에 대한 제약 조건을 설정한다.

<br/>

```sql
-- 제약 조건 확인
DESC USER_CONSTRAINTS;
SELECT * FROM USER_CONSTRAINTS; 
-- 다음 구문의 결과 아무것도 출력이 되지 않음.
--> 만들어진 테이블은 test 하나, 제약조건을 설정해주지 않았기 때문

-- 제약 조건이 걸려있는 컬럼 확인
DESC USER_CONS_COLUMNS;
SELECT * FROM USER_CONS_COLUMNS;
-- 마찬가지로 아무값도 출력되지 않는다.

CREATE TABLE TEST2(
    MYROOM NUMBER NOT NULL, -- NOT NULL 제약 조건 컬럼에서 설정
    MYROOM_NAME VARCHAR2(15) UNIQUE, -- 중복 불가능 UNIQUE 제약조건 컬럼에서 설정
    MYROOM_NAME2 VARCHAR2(15) CONSTRAINT NN_DATA_NAME2 NOT NULL,
    MYROOM_NAME3 VARCHAR2(15),
    MYROMM_GRADE VARCHAR2(15) NOT NULL,
    UNIQUE(MYROOM), -- 테이블 레벨에서 제약조건 설정
    CONSTRAINT UK_DATA_TEST2 UNIQUE (MYROOM_NAME2, MYROOM_NAME3) -- 두개의 컬럼을 묶어서 UNIQUE 설정 
    -- 제약조건에 이름을 붙일 수 있음
);

-- 작성된 제약조건 확인
SELECT * FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'TEST2';

SELECT * FROM USER_CONS_COLUMNS WHERE TABLE_NAME ='TEST2';

SELECT * 
FROM USER_CONSTRAINTS C1
JOIN USER_CONS_COLUMNS C2 USING(CONSTRAINT_NAME)
WHERE C1.TABLE_NAME  = 'TEST2';


-- 행 삽입
INSERT INTO TEST2 VALUES(1,'room1','room2','room3','A');
INSERT INTO TEST2 VALUES(1,'room1','room2','room3','A');
-- 오류 발생 
-- 1중복 불가능, room1 중복 불가능, room2와 room3 묶어서 중복 불가능

-- 제약조건 확인
SELECT UCC.TABLE_NAME, UCC.COLUMN_NAME, UC.CONSTRAINT_TYPE
FROM USER_CONSTRAINTS UC, USER_CONS_COLUMNS UCC
WHERE UCC.CONSTRAINT_NAME = UC.CONSTRAINT_NAME
AND UCC.CONSTRAINT_NAME = 'SYS_C007066';
-- 위와 같이 오류번호를 입력시켜 제약조건 확인 가능

-- 다음과 같이 수정함으로서 해결
INSERT INTO TEST2 VALUES(2,'room2','room3','room3','A');
```

<br/>

## PRIMARY KEY(기본키), FOREIGN KEY(외래키)

<br/>

```sql
-- RRIMARY
CREATE TABLE TEST3(
    MYROOM NUMBER, --CONSTRAINT PK_MYROOM PRIMARY KEY, -- 컬럼레벨
    ROOM_NAME2 VARCHAR(15),
    ROOM_NAME3 VARCHAR(15),
    CONSTRAINT PK_RN2_RN3 PRIMARY KEY(ROOM_NAME2, ROOM_NAME3) -- 복합키
);

-- 여기서 컬럼레벨과 테이블단에서 동시에 PRIMARY_KEY를 설정할 수는 없다.

-- FOREIGN KEY
-- 외래키로 사용할 ROOM_GRADE 생성 후 행 삽입
CREATE TABLE ROOM_GRADE(
    GRADE_CODE VARCHAR(5) PRIMARY KEY,
    GRADE_NAME VARCHAR(15) NOT NULL
);

INSERT INTO ROOM_GRADE VALUES('S','VIP등급');
INSERT INTO ROOM_GRADE VALUES('A','NORMAL등급');
INSERT INTO ROOM_GRADE VALUES('B','LOW등급');
INSERT INTO ROOM_GRADE VALUES('C','BAD등급');

-- 외래키를 이용한 테이블 생성
CREATE TABLE TEST4(
    MYROOM NUMBER PRIMARY KEY,
    MYROOM_NAME VARCHAR2(15) UNIQUE,
    MYROOM_NAME2 VARCHAR2(15) CONSTRAINT NN_DATA_NAME3 NOT NULL,
    MYROOM_NAME3 VARCHAR2(15),
    CUS_NAME VARCHAR2(15),
    CUS_PHONE VARCHAR2(15),
    GRADE_CODE  VARCHAR(5),
    CONSTRAINT FK_ROOM_GRADE FOREIGN KEY (GRADE_CODE) REFERENCES ROOM_GRADE(GRADE_CODE)
);

INSERT INTO TEST4 VALUES(1,'1번방','김치','배추',null,'null','S');
INSERT INTO TEST4 VALUES(2,'2번방','사과','맛있다','손님','010-1234-5678','A');
INSERT INTO TEST4 VALUES(3,'3번방','복숭아','달아?','진상','010-1111-2222','C');

-- NATURAL 조인 
-- 외래키를 이용한 테이블 생성시 GRADE_CODE의 이름이 같으므로
-- 동일한 타입과 이름을 가진 컬럼을 조인 조건으로 이용함
SELECT *
FROM TEST4 
NATURAL LEFT JOIN ROOM_GRADE;

-- FOREIGN KEY 삭제

DELETE FROM ROOM_GRADE
WHERE GRADE_CODE = 'A';
-- 삭제 불가능  삭제 제한 설정되어 있음.
-- 외래키에서 컬럼에서 사용중이므로 제공하는 컬럼은 삭제 불가능
-- 만약 컬럼이 참조되고 있지 않다면 삭제 가능

-- COMMIT을 이용하여 참조되어 있지 않은 컬럼 삭제
COMMIT; -- 커서를 대고 실행 ( 현재 상태 저장 )
DELETE FROM ROOM_GRADE
WHERE GRADE_CODE= 'B';
-- 삭제

SELECT * FROM ROOM_GRADE; -- 조건에 맞는 컬럼이 삭제됨을 알 수 있따.
SELECT * FROM TEST4;

ROLLBACK; -- 돌아오기

-- ON DELETE CASCADE 조건
-- 부모키 삭제시 자식키도 삭제되는 옵션

-- 테이블 생성 및 행 삽입
CREATE TABLE TEST5(
    MYROOM NUMBER PRIMARY KEY,
    MYROOM_NAME VARCHAR2(15) UNIQUE,
    MYROOM_NAME2 VARCHAR2(15) CONSTRAINT NN_DATA_NAME4 NOT NULL,
    MYROOM_NAME3 VARCHAR2(15),
    CUS_NAME VARCHAR2(15),
    CUS_PHONE VARCHAR2(15),
    GRADE_CODE  VARCHAR(5),
    CONSTRAINT FK_ROOM_GRADE2 FOREIGN KEY (GRADE_CODE) REFERENCES ROOM_GRADE(GRADE_CODE) ON DELETE CASCADE
);
-- 외래키 부분 조건의 ON DELETE CASCADE 조건 확인

INSERT INTO TEST5 VALUES(1,'1번방','김치','배추',null,'null','S');
INSERT INTO TEST5 VALUES(2,'2번방','사과','맛있다','손님','010-1234-5678','A');
INSERT INTO TEST5 VALUES(3,'3번방','복숭아','달아?','진상','010-1111-2222','C');

-- TEST4 테이블을 삭제해둬야 TEST5 테이블 실습 가능.
-- TEST4 테이블이 ROOM_GRADE를 참고하고 있기 때문에 TEST5에서 ON DELETE CASCADE를 
-- 설정해두더라도 삭제 불가능함.
DROP TABLE TEST4;

COMMIT;
DELETE FROM ROOM_GRADE
WHERE GRADE_CODE= 'C';
-- 참고하고 있는 컬럼을 삭제할 시
-
SELECT * FROM ROOM_GRADE;
SELECT * FROM TEST5;
-- TEST5의 행을 확인하면 C의 해당되는 3번방 행이 삭제됨을 알 수 있음
-- 

ROLLBACK;

-- 제약조건 추가

-- 제약조건 확인
SELECT * 
FROM USER_CONSTRAINTS c1
JOIN USER_CONS_COLUMNS USING(CONSTRAINT_NAME)
WHERE c1.TABLE_NAME = 'TEST5';


ALTER TABLE TEST5 ADD UNIQUE(CUS_NAME);
ALTER TABLE TEST5 MODIFY CUS_PHONE NOT NULL;
ALTER TABLE TEST5 ADD CHECK(CUS_NAME IN(null,'손님','진상'));
```

<br/>
