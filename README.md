# Programmers-SQL-Kit
SQL kit in programmers(MySQL)

## SELECT
> 모든 레코드 조회하기
```SQL
SELECT * from ANIMAL_INS order by ANIMAL_ID
```
> 역순 정렬하기
```SQL
SELECT NAME, DATETIME FROM ANIMAL_INS ORDER BY ANIMAL_ID DESC
```
> 아픈 동물 찾기
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS WHERE INTAKE_CONDITION = 'Sick' ORDER BY ANIMAL_ID
```
> 어린 동물 찾기
```SQL
SELECT ANIMAL_ID, NAME FROM ANIMAL_INS where not INTAKE_CONDITION = 'Aged' ORDER BY ANIMAL_ID
```

> 동물의 아이디와 이름
```SQL
SELECT ANIMAL_ID, NAME from ANIMAL_INS ORDER BY ANIMAL_ID
```

> 여러 기준으로 정렬하기
```SQL
SELECT ANIMAL_ID, NAME, DATETIME FROM ANIMAL_INS ORDER BY NAME, DATETIME DESC
```
## SUM, MAX, MIN
> 최댓갑 구하기
```SQL
SELECT MAX(DATETIME) FROM ANIMAL_INS
```
> 최솟값 구하기
```SQL
SELECT MIN(DAtETIME) FROM ANIMAL_INS
```
> 동물 수 구하기
```SQL
SELECT count(*) from ANIMAL_INS
```
> 중복 제거하기
```SQL
SELECT count(distinct name) FROM ANIMAL_INS
```

## GROUP BY
> 고양이와 개는 몇 마리 있을까?
```SQL
SELECT animal_type, count(animal_type) FROM ANIMAL_INS GROUP BY animal_type order by animal_type
```

> 동명 동물 수 찾기
```SQL
SELECT name, count(name) FROM ANIMAL_INS GROUP BY name having count(name) >= 2 order by name
```


> 입양 시각 구하기(1)
```SQL
SELECT HOUR(DATETIME) AS HOUR, count(*) AS COUNT 
FROM ANIMAL_OUTS WHERE HOUR(DATETIME) BETWEEN 9 and 19
group by HOUR(DATETIME)
order by HOUR(DATETIME)
```


> 입양 시각 구하기(2)
```SQL
SET @HOUR = -1;

SELECT 
(@HOUR := @HOUR + 1) AS HOUR,
(SELECT count(*) FROM ANIMAL_OUTS WHERE @HOUR = HOUR(datetime)) AS COUNT
FROM ANIMAL_OUTS
where @HOUR < 23
```

## IS NULL
> 이름이 없는 동물의 아이디
```SQL
SELECT animal_id FROM ANIMAL_INS WHERE name is null
```
> 이름이 있는 동물의 아이디
```SQL
SELECT animal_id FROM ANIMAL_INS WHERE name is not null order by animal_id
```

> NULL 처리하기
```SQL
SELECT
ANIMAL_TYPE,
IFNULL(name, "No name") AS NAME,
SEX_UPON_INTAKE
FROM ANIMAL_INS
ORDER BY animal_id
```

## JOIN
> 없어진 기록 찾기
```SQL
SELECT ANIMAL_ID, NAME
FROM animal_outs 
WHERE animal_id 
NOT IN (select animal_id from animal_ins)
```
> 있었는데요 없었습니다
```SQL
SELECT A.ANIMAL_ID, A.NAME 
FROM ANIMAL_INS A
JOIN ANIMAL_OUTS B
ON (A.ANIMAL_ID = B.ANIMAL_ID)
WHERE A.DATETIME > B.DATETIME 
ORDER BY A.DATETIME
```
> 오랜 기간 보호한 동물(1)
```SQL
SELECT NAME, DATETIME 
FROM ANIMAL_INS 
WHERE ANIMAL_ID NOT IN (SELECT ANIMAL_ID FROM ANIMAL_OUTS)
ORDER BY DATETIME LIMIT 3
```
> 보호소에서 중성화한 동물
```SQL
SELECT A.ANIMAL_ID, A.ANIMAL_TYPE, A.NAME FROM ANIMAL_OUTS A
JOIN ANIMAL_INS B ON (A.ANIMAL_ID = B.ANIMAL_ID) 
WHERE NOT A.SEX_UPON_OUTCOME = B.SEX_UPON_INTAKE
ORDER BY A.ANIMAL_ID
```

## String, Date
> 루시와 엘라 찾기
```SQL
SELECT ANIMAL_ID, NAME, SEX_UPON_INTAKE FROM ANIMAL_INS
WHERE NAME IN ('Lucy', 'Ella', 'Pickle', 'Sabrina', 'Mitty')
```
> 이름에 el들어가는 동물 찾기
> 중성화 여부 파악하기
