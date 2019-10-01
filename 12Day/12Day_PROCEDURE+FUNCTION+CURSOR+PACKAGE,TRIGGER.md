목차
=========
* [PROCEDURE](#procedure)</br>
* [FUNCTION](#function)</br>
* [CURSOR](#cursor)</br>
* [PACKAGE](#package)</br>
* [TRIGGER](#trigger)</br></br>

## PROCEDURE

</br>

- 이름이 있는 PL/SQL BLOCK

- 필요할 때마다 복잡한 구문을 다시 입력할 필요가 없이 PL/SQL BLOCK을 데이터베이스에 저장하기 위해 생성함.

</br>

```sql
SET SERVEROUTPUT ON;
SET AUTOPRINT ON;

CREATE TABLE TEST_C
AS SELECT * FROM TEST_COPY;

SELECT * FROM TEST_C;

CREATE OR REPLACE PROCEDURE DEL_ALL_TEST
IS
BEGIN
    DELETE FROM TEST_C
    COMMKT;
END;
/

EXEC DEL_ALL_TEST;

SELECT * FROM TEST_C;
```

</br>

## FUNCTION

</br>

- 보통 값을 계산하고 결과 값을 반환하기 위해 함수를 많이 사용한다

- 반드시 반환값이 될 값의 데이터 타입을 RETURN 문을 선언해야 한다.

```sql
SET SERVEROUTPUT ON;
SET AUTOPRINT ON;

SELECT * FROM TEST_COPY;

CREATE OR REPLACE FUNCTION NAME_TEST(V_TET_NO TEST_COPY.ROOM_NO%TYPE)
RETURN VARCHAR2
IS
    V_TET_NAME TEST_COPY.MYROOM_NAME2%TYPE;
BEGIN
    SELECT MYROOM_NAME2
    INTO V_TET_NAME
    FROM TEST_COPY
    WHERE ROOM_NO = V_TET_NO;
    RETURN V_TET_NAME;
END;
/

VAR VAR_CHAR VARCHAR2;

EXEC :VAR_CHAR := NAME_TEST('&ROOM_NO');
```

</br>

## CURSOR

</br>

- 쿼리문에 의해서 반환되는 결과값들을 저장하는 메모리 공간

- FETCH : 커서에서 원하는 결과값을 추출하는 것

</br>


[(SQL) 커서(Cursor)에 대해 알아보자](https://rh-cp.tistory.com/50#recentEntries)

</br>

```sql
SET SERVEROUTPUT ON;
SET AUTOPRINT ON;

SELECT * FROM TEST_COPY;

COMMIT;

-- 묵시적 커서 예
BEGIN 
    UPDATE TEST_COPY
    SET MAS_ROOM = '부재중'
    WHERE MAS_ROOM = '대기중';
    
    -- 묵시적 커서
    DBMS_OUTPUT.PUT_LINE(SQL%ROWCOUNT || '행 수정');   
END;
/
ROLLBACK;

-- 명시적 커서 예
DECLARE 
    V_TET_NO TEST_COPY.ROOM_NO%TYPE;
    V_TET_NAME2 TEST_COPY.MYROOM_NAME2%TYPE;
    V_TET_MAS TEST_COPY.MAS_ROOM%TYPE;
    
    CURSOR C1 IS
        SELECT ROOM_NO, MYROOM_NAME2, MAS_ROOM
        FROM TEST_COPY
        WHERE GRADE_CODE IN('S','C');
BEGIN
    OPEN C1;
    
    LOOP
        FETCH C1 INTO V_TET_NO, V_TET_NAME2, V_TET_MAS;
        EXIT WHEN C1%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE(V_TET_NO || ' ' || V_TET_NAME2 || ' ' || V_TET_MAS);
    END LOOP;
    CLOSE C1;
END;
/
```

</br>

## PACKAGE

</br>

- 패키지는 오라클 데이터베이스에 저장되어 있는 PL/SQL 프로시져와 함수들의 집합이다.

</br>

## TRIGGER

</br>

- 특정 테이블에 DML 문이 수행되었을 때 데이터베이스에서 자동으로 동작하도록 작성된 프로그램

```sql
SET SERVEROUTPUT ON;
SET AUTOPRINT ON;

SELECT * FROM TEST_COPY;

CREATE OR REPLACE TRIGGER TET_01
AFTER INSERT
ON TEST_COPY
BEGIN
    DBMS_OUTPUT.PUT_LINE('새로운 손님이 들어왔습니다');
END;
/

INSERT INTO TEST_COPY VALUES(3, '3번방','수박','크다',null,null,'B','부재중',null,null);
```

</br>
