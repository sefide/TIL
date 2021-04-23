_**Q. row가 하나여도 Full Index Scan이 될 수 있다 ?**_

그렇다. 테이블에는 row가 하나고, 인덱스가 걸린 컬럼을 기준으로 SELECT문을 수행하는 경우, Full Index Scan가 일어난다. <br>
row 1개를 유지하는 테이블은 하나 있는 row와 비교할 다른 row가 없기 때문에 당연히 카디널리티가 낮다. 
하나의 row만 유지하는 테이블에서 그 Primary Key 이외의 컬럼을 생성하고 해당 컬럼을 통해 값을 조회하는 것이 좋다.


