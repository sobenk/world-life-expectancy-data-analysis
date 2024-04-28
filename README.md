# Présentation de l'étude de cas : Analyse exploratoire des données de l'espérance de vie mondiale

Dans cette partie de l'étude de cas, nous avons effectué une analyse exploratoire des données sur l'espérerance de vie mondiale.
L'objectif est de découvrir des tendances et des insights pertinentes dans les données.

## Commentaires sur le code et explications

1. Nous avons commencé par examiner l'espérance de vie par pays et calculer l'augmentation de l'esperance de vie sur 15 ans.
```sql
SELECT Country,
MIN(`Life expectancy`), 
MAX(`Life expectancy`),
ROUND(MAX(`Life expectancy`) - MIN(`Life expectancy`),1) AS Life_Increase_15_Years
FROM world_life_expectancy
GROUP BY Country
HAVING MIN(`Life expectancy`) <> 0
AND MAX(`Life expectancy`) <> 0
ORDER BY Country DESC;
```

2. Ensuite, nous avons calculé l'espérance de vie moyenne par année pour déduire une tendance générale.
```sql
SELECT Year, ROUND(AVG(`Life expectancy`),2)
FROM world_life_expectancy
WHERE `Life expectancy` <> 0
AND `Life expectancy` <> 0
GROUP BY YEAR
ORDER BY YEAR;
```

3. Nous avons également examiné la tendance du PIB (variable _GDP_) par pays et son lien avec l'espérance de vie.
```sql
SELECT Country, ROUND(AVG(`Life expectancy`),1) AS Life_Exp, ROUND(AVG(GDP),1) AS GDP
FROM world_life_expectancy
GROUP BY Country
HAVING Life_Exp >0
ORDER BY GDP ASC;
```
Nous avons constaté qu'il existe uen corrélation entre le PIB et l'espérance de vie : une espérance de vie plus basse est généralement
associée à un PIB plus bas.

4. Enfin, nous avons utilisé une instruction __CASE__ pour catérogiser les pays en fonction de leur PIB.
```sql
SELECT
SUM(CASE WHEN GDP >= 1500 THEN 1 ELSE 0 END) High_GDP_Count,
AVG(CASE WHEN GDP >= 1500 THEN `Life expectancy` ELSE NULL END) High_GDP_Life_Expectancy,
SUM(CASE WHEN GDP <= 1500 THEN 1 ELSE 0 END) Low_GDP_Count,
AVG(CASE WHEN GDP <= 1500 THEN `Life expectancy` ELSE NULL END) Low_GDP_Life_Expectancy
FROM world_life_expectancy;
```
Nous avons calculé l'espérance de vie moyenne pour les pays avec un PIB élevé et un PIB faible.
On constate que l'espérance de vie est généralement plus élevé dans les pays avec un PIB elevé.
