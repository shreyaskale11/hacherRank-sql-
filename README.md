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
Samantha was tasked with calculating the average monthly salaries for all employees in the EMPLOYEES table, but did not realize her keyboard's  key was broken until after completing the calculation. She wants your help finding the difference between her miscalculation (using salaries with any zeros removed), and the actual average salary.
Write a query calculating the amount of error, and round it up to the next integer.
```
select round(avg(salary))  -  round( avg( replace(salary,'0','')))
from employees;
```
We define an employee's total earnings to be their monthly  worked, and the maximum total earnings to be the maximum total earnings for any employee in the Employee table. Write a query to find the maximum total earnings for all employees as well as the total number of employees who have maximum total earnings. Then print these values as  space-separated integers.
```
select max(months * salary) ,count(employee_id)
from employee 
where (months * salary)  = (select max(months * salary) 
                           from Employee) ;
 ```                          

Query the Western Longitude (LONG_W) for the largest Northern Latitude (LAT_N) in STATION that is less than 137.2345. Round your answer to  decimal places.
```
select round(long_w,4) from station where lat_n in (select max(lat_n)from station where lat_n<137.2345);
```

Query the Western Longitude (LONG_W)where the smallest Northern Latitude (LAT_N) in STATION is greater than . Round your answer to  decimal places.
```
select round( long_w,4) from station where lat_n in (select min(lat_n) from station where lat_n>38.7780);
```

Consider  and  to be two points on a 2D plane.

 happens to equal the minimum value in Northern Latitude (LAT_N in STATION).
 happens to equal the minimum value in Western Longitude (LONG_W in STATION).
 happens to equal the maximum value in Northern Latitude (LAT_N in STATION).
 happens to equal the maximum value in Western Longitude (LONG_W in STATION).
Query the Manhattan Distance between points  and  and round it to a scale of  decimal places.
```
select round((abs(min(lat_n)-max(lat_n)) + abs(min(long_w)-max(long_w))),4) from station
```
Consider  and  to be two points on a 2D plane where  are the respective minimum and maximum values of Northern Latitude (LAT_N) and  are the respective minimum and maximum values of Western Longitude (LONG_W) in STATION.

Query the Euclidean Distance between points  and  and format your answer to display  decimal digits.
```
select round(sqrt(  power(min(lat_n)-max(lat_n),2)+ power(min(long_w)-max(long_w),2)),4)from station
```

A median is defined as a number separating the higher half of a data set from the lower half. Query the median of the Northern Latitudes (LAT_N) from STATION and round your answer to  decimal places.

```
SELECT ROUND(S1.LAT_N, 4) FROM STATION AS S1 
WHERE (SELECT ROUND(COUNT(S1.ID)/2) - 1 FROM STATION) = 
    (SELECT COUNT(S2.ID) FROM STATION AS S2 WHERE S2.LAT_N > S1.LAT_N);
```

Write a query to output the names of those students whose best friends got offered a higher salary than them. Names must be ordered by the salary amount offered to the best friends. It is guaranteed that no two students got same salary offer.
```
select name from students  s 
join packages p1 on p1.id = s.id
join friends f on f.id=s.id
join packages p2 on f.friend_id=p2.id
where p2.salary>p1.salary
order by p2.salary;
```


Two pairs (X1, Y1) and (X2, Y2) are said to be symmetric pairs if X1 = Y2 and X2 = Y1.

Write a query to output all such symmetric pairs in ascending order by the value of X. List the rows such that X1 ≤ Y1.
```
SELECT X, Y FROM (
SELECT X, Y FROM Functions WHERE X=Y GROUP BY X, Y HAVING COUNT(*)=2
UNION
SELECT f1.X, f1.Y FROM Functions f1, Functions f2 
WHERE f1.X < f1.Y 
AND f1.X=f2.Y 
AND f2.X=f1.Y
)t
ORDER BY X, Y;
```

Samantha interviews many candidates from different colleges using coding challenges and contests. Write a query to print the contest_id, hacker_id, name, and the sums of total_submissions, total_accepted_submissions, total_views, and total_unique_views for each contest sorted by contest_id. Exclude the contest from the result if all four sums are .
```
select con.contest_id,
        con.hacker_id, 
        con.name, 
        sum(total_submissions), 
        sum(total_accepted_submissions), 
        sum(total_views), sum(total_unique_views)
from contests con 
join colleges col on con.contest_id = col.contest_id 
join challenges cha on  col.college_id = cha.college_id 
left join
(select challenge_id, sum(total_views) as total_views, sum(total_unique_views) as total_unique_views
from view_stats group by challenge_id) vs on cha.challenge_id = vs.challenge_id 
left join
(select challenge_id, sum(total_submissions) as total_submissions, sum(total_accepted_submissions) as total_accepted_submissions from submission_stats group by challenge_id) ss on cha.challenge_id = ss.challenge_id
    group by con.contest_id, con.hacker_id, con.name
        having sum(total_submissions)!=0 or 
                sum(total_accepted_submissions)!=0 or
                sum(total_views)!=0 or
                sum(total_unique_views)!=0

            order by contest_id;
```

## find duplicate
To find duplicates in multiple column values, we can use the following query. It’s very similar to the one for a single column:
```
SELECT OrderID, ProductID, COUNT(*)
FROM OrderDetails
GROUP BY OrderID, ProductID
HAVING COUNT(*) > 1
```
