---
title: SQL - GROUP BY와 HAVING (Oracle DB)
date: 2024-04-01 16:30:00 +09:00
categories: [DB, sql]
tags:
  [
    Data,
    DB,
    Datebase,
    oracle,
    sql,
  ]
---

## 1. GROUP BY

### group by와 집계함수

`group by`를 통해 그룹별로 집계함수를 적용할 수 있다.

```sql
SELECT deptno, avg(sal) FROM EMP GROUP BY deptno;

/*
30	1566
20	2615
10	2916
*/
```

<hr>

### group by와 컬럼 순서

`group by` 뒤 오는 컬렴명의 순서에 따라 결과값이 달라진다.
- 그룹화의 결과가 달라지기 때문

```sql
SELECT deptno, job, sum(sal) FROM EMP GROUP BY deptno, job;
/*
30	SALESMAN	5600
20	MANAGER	2975
20	CLERK	1100
30	CLERK	950
10	PRESIDENT	5000
30	MANAGER	2850
10	CLERK	1300
20	ANALYST	9000
10	MANAGER	2450
*/

SELECT deptno, job, sum(sal) FROM EMP GROUP BY job, deptno;
/*
20	MANAGER	2975
10	PRESIDENT	5000
10	CLERK	1300
30	SALESMAN	5600
20	ANALYST	9000
30	MANAGER	2850
10	MANAGER	2450
30	CLERK	950
20	CLERK	1100
*/
```

### GROUP BY의 실행순서

- `SELECT` : 4
- `FROM` : 1
- `WHERE` : 2
- `GROUP BY` : 3
- `ORDER BY` : 5

<hr><br>

## 2. HAVING

### HAVING과 GROUP BY

`WHERE` 절은 `GROUP BY`절보다 실행 순서가 느리다.
- `GROUP BY`로 생성되는 값으로는 조건을 걸 수 없다.

이 때 `HAVING`을 사용하여 그룹에 조건을 건다.

```sql
SELECT job, avg(sal)
FROM EMP
GROUP BY job
HAVING avg(sal) >= 3000;

/*
PRESIDENT	5000
ANALYST	3000
*/
```

<hr>

### HAVING의 실행 순서

- `SELECT` : 5
- `FROM` : 1
- `WHERE` : 2
- `GROUP BY` : 3
- `HAVING` : 4
- `ORDER BY` : 6

