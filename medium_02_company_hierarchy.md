2018.3.20 MySQL
'New Companies' challenge
https://www.hackerrank.com/challenges/the-company/problem




#### Ver.2.0
가독성 고려. JOIN하려는 두 테이블의 company_code 개수는 동일하므로 순서 무관. EXPLAIN 결과도 동일.

```sql
SELECT
    c.company_code
  , c.founder
  , e.l_manager_total
  , e.s_manager_total
  , e.manager_total
  , e.employee_total
FROM (
    SELECT
      company_code,
      COUNT(DISTINCT(lead_manager_code)) AS l_manager_total,
      COUNT(DISTINCT(senior_manager_code)) AS s_manager_total,
      COUNT(DISTINCT(manager_code)) AS manager_total,
      COUNT(DISTINCT(employee_code)) AS employee_total
    FROM employee
    GROUP BY company_code
      ) AS e
JOIN
  company AS c
  ON c.company_code = e.company_code
ORDER BY
  company_code
;
```



#### Ver.1.0
```sql
SELECT
    c.company_code
  , c.founder
  , e.l_manager_total
  , e.s_manager_total
  , e.manager_total
  , e.employee_total
FROM
  company AS c
JOIN (
    SELECT company_code,
      COUNT(DISTINCT(lead_manager_code)) AS l_manager_total,
  	  COUNT(DISTINCT(senior_manager_code)) AS s_manager_total,
  	  COUNT(DISTINCT(manager_code)) AS manager_total,
  	  COUNT(DISTINCT(employee_code)) AS employee_total
  	FROM employee
  	GROUP BY company_code
      ) AS e
  ON c.company_code = e.company_code
ORDER BY
  company_code
;
```
