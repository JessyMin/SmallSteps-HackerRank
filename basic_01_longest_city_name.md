2018.3.18 MySQL
https://www.hackerrank.com/challenges/weather-observation-station-5/problem

### 길이가 가장 긴 도시명과 글자수 추출하기

```sql
SELECT
  city,
  CHAR_LENGTH(city)
FROM
  station
WHERE
  CHAR_LENGTH(city) = (SELECT MAX(CHAR_LENGTH(city)) FROM station)
ORDER BY
  city
LIMIT 1;
```

<br>

#### 시행착오 쿼리

```sql
SELECT
  city,
  CHAR_LENGTH(city)
FROM
  station
WHERE
  CHAR_LENGTH(city) = MAX(CHAR_LENGTH(city)
ORDER BY
  city
LIMIT 1;
```


__ERROR 1111__ (HY000) at line 1: Invalid use of group function
