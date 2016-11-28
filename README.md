# sql-zoo-practice

Section 14

1. SELECT population FROM world
WHERE name = 'Germany'

2.
SELECT name, population FROM world
WHERE name IN ('Ireland', 'Iceland', 'Denmark');

3.
SELECT name, area FROM world
WHERE area BETWEEN 200000 AND 250000

Select from world

2.
SELECT name FROM world
WHERE population>200000000

3.
SELECT name, (gdp/population) AS per_capita_GDP
FROM world
WHERE population > 200000000

4.
SELECT name, ( population / 1000000 ) AS pop_in_millions
FROM world
WHERE continent = 'South America'

5.
SELECT name, population
FROM world
WHERE name IN ('France', 'Germany', 'Italy')

6.
SELECT name
FROM world
WHERE name LIKE '%United%'

7.
SELECT name,  population, area
FROM world
WHERE area > 3000000
OR population > 250000000

8.
SELECT name,  population, area
FROM world
WHERE area > 3000000
XOR population > 250000000

9.
SELECT name,  
ROUND((population/1000000), 2) ,
ROUND( gdp/1000000000 , 2 )
FROM world
WHERE continent = 'South America'

10.
SELECT name,  
ROUND((gdp/population), -3)
FROM world
WHERE gdp > 1000000000000

11.
SELECT name,
CASE WHEN continent='Oceania' THEN 'Australasia'
ELSE continent END
FROM world
WHERE name LIKE 'N%'

12.
SELECT name,
CASE WHEN continent IN ('Europe', 'Asia') THEN 'Eurasia'
WHEN continent IN ('North America', 'South America', 'Caribbean') THEN 'America'
ELSE continent END
FROM world
WHERE name LIKE 'A%'
OR name LIKE 'B%'
