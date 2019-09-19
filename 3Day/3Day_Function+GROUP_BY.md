</br>

```sql
--NVL 
SELECT NVL(ROOM_CUSTOMER,0) FROM LIVINGROOM; --ROOM_CUSTOMER이 NULL일때 0으로 변경.
SELECT NVL(null,'값 존재X') FROM DUAL;
SELECT NVL(1,'값 존재X') FROM DUAL; -- 오류 발생 컬럼으로 숫자가 들어갈 수 없음
-- 애초 숫자가 들어갈 경우라면 값이 존재하는 경우이므로 NVL을 사용할 필요 없음

--NVL2
SELECT NVL2(ROOM_CUSTOMER,0,1) FROM LIVINGROOM; -- ROOM_CUSTOMER이 NULL이라면 0 아닐 경우 1로 변경
SELECT NVL2(null,'값 존재X','값 존재') FROM DUAL;
SELECT NVL2(1,'값 존재X','값 존재') FROM DUAL;

--NULLIF
-- 두 개 값 비교 - 동일 시 NULL, 비동일시 전자의 매개변수를 리턴
SELECT NULLIF('123','123') FROM DUAL;
SELECT NULLIF('123','1234') FROM DUAL;

SELECT NULLIF(123,'123') FROM DUAL; -- 오류 발생 문자 숫자 비교 불가능. 문자를 숫자로 만들어주기
SELECT NULLIF(TO_NUMBER('123'),123) FROM DUAL; -- 해결

--DECODE
-- 선택함수, SWITCH문과 같은 원리
SELECT DECODE('123','123','123임','1234','1234임') FROM DUAL;

-- 해당하는 칼럼이 없다면 NULL 리턴
SELECT DECODE('123','123','123임','1234','1234임') FROM DUAL;

--CASE-WHEN-END
SELECT ROOM_PRICE
    CASE WHEN SALARY > 5000 THEN '1'
        WHEN SALARY > 3500 THEN '2'
        WHEN SALARY > 2000 THEN '3'
        ELSE '4'     
    END 방등급
FROM LIVINGROOM;

--SUM
-- ROOM_PRICE의 모든 합계 리턴
SELECT SUM(ROOM_PRICE) FROM LIVINGROOM;

--AVG
-- ROOM_PRICE의 모든 합계 평균 리턴
SELECT AVG(ROOM_PRICE) FROM LIVINGROOM;

--MIN
-- ROOM_PRICE의 가장 작은 값 리턴 (꼭 정수를 비교하지 않음 어떤 값이든 최소값을 뽑아낼 수 있다.)
SELECT AVG(ROOM_RRICE) FROM LIVINGROOM;

-- ROOM_PRICE의 가장 큰 값 리턴 (MIN과 마찬가지)
SELECT MAX(ROOM_PRICE) FROM LIVINGROOM;

-COUNT()
-- * - 행 개수 리턴
-- 컬럼 - NULL 제외 값 리턴
-- DISTINCT 컬럼 - 중복제거 행 개수 리턴
SELECT COUNT(*), COUNT(ROOM_CUSTOMER), COUNT(DISTINCT ROOM_CUSTOMER)
FROM LIVINGROOM;
```
</br>

## GROUP BY

</br>

SELECT한 컬럼에 대해 정렬하기 위해 작성하는 구문

</br>

### 실행 순서

* 5 : SELECT 컬렴명 AS 별칭, 계산식, 함수식

* 1 : FROM 참조할 테이블명

* 2 : WHERE 컬럼명 | 함수식 비교연산자 비교값

* 3 : GROUP BY 그룹을 묶을 컬럼명

* 4 : HAVING 그룹함수식 비교연산자 비교

* 6 : ORDER BY 컬럼명 | 별칭 | 컬럼순번 정렬방식 [NULLS FIRST | LAST ]

</br>

```sql
SELECT ROOM_NAME, SUM(ROOM_PRICE)
FROM LIVINGROOM;
-- 오류 발생 :  not a single-group group function
-- 오류 그룹 함수 SUM은한 개의 결과값만 산출함. 그룹이 여러개인 경우 그에 맞춰 GROUP BY 절 
-- 사용이 필요

SELECT ROOM_NAME, SUM(ROOM_PRICE)
FROM LIVINGROOM
GROUP BY ROOM_NAME;

-- 룸 이름별 가격 평균이 5000원 이상 조회
-- 그룹화 해야 된다는 오류 발생.
--> 우선순위에 따른 오류 -
SELECT ROOM_NAME, FLOOR(AVG(ROOM_PRICE)) 평균
FROM LIVINGROOM
WHERE FLOOR(AVG(ROOM_RRICE)) >= 5000
GROUP BY ROOM_NAME
ORDER BY 1;

-- HAVING을 이용함으로 해결.
SELECT ROOM_NAME, FLOOR(AVG(ROOM_PRICE)) 평균
FROM LIVINGROOM
GROUP BY ROOM_NAME
HAVING FLOOR(AVG(ROOM_PRICE) >= 5000
ORDER BY 1;

-- ROLLUP, CUBE

-- 각 방별 가격의 합과 그 합을 더한 총합이 출력
SELECT ROOM_NAME, SUM(ROOM_PRICE)
FROM LIVINGROOM
GROUP BY ROLLUP(ROOM_NAME)
ORDER BY 1;

SELECT ROOM_NAME, SUM(ROOM_PRICE)
FROM LIVINGROOM
GROUP BY CUBE(ROOM_NAME)
ORDER BY 1;
```

</br>

[[ oracle 10g ] 오라클 고급SQL -그룹핑 함수들](https://m.blog.naver.com/PostView.nhn?blogId=minis24&logNo=80100555203&proxyReferer=https%3A%2F%2Fwww.google.com%2F)

</br>

```sql
--SET OPTION 

--UNION
-- 중복 영억 제거해서 합친다.
SELECT *
FROM LIVINGROOM
WHERE ROOM_NAME LIKE 'A%';

UNION

SELECT *
FROM LIVINGROOM
WHERE ROOM_PRICE > 2500;

--INTERSECT
-- 공통 부분만 결과로 추출(교집합)
SELECT *
FROM LIVINGROOM
WHERE ROOM_NAME LIKE 'A%';

INTERSECT

SELECT *
FROM LIVINGROOM
WHERE ROOM_PRICE > 2500;

--UNION ALL
-- 결과를 하나로 합침, 중복도 포함
SELECT *
FROM LIVINGROOM
WHERE ROOM_NAME LIKE 'A%';

UNION ALL

SELECT *
FROM LIVINGROOM
WHERE ROOM_PRICE > 2500;

--MINUS
-- 전자 SELECT 결과에서 후자 SELECT에 겹치는 부분을 제외한 나머지 부분만 리턴
-- A로 시작하는 결과물들 중 2500을 넘지 않는 결과만 리턴
SELECT *
FROM LIVINGROOM
WHERE ROOM_NAME LIKE 'A%';

UNION

SELECT *
FROM LIVINGROOM
WHERE ROOM_PRICE > 2500;
```

</br>

[GROUPING SET](http://www.gurubee.net/lecture/2680) </br>
[GROUPING SETS](https://thebook.io/006696/part01/ch05/04/06/) </br>
