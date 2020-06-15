# NBA Breakdown
This project is a data visualisation project done in Tableau on players in the NBA.

## Dataset
Dataset can be obtained from https://data.world/carolinadata/basketball, by writing SQL queries on the website to parse the required data.

To obtain the first dataset (adding positions to the dataset),
```sql
SELECT t1.*, t2.position
FROM seasons_stats AS t1
JOIN (SELECT player, year, ws,
       CASE
         WHEN REGEX(pos, '^C.*') THEN 'Center'
         WHEN REGEX(pos, '^F.*') THEN 'Forward'
         WHEN REGEX(pos, '^PF.*') THEN 'Forward'
         WHEN REGEX(pos, '^SF.*') THEN 'Forward'
         WHEN REGEX(pos, '^G.*') THEN 'Guard'
         WHEN REGEX(pos, '^SG.*') THEN 'Guard'
         WHEN REGEX(pos, '^PG.*') THEN 'Guard'
       END AS position
       FROM seasons_stats) t2 ON t1.player = t2.player AND t1.year = t2.year AND t1.ws = t2.ws
```

To obtain the second dataset (aggregated data),
```sql
SELECT year,
       AVG(age) AS averageAge,
       SUM(seasons_stats.2p) / SUM(seasons_stats.2pa) AS average2PTPct, 
       SUM(seasons_stats.3p) / SUM(seasons_stats.3pa) AS average3PTPct,
       SUM(seasons_stats.pts) / SUM(seasons_stats.mp) * 48 * 5 AS averagePointsPerGame,
       SUM(seasons_stats.trb_2) / SUM(seasons_stats.mp) * 48 * 5 AS averageReboundsPerGame,
       SUM(seasons_stats.ast_2) / SUM(seasons_stats.mp) * 48 * 5 AS averageAssistsPerGame,
       SUM(seasons_stats.stl_2) / SUM(seasons_stats.mp) * 48 * 5 AS averageStealsPerGame,
       SUM(seasons_stats.blk_2) / SUM(seasons_stats.mp) * 48 * 5 AS averageBlocksPerGame    
FROM seasons_stats
GROUP BY year
ORDER BY year
```

## Data Visualisation
Open `NBA.twbx` and click _Presentation Mode_ or press F7 to view the three Stories, 'Scoring Breakdown', 'Win Shares' and 'Team Stats'.

## Author
Chua Jing Yang