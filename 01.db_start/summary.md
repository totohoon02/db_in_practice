### DBMS

- 데이터베이스 관리 시스템
- 데이터를 처리하는 방법을 추상화
- 개발자는 SQL만 알면 된다. 내부 처리는 몰라도 됨.
- DBMS도 내부적으로는 파일로 데이터베이스 관리

### HAVING

```SQL
SELECT
    category,
    SUM(price * quantity) AS total_sales
FROM
    order_stat
WHERE
    SUM(price * quantity) >= 500000
GROUP BY
    category;

--- SQL Error [1111] [HY000]: Invalid use of group function
```

- `WHERE`은 `GROUP BY` 이전 실행
  - 그룹화 전 개별 행에 대한 조건 검사
- `FROM` -> `WHERE` -> `GROUP BY` -> `...` -> `HAVING` -> `SELECT` -> `ORDER BY`

```SQL
SELECT
	category,
	SUM(price * quantity) AS total_sales
FROM
	order_stat
GROUP BY
	category
HAVING
	total_sales >= 500000;
```

- `GROUP BY` 이후 실행
- HAVING에 별칭은 원칙적으로는 사용 불가(`SELECT`가 후순위)
  - mysql, postgresql는 사용자 편의를 위해 지원
- 집계 함수를 사용 가능

#### 성능상의 차이

- `WHERE`: 그룹화(GROUP BY)를 하기 전에 전체 테이블에서 조건에 맞지 않는 행들을 먼저 제거
- `HAVING`: 일단 모든 데이터를 가져와서 그룹화(GROUP BY)를 완료한 후, 생성된 그룹 결과 집합에서 조건을 만족하는 그룹만 필터
  - Disc I/O: `WHERE` < `HAVING`
  - Index: `WHERE` O | `HAVING` X

> `WHERE`에서 레코드를 최대한 먼저 깎아내고, 남은 데이터로 `GROUP BY`, 최종 집계 조건만 `HAVING`으로 필터링
