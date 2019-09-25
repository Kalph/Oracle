## 연산자 우선순위


</br>

1. 산술연산자

2. 연결연산자

3. 비교연산자

4. IS NULL / IS NOT NULL, LIKE / NOT LIKE, IN / NOT IN

5. BETWEEN AND / NOT BETWEEN AND

6. NOT(논리연산자)

7. AND(논리연산자)

8. OR(논리연산자)

</br>

```sql
-- NOT는 컬럼명 앞이나 뒤에 붙으면 된다.
SELECT ROOM1 FROM LIVINGROOM WHERE NOT ROOM1 LIKE = '1호실';
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM1 NOT LIKE = '1호실';


-- 컬럼 값이 NULL인지 비교
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM1 IS NULL;
SELECT ROOM1 FROM LIVINGROOM WHERE ROOM1 IS NOT NULL;
-- NULL의 유무는 항상 IS NULL, IS NOT NULL로 비교되어야 한다.

SELECT * FROM LIVINGROOM WHERE ROOM_NAME IN('A룸', 'B룸');
 -- LIVINGROOM 컬럼의 ROOM_NAME이 A룸과 B룸인 모든 정보 조회
SELECT * FROM LIVINGROOM WHERE ROOM_NAME NOT IN('A룸');
 -- LIVINGROOM 컬럼의 ROOM_NAME이 A룸이 아닌 모든 정보 조회

SELECT * FROM LIVINGROOM WHERE ROOM_NAME IN('A룸','B룸') AND ROOM IS NULL;

-- || 연결 연산자
SELECT ROOM1 || '에는' || CUSTOMER '손님이 있습니다.' "방 정보"
FROM LIVINGROOM;

-- ORDER BY
SELECT * FROM LIVINGROOM ORDER BY ROOM_NAME ASC NULLS FIRST; -- LAST 옵션
-- LIVINGROOM 컬럼의 ROOM_NAME을 기준으로 오름차순 정렬하되 NULL을 제일 앞에 놓겠다는 의미.

-- ORDER BY 1 -- 컬럼 순서가 올 수도 있음

SELECT ROOM_NAME 방 정보 
FROM LIVINGROOM 
ORDER BY 방 정보;
--  방 정보와 같은 별칭이 올 수도 있다.
```

</br>


```sql
-- LENGTH : 글자 수 반환, LENGTHB : 글자의 바이트 사이즈 반환
-- 한글의 경우 한 글자를 3바이트로 인식
-- DUAL 가상 테이블 사용

SELECT LENGTH('HELLO'), LENGTHB('HELLO')
FROM DUAL;

--INSTR
SELECT INSTR('AABBA','B') FROM DUAL; -- 왼쪽부터 B를 찾아 반환
SELECT INSTR('AABBA','B',1) FROM DUAL; -- 1번 인덱스부터 B를 찾아 반환
SELECT INSTR('AABBA','B',-1) FROM DUAL; -- 오른쪽 1번 인덱스부터 B를 찾아 반환
SELECT INSTR('AABBA','B',1,2) FROM DUAL; -- 1번 인덱스부터 B를 찾되, 2번째 B를 찾아라

--SUBSTR
-- 자바의 subString 개념
SELECT SUBSTR('HELLO MY NAME', 5,2) FROM DUAL; 
-- 5의 경우는 오른쪽부터 -5만큼 왼쪽으로 이동하되 문자열을 잘라내어 반환할때는
-- 오른쪽으로 2만큼 반환한다.
SELECT SUBSTR('HELLO MY NAME', -5,2) FROM DUAL;  

--LPAD, RPAD
SELECT LPAD('HELLO', 10, '@') FROM DUAL;
SELECT RPAD('HELLO', 10,'@') FROM DUAL;

--LTRIM, RTRIM
-- 주어진 컬럼명의 문자열 왼쪽 오른쪽에서 지정한 문자를 제거한 나머지 문자를 반환
SELECT LTRIM('    HELLO') FROM DUAL; -- 문자열을 생략할 경우 공백 인식
SELECT LTRIM('HELLO MY NAME', 'HELLO'), RTRIM('HELLO MY NAME', 'HELLO') FROM DUAL;
-- HELLO MY NAME이란 문자열에서 H,E,L,L,O를 하나씩 비교하여 왼쪽부터 제거
-- 왼쪽 M을 만나면 HELLO랑 겹치는 것이 하나도 없기 때문에 종료
-- HELLO를 넣었으나 비교할때는 H,E,L,L,O라는 문자 하나 하나를 읽음.
-- RTRIM은 오른쪽부터 비교한다.

--TRIM
SELECT TRIM ('    HELLO    ') FROM DUAL; -- 공백 제거
SELECT TRIM('@' FROM '@@@@HELLO@@@@') FROM DUAL; 
-- 지정한 @ 문자를 넣어 문자열의 앞 뒤 @를 모두 제거

--LOWER, UPPER, INITCAP
-- 각각 문자열을 소문자 대문자 앞글자만 대문자로 변경해주는 함수
SELECT LOWER('HELLO MY NAME') FROM DUAL;
SELECT UPPER('hello my name') FROM DUAL;
SELECT INITCAP('hello my name') FROM DUAL;

--CONCAT
-- 문자열을 합친다.
SELECT CONCAT('hello',' my name') FROM DUAL;
SELECT CONCAT('hello',' my name') || ' IS SQL' FROM DUAL;

--REPLACE
-- 특정 문자를 지정한 문자로 바꿔 반환한다
SELECT REPLACE('HELLO HELLO MY NAME', 'HELLO', 'HI') FROM DUAL;

--ABS, MOD 
-- 절대값과 나머지를 구하는 함수
SELECT ABS(-10.1) FROM DUAL;
SELECT MOD(10.9,3) FROM DUAL;

--ROUND, FLOOR, TRUNC, CEIL
-- 각각 반올림, 내림, 내림차림, 올림차림하는 함수
SELECT ROUND(123.456) FROM DUAL;
SELECT ROUND(123.956) FROM DUAL;

SELECT FLOOR(123.456) FROM DUAL;

SELECT TRUNC(123.456) FROM DUAL;
SELECT CEIL(123.456) FROM DUAL;
```

</br>

[[MSSQL] CEILING / FLOOR / ROUND 절상 / 절삭 / 반올림](https://developerking.tistory.com/20)

</br>

```sql
SELECT SYSDATE FROM DUAL; -- 시스템에 저장된 날짜 반환 함수

-- 날짜 포맷 변경
ALTER SESSION SET NLS_DATE_FORMAT = 'RR-MM-DD';
ALTER SESSION SET NLS_DATE_FORMAT = 'RR/MM/DD';
```

</br>

[https://samdo0812.tistory.com/58](https://samdo0812.tistory.com/58)

</br>

