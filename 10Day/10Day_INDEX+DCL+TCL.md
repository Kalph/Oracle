목차
=========
* [인덱스](#인덱스)</br>
* [DCL](#DCL)</br>
* [TCL트랜잭션](#tcl트랜잭션)</br></br>

## 인덱스

</br>

- 데이터베이스에서 데이터를 검색할 때 검색되는 데이터의 수를 줄여 성능을 높기이 위해 지정되는 식별자

- 인덱스를 이용하면 데이터베이스 전체를 검색하지 않고 원하는 정보를 빠르게 검색 가능

</br>

```sql
SELECT * FROM USER_IND_COLUMNS;

SELECT * FROM TEST_COPY;

SELECT ROWID, ROOM_NO, MYROOM_NAME
FROM TEST_COPY;
```

</br>

```sql
--인덱스를 활용하여 검색하는 경우
SELECT *
FROM TEST_COPY
WHERE ROOM_NO = 1;
-- PRIMARY KEY와 UNIQUE KEY 제약조건을 설정 시 해당 컬럼에 INDEX가 존재하지 않을 시 
-- 자동으로 UNIQUE 생성

SELECT *
FROM TEST_COPY
WHERE MYROOM_NAME = '1번방';
-- UNIQUE 제약조건이 설정되어 있으므로 인덱스를 이용한 검색

-- 인덱스를 활용하지 않는 경우
SELECT *
FROM TEST_COPY
WHERE MYROOM_NAME2 = '복숭아';
-- 복숭아가 어디 있는지 모르므로 모든 테이블을 조회해서 검색한다. -> 인덱스를 이용하지 않음

SELECT * FROM USER_CONSTRAINTS WHERE TABLE_NAME = 'TEST_COPY';

-- UNIQUE 인덱스 생성
CREATE UNIQUE INDEX MY_RM2_UNI 
ON TEST_COPY(MYROOM_NAME2);
-- UNIQUE 인덱스를 생성함으로서 이제 MYROOM_NAME2를 이용하여 검색할 경우에도
-- 인덱스를 활용하여 검색하게 된다.
-- 또한 UNIQUE 인덱스를 생성할 때, 컬럼값이 중복되어 있으면 생성이 불가능하다.

-- 사용자가 생성된 인덱스 조회
SELECT * FROM USER_INDEXES WHERE TABLE_NAME = 'TEST_COPY';

-- 인덱스 키 조회
SELECT * FROM USER_IND_COLUMNS WHERE TABLE_NAME = 'TEST_COPY';
```

</br>

```sql
-- 비고유 인덱스
-- 성능 향상이 목적

CREATE INDEX IDS_MM3
ON TEST_COPY(MYROOM_NAME3);

ALTER INDEX IDS_MM3
RENAME TO IDX_MM3;

DROP INDEX IDX_MM3;
```

</br>

```sql
-- 인덱스 모니터링
-- 인덱스용 테스트 테이블 생성
CREATE TABLE IND_TEST
AS SELECT * FROM TEST_COPY;

-- 테이블을 생성하면 NOT NULL외의 제약조건은 복사되지 않음
-- UNIQUE 인덱스 생성
CREATE UNIQUE INDEX IDX_EID
ON IND_TEST(ROOM_NO);

-- 인덱스 컬럼을 조회하고
SELECT * FROM USER_IND_COLUMNS
WHERE TABLE_NAME = 'IND_TEST';

-- 인덱스 모니터링 진행
ALTER INDEX IDX_EID MONITORING USAGE;

-- 전체를 조회할 경우
SELECT * FROM IND_TEST;

-- 인덱스 사용유무인 USED가 NO임
SELECT * 
FROM V$OBJECT_USAGE;

-- 생성한 UNIQUE 인덱스로 조회를 할 경우
SELECT * FROM IND_TEST
WHERE ROOM_NO > 0;

-- 인덱스 사용우무인 USED가 YES가 됨
SELECT * 
FROM V$OBJECT_USAGE;

-- 모니터링 종료
ALTER INDEX IDX_EID NOMONITORING USAGE;

-- 인덱스 재구성
ALTER INDEX IDX_MM3 REBUILD;
* 확실하진 않으나, 삽질의 결과

- 인덱스 테이블을 생성해야만 인덱스를 사용해서 조회하는 것을 알 수 있음

- 기존 제약조건에서 UNIQUE 제약 조건을을 줘도 인덱스를 자동 생성하나

- 제약조건과 인덱스라는 객체는 별도.

- USED의 사용유무는 인덱스를 생성해야만 변화가 보인다.
```

</br>

## DCL

</br>

- 데이터를 제어하는 언어

- 데이터베이스, 데이터베이스 객체에 대한 접근 권한을 제어(부여, 회수) 하는 언어

</br>

**테이블 스페이스 할당량 부여**

ALTER USER 계정 QUOTA 할당량(ex.2M) ON SYSTEM; 

**권한 부여**

GRANT 권한 종류 ON 테이블(Ex.COPY_TEST.ROOM_NO) TO 계정

**권한 회수**

REVOKE 권한 종류 ON 테이블(Ex.COPY_TEST.ROOM_NO) TO 계정

</br>

## TCL(트랜잭션)

- 데이터베이스에서 데이터를 처리하는 하나의 단위

COMMIT; , ROLLBACK;, SAVEPOINT 포인트명;

</br>
