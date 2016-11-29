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

JOINS

1.
SELECT matchid, player FROM goal
WHERE teamid = 'GER'

2.
SELECT id,stadium,team1,team2
FROM game
WHERE id = 1012

3.
SELECT player,teamid, stadium, mdate
FROM game JOIN goal ON (id=matchid)
WHERE teamid = 'GER'

4.
SELECT team1, team2, player
FROM game JOIN goal ON game.id = goal.matchid
WHERE player LIKE 'Mario %'

5.
SELECT player, teamid, coach, gtime
FROM goal JOIN eteam ON teamid=id
WHERE gtime<=10

6.
SELECT mdate, teamname
FROM game JOIN eteam ON team1=eteam.id
WHERE coach = 'Fernando Santos'

7.
SELECT player
FROM game JOIN goal ON id=matchid
WHERE stadium='National Stadium, Warsaw'

8.
SELECT DISTINCT(player)
FROM game JOIN goal ON matchid = id
WHERE (team2='GER' OR team1='GER')
AND teamid != 'GER'

9.
SELECT teamname, COUNT(*) AS goals
FROM eteam JOIN goal ON id=teamid
GROUP BY teamname
ORDER BY goals

10.
SELECT stadium, COUNT(*) AS goals
FROM game JOIN goal ON id=matchid
GROUP BY stadium
ORDER BY goals

11.
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE (team1 = 'POL' OR team2 = 'POL')
GROUP BY matchid, mdate

12.
SELECT matchid, mdate, COUNT(*)
FROM game JOIN goal ON matchid = id
WHERE (teamid = 'GER')
GROUP BY matchid, mdate

13.
SELECT mdate, team1, SUM(score1) AS score1, team2, SUM(score2) AS score2
FROM ( SELECT mdate, id,
team1,
CASE WHEN teamid=team1 THEN 1 ELSE 0 END AS score1,
team2,
CASE WHEN teamid=team2 THEN 1 ELSE 0 END AS score2
FROM game LEFT OUTER JOIN goal ON matchid = id
) as score_table
GROUP BY id
ORDER BY mdate, id,team1, team2

SELECT mdate, team1,
SUM(CASE WHEN teamid=team1 THEN 1 ELSE 0 END) AS score1,
team2,
SUM(CASE WHEN teamid=team2 THEN 1 ELSE 0 END) AS score2
FROM game LEFT OUTER JOIN goal ON matchid = id
GROUP BY team1, team2, mdate, matchid


MORE JOIN OPERATIONS
1.
SELECT id, title
FROM movie
WHERE yr=1962

2.
SELECT yr
FROM movie
WHERE title='Citizen Kane'

3.
SELECT id, title, yr
FROM movie
WHERE title LIKE '%Star Trek%'
ORDER BY yr

4.
SELECT title
FROM movie
WHERE id IN (11768, 11955, 21191)

5.
SELECT id
FROM actor
WHERE name = 'Glenn Close'

6.
SELECT id
FROM movie
WHERE title = 'Casablanca'

7.
SELECT name
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actor.id = actorid
WHERE movieid = 11768

8.
SELECT name
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actor.id = actorid
WHERE title = 'Alien'

9.
SELECT title
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actor.id = actorid
WHERE name = 'Harrison Ford'

10.
SELECT title
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actor.id = actorid
WHERE name = 'Harrison Ford'
AND ord != 1

11.
SELECT title, name
FROM movie JOIN casting ON movie.id = movieid
JOIN actor ON actor.id = actorid
WHERE yr = 1962
AND ord = 1

12.
SELECT yr,COUNT(title) FROM
movie JOIN casting ON movie.id=movieid
JOIN actor   ON actorid=actor.id
WHERE name='John Travolta'
GROUP BY yr
HAVING COUNT(title) > 2

13.
SELECT title, name
FROM movie JOIN casting ON movie.id=movieid JOIN actor ON actorid=actor.id
WHERE movieid IN (
SELECT movieid FROM casting JOIN actor ON actorid=actor.id
WHERE name = 'Julie Andrews')
AND ord = 1

14.
SELECT name
FROM casting JOIN actor ON actorid=actor.id
WHERE ord = 1
GROUP BY name
HAVING COUNT(*) >= 30
ORDER BY name

15.
SELECT title, COUNT(*) AS actors
FROM movie JOIN casting ON movie.id=movieid
WHERE yr = 1978
GROUP BY movie.id, title
ORDER BY COUNT(*) DESC, title

16.
SELECT name
FROM actor JOIN casting ON actor.id=actorid JOIN movie ON movieid=movie.id
WHERE movieid IN (
SELECT movieid
FROM casting JOIN actor ON actorid=actor.id
WHERE name = 'Art Garfunkel'
)
AND name != 'Art Garfunkel'

------------- SUM AND COUNT --------------------------

1.
SELECT SUM(population)
FROM world

2.
SELECT DISTINCT(continent)
FROM world

3.
SELECT SUM(gdp)
FROM world
WHERE continent = 'Africa'

4.
SELECT COUNT(*)
FROM world
WHERE area >= 1000000

