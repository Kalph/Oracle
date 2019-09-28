목차
=========
* [VIEW](#VIEW)</br>
 * [뷰 옵션](#뷰 옵션)</br>
* [SEQUENCE](#SEQUENCE)</br>

## VIEW

- 하나 또는 하나 이상의 테이블로부터 데이터의 부분집합을 논리적으로 표현한 것

- 논리적인 가상 테이블임

- 실질적 데이터를 저장하진 않는다

<br/>

**사용 이유?**

- 데이터를 선별적으로 보여줌으로 민감한 정보 보호 가능

- 복잡한 질의를 쉽게 만들어줌

- 성능 향상

<br/>

```sql
GRANT CREATE VIEW TO test;
```

<br/>

먼저 관리자 계정에서 위와 같은 구문을 통해 VIEW를 생성할 권한을 test 계정에 부여해준다.

<br/>

```sql
SELECT * FROM USER_VIEWS;
SELECT * FROM VIEW_TEST;
```

<br/>

위와 같은 구문을 통해 뷰를 조회할 경우 VIEW에 TEST_COPY의 값들이 들어가 있음을 알 수 있다.

**여기서 TEST_COPY(베이스 테이블)의 정보가 변경되면 VIEW의 정보도 변경된다.**

<br/>

```sql
CREATE OR REPLACE VIEW V_TEST2(방번호,방이름,손님,손님등급)
AS SELECT ROOM_NO,MYROOM_NAME, MYROOM_NAME2, DECODE(GRADE_CODE, 'S','VIP','B','LOW','C','BAD')
FROM TEST_COPY;

SELECT * FROM V_TEST2;
```

<br/>

함수를 사용할 경우 별칭을 지정해줘야 오류가 생기지 않고 정상적으로 VIEW가 생성된다.

<br/>

생성된 뷰를 통해서 INSERT, UPDATE, DELETE도 가능하다

<br/>

```sql
SELECT * FROM TEST_COPY;
INSERT INTO V_TEST2 VALUES(4,'4번방','수박','C');
```
<br/>

그러나 위 구문에서는 오류가 생긴다.

이는 뷰를 통해서 베이스 테이블을 업데이트 하려 할 경우 대상 테이블의 레코드가 변형이 되면 안된다.

따라서 아래와 같이 새 테이블을 만들고 수정하면 정상적으로 INSERT가 됨을 알 수 있다.

<br/>

```sql
CREATE OR REPLACE VIEW V_TEST3
AS SELECT *
FROM TEST_COPY;

INSERT INTO V_TEST3 VALUES(2,'2번방','수박','달아',null,null,'S','대기중',null,null);

SELECT * FROM V_TEST3;
SELECT * FROM TEST_COPY;
-- 베이스테이블과 뷰 테이블의 내용물을 확인할 경우 베이스 테이블의 
-- 내부값들도 수정됐음을 알 수 있다.

UPDATE V_TEST3
SET MYROOM_NAME2 = '복숭아'
WHERE MYROOM_NAME2 ='사과';

DELETE FROM V_TEST3
WHERE ROOM_NO = 2;
```

**핵심은 뷰에 정의되지 않은 컬럼을 조작하려 할 경우 오류가 생긴다는 것.** 

또는 뷰에 포함되지 않은 컬럼 중 베이스가 되는 테이블 컬럼이 NOT NULL 제약조건이 지정된 경우에도 

INSERT시 오류가 발생한다.

<br/>

이 외에도 산술 표현식의 경우 INSERT 부분에서 오류 발생

JOIN은 INSERT와 UPDATE부분에서 오류 발생

GROUP BY, DISTINCT절을 포함한 경우 모든 DML 명령어 모두 오류가 발생한다.

<br/>

## 뷰 옵션

<br/>

OR  REPLACE - 기존의 뷰를 생성하는 것과 마찬가지로 사용, 덮어쓰기 기능

FORECE / NO FORCE - FROCE를 사용할 경우 테이블이 존재하지 않아도 뷰를 생성한다. 

WITH CHECK OPTION - 해당 옵션을 설정한 컬럼의 값을 수정 불가능하게 함

WITH READ ONLY  - 뷰에 대해 조회만 가능하게 설정

<br/> <br/>

## SEQUENCE

<br/>

- 자동으로 숫자를 생성시켜 기본 키의 조건을 만족시켜줌

- 하나의 시퀸스는 여러 테이블에서 사용 가능

<br/>

**시퀸스 만들어보기**

<br/>

```sql
CREATE SEQUENCE SEQ_TEST
START WITH 100
INCREMENT BY 10
MAXVALUE 200
NOCYCLE
NOCACHE;
-- 시작값 100에서 10씩 증가하되 200을 최대로 두는 SEQ_TEST 시퀸스 생성

SELECT * FROM USER_SEQUENCES;

-- NEXTVAL을 통해 시퀸스를 호출하고 CURRVAL을 통해 현재 시퀸스의 값을 확인
SELECT SEQ_TEST.NEXTVAL FROM DUAL;
SELECT SEQ_TEST.CURRVAL FROM DUAL;

-- 시퀸스를 SELECT, INSERT, UPDATE에도 사용 가능
- VIEW나 DISTINCT, GROUP/ORDER BY 및 서브쿼리, DEFAULT 값 에서는 사용 불가능

-- 시퀸스 수정
ALTER SEQUENCE SEQ_TEST
INCREMENT BY 20
MAXVALUE 1000;

-- 시작 값은 수정 불가능
-- 삭제 후 재생성 해야함
DROP SEQUENCE SEQ_TEST;
```
<br/>
