
### 순차 리스트 생성하기 (since mysql 5.8)

```mysql
WITH RECURSIVE 테이블명 AS (
  SELECT 초기값 AS 컬럼명
  UNION ALL
  SELECT 표현식 FROM 테이블명 WHERE 조건
)
```
가상 테이블을 생성하여 재귀 쿼리로 컬럼에 들어갈 값을 생성한다. <br>
표현식에서는 초기값이 어떻게 변할지를 명시해주고 조건에는 컬럼에 들어갈 제한값을 명시해준다. 

<br>

1에서부터 100까지의 숫자 리스트를 만들고 싶다면, 
```mysql
WITH RECURSIVE numbers AS (
  SELECT 1 AS no
  UNION ALL
  SELECT no+1 FROM numbers WHERE no < 100
)

SELECT * FROM numbers;
```
이런식으로 사용하면 된다. 


<br>

**관련 테스트** <br>
프로그래머스 코딩테스트 연습 - [MySql] 입양 시각 구하기(2) <br>
https://programmers.co.kr/learn/courses/30/lessons/59413

<br>

참고로 oracle에서는 DUAL 테이블과 connect by 함수를 이용해 만들어 쓸 수 있다.
```oracle
SELECT * FROM DUAL CONNECT BY LEVEL < 100;
```

<br>

### 정규 표현식 사용하여 조회하기

가끔 LIKE 검색을 IN절 처럼 사용하고 싶을때가 있다. <br>
그럴 떄 REGEXP 함수를 사용하면 된다. <br>

- REGEXP() : https://dev.mysql.com/doc/refman/5.6/en/regexp.html
- REGEXP_LIKE() (since mysql 8.0) : https://dev.mysql.com/doc/refman/8.0/en/regexp.html


```mysql
SELECT menu_no
    FROM menus
    WHERE menu_name REGEXP "로제|짜장|궁중"
```
위 쿼리를 수행하면 menus 테이블에 <br> 
메뉴 이름이 "로제" 혹은 "짜장" 혹은 "궁중" 이라는 단어가 들어간 ROW가 전부 조회될 것이다. 

<br>

이와 같이 간단하게 LIKE IN을 구현할수도 있고 <br>
LIKE 함수처럼 정규식 패턴을 활용하여 검색 조건을 구체화할 수 도 있다. 


```mysql


```


<br>

### INSERT 시 중복되는 키에 대해 업데이트하기 

INSERT 시 해당 테이블에 같은 키(PRIMARY KEY 혹은 UNIQUE KEY)를 가진 ROW가 존재한다면, <br> 
새롭게 INSERT하지 않고 해당 ROW에 대해 UPDATE 할 수 있다.  <br> 

> ON DUPLICATE KEY UPDATE 

<br> 

PRIMARY KEY가 no이고 ROW의 업데이트 날짜를 update_date라는 컬럼에서 관리하고 있다면 아래와 같이 사용할 수 있다. 

```mysql
INSERT INTO sample (no, name, create_date) VALUES (1, 'momo', now())
ON DUPLICATE KEY
UPDATE update_date = now()

```

no가 1인 ROW가 존재하는 경우, 해당 ROW의 update_date 컬럼값을 현재 시간(now())으로 업데이트한다. 