5.
SELECT SUM(population)
FROM world
WHERE name IN ('France','Germany','Spain')

6.
SELECT continent, COUNT(*) AS num_countries
FROM world
GROUP BY continent

7.
SELECT continent, COUNT(*) AS num_alot_people
FROM world
WHERE population >= 10000000
GROUP BY continent

8.
SELECT continent
FROM world
GROUP BY continent
HAVING SUM(population) >= 100000000

---------------- SELECT WITHIN SELECT --------------------
1.
SELECT name FROM world
WHERE population >
(SELECT population FROM world
WHERE name='Russia')

2.
SELECT name
FROM world
WHERE (gdp/population) > (SELECT (gdp/population) FROM world WHERE name = 'United Kingdom')
AND continent = 'Europe'

3.
SELECT name, continent
FROM world
WHERE continent IN (SELECT continent FROM world WHERE name IN ('Argentina', 'Australia'))
ORDER BY name

4.
SELECT name, population
FROM world
WHERE population > (SELECT population FROM world WHERE name ='Canada')
AND population < (SELECT population FROM world WHERE name='Poland')

5.
SELECT name, CONCAT(ROUND( population/(SELECT population FROM world WHERE name = 'Germany')*100, 0), '%')
FROM world
WHERE continent = 'Europe'

6.
SELECT name
FROM world
WHERE gdp > ALL(SELECT gdp FROM world WHERE continent ='Europe' AND gdp > 0)

7.
SELECT continent, name, area FROM world x
WHERE area >= ALL
(SELECT area FROM world y
WHERE y.continent=x.continent
AND area>0)

8.
SELECT continent, MIN(name)
FROM world
GROUP BY continent

9.
SELECT name, continent, population
FROM world x
WHERE 25000000 >= ALL (
SELECT population
FROM world y
WHERE x.continent = y.continent)

10.
SELECT name, continent
FROM world x
WHERE population > ALL(
SELECT population * 3
FROM world y
WHERE y.continent = x.continent AND population > 0
AND y.name != x.name)

-------------------------------------- USING NULL --------------------------------------

1.
SELECT name
FROM teacher
WHERE dept IS NULL

2.
SELECT teacher.name, dept.name
FROM teacher INNER JOIN dept
ON (teacher.dept=dept.id)

3.
SELECT teacher.name, dept.name
FROM teacher LEFT OUTER JOIN dept ON dept=dept.id

4.
SELECT teacher.name, dept.name
FROM dept LEFT OUTER JOIN teacher ON dept.id=dept

5.
SELECT name, COALESCE(mobile, '07986 444 2266')
FROM teacher

6.
SELECT teacher.name, COALESCE(dept.name, 'None')
FROM teacher LEFT JOIN dept ON dept=dept.id

7.
SELECT COUNT(*) AS num_teachers, COUNT(mobile)
FROM teacher

8.
SELECT dept.name, COUNT(teacher.dept) AS staff
FROM teacher RIGHT JOIN dept ON dept=dept.id
GROUP BY dept.name

9.
SELECT name,
CASE WHEN dept IN(1,2) THEN 'Sci'
ELSE 'Art' END
FROM teacher

10.
SELECT name,
CASE WHEN dept IN(1,2) THEN 'Sci'
WHEN dept = 3 THEN 'Art'
ELSE 'None' END
FROM teacher

--------------------------------------------SELF JOIN--------------------------------------------

1.
SELECT COUNT(*)
FROM stops

2.
SELECT id
FROM stops
WHERE name='Craiglockhart'

3.
SELECT id, name
FROM stops JOIN route ON stops.id = route.stop
WHERE num = 4 AND company = 'LRT'

4.
SELECT company, num, COUNT(*)
FROM route WHERE stop=149 OR stop=53
GROUP BY company, num
HAVING COUNT(*) = 2

5.
SELECT a.company, a.num, a.stop, b.stop
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
WHERE a.stop=53 AND b.stop=(SELECT id FROM stops WHERE name = 'London Road')

6.
SELECT a.company, a.num, stopa.name, stopb.name
FROM route a JOIN route b ON
  (a.company=b.company AND a.num=b.num)
  JOIN stops stopa ON (a.stop=stopa.id)
  JOIN stops stopb ON (b.stop=stopb.id)
WHERE stopa.name='Craiglockhart'
AND stopb.name = 'London Road'

7.
SELECT a.company, a.num
FROM route a JOIN route b on a.company = b.company AND a.num = b.num
WHERE a.stop = 115 AND b.stop = 137
GROUP BY a.company, a.num

8.
SELECT a.company, a.num
FROM route a JOIN route b on a.company = b.company AND a.num = b.num
                        JOIN stops stop_a ON a.stop = stop_a.id JOIN stops stop_b ON b.stop = stop_b.id
WHERE stop_a.name = 'Craiglockhart' AND stop_b.name = 'Tollcross'
GROUP BY a.company, a.num

9.
SELECT name, company, num
FROM route JOIN stops ON route.stop = stops.id
WHERE company = 'LRT'
AND num IN (SELECT num FROM route JOIN stops ON stop = id WHERE name = 'Craiglockhart')
GROUP BY name, company, num
