
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

