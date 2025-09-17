# MLB Player Analysis using MySQL


## 1. SCHOOLS: What Schools do MLB Players attend
 ### 1.1 In each decade, how many schools were there that produced MLB players?
```sql
WITH 	cte_decade AS (
SELECT	yearID, schoolID, playerid,
		FLOOR(yearID/10) * 10 AS decade
FROM	schools
ORDER BY yearID 
)
SELECT	decade,
        COUNT(distinct schoolid) as school_count
FROM	cte_decade
WHERE	decade > 1900
GROUP BY decade
ORDER BY decade;
```
![1.1Output](Images/1.1output.png)

 
 ### 1.2 What are the names of the top 5 schools that produced the most players?
 ```sql
with cte_school AS (
				SELECT	schoolid, count(DISTINCT playerid) AS players
				FROM	schools
				GROUP BY schoolid
				ORDER BY players desc
				)
,
cte_ranking as (
				SELECT schoolid, players,
						RANK() OVER (order by players DESC) as ranking
				FROM cte_school
				)

SELECT	a.schoolid, b.name_full as School_name, players
FROM	cte_ranking a LEFT JOIN school_details b 
		ON a.schoolid = b.schoolid
WHERE	ranking <=5
```

![1.2Output](Images/1.2output.png)

 
 3.	For each decade, what were the names of the top 3 schools that produced the most players?

## 2. SALARIES: How much do teams spend on Player salaries
 1.	Return the top 20% of teams in terms of average annual spending
 2.	For each team, show the cumulative sum of spending over the years
 3.	 Return the first year that each team's cumulative spending surpassed 1 billion

## 3. CAREER: What does each player's career look like
 1.	For each player, calculate their age at their first (debut) game, their last game, and their career length.
 2.	What team did each player play on for their starting and ending years?
 3.	How many players started and ended on the same team and also played for over a decade?

## 4. SUMMARY STATS: How do player attributes compare
 1.	Which players have the same birthday?
 2.	Create a summary table that shows for each team, what percent of players bat right, left and both
 3.	How have average height and weight at debut game changed over the years, and what's the decade-over-decade difference?
