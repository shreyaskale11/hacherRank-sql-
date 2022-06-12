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

Query the list of CITY names starting with vowels (i.e., a, e, i, o, or u) from STATION. Your result cannot contain duplicates.
```
SELECT CITY
FROM STATION
WHERE LEFT(CITY , 1) IN ('a','e','i','o','u','A','E','I','O','U');
```
RIGHT
```
SELECT DISTINCT CITY
FROM STATION
WHERE RIGHT(CITY , 1) IN ('a','e','i','o','u','A','E','I','O','U');
```
Query the list of CITY names from STATION that do not start with vowels. Your result cannot contain duplicates.
```
SELECT DISTINCT CITY FROM STATION 
WHERE CITY RLIKE '^[^aeiouAEIOU].*$';
```
Query the list of CITY names from STATION that do not end with vowels. Your result cannot contain duplicates.
```
SELECT DISTINCT CITY FROM STATION 
WHERE RIGHT(CITY,1) RLIKE '^[^aeiouAEIOU]';
```
Query the list of CITY names from STATION that either do not start with vowels or do not end with vowels. Your result cannot contain duplicates.
```
SELECT DISTINCT CITY
FROM STATION
WHERE RIGHT(CITY,1) RLIKE '^[^aeiouAEIOU]'
OR LEFT(CITY,1) RLIKE '^[^aeiouAEIOU]';
```
Query the Name of any student in STUDENTS who scored higher than  Marks. Order your output by the last three characters of each name. If two or more students both have names ending in the same last three characters (i.e.: Bobby, Robby, etc.), secondary sort them by ascending ID.

```
SELECT NAME
FROM STUDENTS
WHERE MARKS>75
ORDER BY RIGHT(NAME,3),ID;
```
### Use of Min() and Max()
The first thing you can notice is the list of SELECT columns: name, price, category, delivered_year. Next is the MIN(price) aggregate function, which finds the lowest value in the price column. OVER is what makes this a window function; it defines the window. or the set of rows within the query result set. This allows us to calculate an aggregate value for each row in the window. Here, OVER is paired with ORDER BY category DESC (i.e. in descending order); thus, the minimum price is always $3, because the lowest price in the “hair” category is $3, which is lower than the next category minimum of $5.
```
SELECT name, price, category, delivered_year,
  MIN(price) OVER (ORDER BY category DESC) AS min_price
FROM cosmetics;
```
https://learnsql.com/blog/sql-min-max-functions/

This query calculates the minimum price for each partition based on the delivered_year column and sorts rows by the category.
```
SELECT name, price, category, delivered_year,
  MIN(price) OVER (PARTITION BY delivered_year
            ORDER BY category DESC)
  AS min_price
FROM cosmetics;
```
