2018.3.25 MySQL
### Ollivander's Inventory
https://www.hackerrank.com/challenges/harry-potter-and-wands/problem
power와 age가 동일한 지팡이들 중에서 가장 저렴한 것 추출. not evil할 것.



#### Ver.2.0
논리 흐름에 맞게 JOIN 순서 변경

```sql
SELECT
  w.id
  , c.age
  , c.coins_needed
  , c.power
FROM (
      SELECT
        code
        , power
        , age
        , MIN(coins_needed) AS coins_needed
      FROM (
            SELECT
              w.id
              , w.code
              , w.coins_needed
              , w.power
              , p.age
            FROM
              wands w
            JOIN
              wands_property p
              ON w.code = p.code
                 AND p.is_evil = 0
            ) sub
      GROUP BY
        code, power, age
      ) c
JOIN
  wands w
  ON c.code = w.code
     AND c.coins_needed = w.coins_needed
ORDER BY
  power DESC
  , age DESC
;
```

#### Ver.1.0
```sql
SELECT w.id, m.age, m.coins_needed, m.power
FROM wands AS w
JOIN (
      SELECT code, power, age, MIN(coins_needed) AS coins_needed
      FROM (
            SELECT w.code, w.coins_needed, w.power, p.age
            FROM wands w
            JOIN wands_property p
            ON w.code = p.code AND p.is_evil = 0
            ) not_evil
      GROUP BY code, power, age
      ) m
ON w.code = m.code
   AND w.coins_needed = m.coins
ORDER BY power DESC, age DESC;
```



1.
"The mapping between code and age is one-one"
age 대신 code를 맵핑키로 사용해야 하는 거였다.

2.
풀이 과정에서 아래 에러를 가장 많이 맞닥뜨린다.

ERROR 1055 (42000) at line 1: Expression #1 of SELECT list is not in GROUP BY
clause and contains nonaggregated column 'm.id' which is not functionally
dependent on columns in GROUP BY clause; this is incompatible with sql_mode=only_full_group_by

자동으로 full_group_by될 때 nonaggregated column을 섞어서 SELECT하는 습관을 고쳐야 한다.
