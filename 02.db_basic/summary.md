### Inner Join

- 두 테이블의 공통 데이터만
  - key가 같게만 매칭
  - 외래키 제약조건 때문에 실제로 다른 key가 매칭되지는 않는다.
- `INNER`, `OUTER`는 생략 가능

```SQL
SELECT
	u.user_id,
	u.name,
	o.order_date
FROM orders o
INNER JOIN users u
ON o.user_id = u.user_id
WHERE o.status = "COMPLETED";
```

- 한 테이블에만 있는 필드일 경우 테이블명 생략가능
  - 유지보수 관점에서 헷갈릴수 있어 명시해주는데 좋다.
- 내부적으로 쿼리 최적화(`WHERE`이 먼제 필터링)가 발생할 수 있음
- 교집합, A On B, B On A 동일 결과
- 어떤 데이터를 중심으로 보 는지에 따라서 순서 결정

### Outer Join

- 한쪽에만 존재하는 데이터까지 매칭
  - LEFT JOIN, RIGHT JOIN
  - 없는 데이터는 NULL로 채움
- 쇼핑몰에 가입은 했지만 주문하지 않은 고객
- 출시했지만 한 번도 팔리지 않은 상품

```SQL
SELECT
	*
FROM users u
LEFT JOIN orders o
ON u.user_id = o.user_id
WHERE o.order_id is null;
```

- 자식 -> 부모 : 행 유지
- 부모 -> 자식 : 행 증가

### 셀프 조인

- 자기 자신과 조인
- 같은 테이블의 컬럼을 조회

### 크로스 조인

```SQL
SELECT
    s.size,
    c.color
FROM size s
CROSS JOIN colors c;
```
