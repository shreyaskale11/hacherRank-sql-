# hacherRank-sql-

Find the difference between the total number of city entries in the table and the number of distinct city entries in the table.
```
SELECT COUNT(CITY) - COUNT(DISTINCT CITY) FROM STATION ;
```
Query the two cities in STATION with the shortest and longest CITY names, as well as their respective lengths (i.e.: number of characters in the name). If there is more than one smallest or largest city, choose the one that comes first when ordered alphabetically.
```
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY),CITY LIMIT 1;
SELECT CITY,LENGTH(CITY) FROM STATION ORDER BY LENGTH(CITY) DESC LIMIT 1;
```
