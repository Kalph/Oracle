목차
=========
* [DML](#DML)</br>
* [INSERT](#INSERT)</br>
* [UPDATE](#UPDATE)</br>
* [DELETE](#DELETE)</br><br/>

## DML 

* 데이터 조작어, 데이터베이스의 데이터를 조회, 삽입, 변경, 삭제가 가능하다.

- SELECT, INSERT, UPDATE, DELETE

<br/>

## INSERT

<br/>

* 테이블의 데이터(행)을 추가하기 위한 SQL 문

-- 행 추가
INSERT INTO TEST5 VALUES(2,'2번방','사과','맛있다','착한손님','111-23452-4524','S');

-- 서브쿼리를 이용하여 INSERT도 가능하다.

-- TEST5의 정보, 제약조건 딕셔너리와 테이블 컬럼 관리 딕셔너리을 조회

<br/>

```sql
SELECT * FROM TEST5;
SELECT * FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'TEST5';
SELECT * FROM USER_TAB_COLS WHERE TABLE_NAME = 'TEST5';
```

<br/>

## INSERT ALL

<br/>

- INSERT시 서브쿼리가 사용하는 테이블이 같은 경우, 두 개 이사으이 테이블을 한번에 삽입 가능.

<br/>

[Unconditional INSERT ALL](http://www.gurubee.net/lecture/2688) <br/>

[[Oracle 공부하기]INSERT ALL](https://onnuri0.tistory.com/entry/Oracle-%EA%B3%B5%EB%B6%80%ED%95%98%EA%B8%B0INSERT-ALL)

<br/>

## UPDATE 

<br/>

```sql
SELECT * FROM TEST_COPY;

COMMIT;
UPDATE TEST_COPY
SET MAS_ROOM = '대기중';
ROLLBACk;

UPDATE TEST_COPY
SET MAS_ROOM = '대기중'
WHERE GRADE_CODE = 'S';
```

<br/>

## MERGE - 병합

<br/>

[ORACLE | MERGE문 (DML)](https://everyday-deeplearning.tistory.com/entry/ORACLE-MERGE%EB%AC%B8DML)<br/>
## DELETE

<br/>

```sql
COMMIT;
DELETE FROM TEST_COPY
WHERE MYROOM_NAME ='1번방';
SELECT * FROM TEST_COPY;
ROLLBACK;

-- FOREIGN KEY 제약조건이 설정되어 있는 경우 참고 된 값에 대해선 삭제 불가

-- 임시로 사용할 테이블 TMP 생성
CREATE TABLE TMP(
    ROOM_A REFERENCES ROOM_GRADE(GRADE_CODE)
);

SELECT * FROM TMP;

-- 행을 집어넣고
INSERT INTO TMP VALUES('S');
INSERT INTO TMP VALUES('C');

COMMIT;

-- 조건을 주어 참고한 테이블의 S를 삭제하고자 할 경우 불가능 
-- 오류 : child record found 발생
DELETE FROM ROOM_GRADE
WHERE GRADE_CODE ='S';

-- 다만 참고하지 않은 값에 한해서는 행 삭제 가능
DELETE FROM ROOM_GRADE
WHERE GRADE_CODE ='B';

ROLLBACK;

-- 제약조건 활성화/비활성화
SELECT * FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'TMP';

-- 비활성화
ALTER TABLE TMP
DISABLE CONSTRAINT SYS_C007314 CASCADE;

-- 활성화
ALTER TABLE TMP
ENABLE CONSTRAINT SYS_C007314;

-- TRUNCATE
-- 테이블을 자른다. ROLLBACK으로 복구 불가능. 신중한 사용을 요함.
TRUNCATE TABLE TMP;
SELECT * FROM TMP;

ROLLBACK;
```
<br/>
