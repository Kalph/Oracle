## JOIN

* 여러 테이블에 흩어져 있는 정보 중 사용자가 필요한 정보만 가져와 

 테이블처럼 만들어서 결과를 보여주는 것. 

* 하나 이상의 테이블에서 데이터를 조회하기 위해 사용한다.

<br/>

```sql
SELECT ROOM_ID, ROOM_NAME, ROOM_CODE
FROM LIVINGROOM, SMALLROOM
WHERE LIVINGROOM.ROOM_CODE = SMALL_ROOM.ROOM_CODE;

-- 별칭 사용
SELECT ROOM_ID, ROOM_NAME, ROOM_CODE, L.ROOM_CODE, S.ROOM_CODE
FROM LIVINGROOM L, SMALLROOM S
WHERE L.ROOM_CODE = S.ROOM_CODE;

--ANSI 구문
SELECT ROOM_ID, ROOM_NAME
FROM LIVINGROOM
JOIN SMALLROOM USING(ROOM_CODE);

-- 연결에 사용한 컬럼명이 다를 경우
SELECT ROOM_ID, ROOM_NAME, L.VIP_ROOM, S.CUS_ROOM
FROM LIVINGROOM L
JOIN NORMALROOM N ON(L.ROOM_CODE=N.NORMAL_ID);
```

<br/>

* OUTER JOIN - LEFT, RIGHT(일치하지 않는 행도 조인에 포함시킴)

* 교차 조인 - CROSS(곱집합)

* 비등가 조인 - =를 사용하지 않는 조인

* 자체 조인 - 같은 테이블을 조인하는 것

* 다중 조인 - N개의 테이블을 조회할 때 사용

[[SQL] Join(조인)](https://clairdelunes.tistory.com/22)<br/>
