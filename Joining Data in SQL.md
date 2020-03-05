

```python
#Inner join
# Select all columns from cities
SELECT *
FROM cities;
SELECT * 
FROM cities
  # 1. Inner join to countries
INNER JOIN countries
    # 2. Match on the country codes
ON cities.country_code = countries.code;
# 1. Select name fields (with alias) and region 
SELECT  cities.name AS city, 
        countries.name AS country, 
        countries.region AS region
FROM cities
    INNER JOIN countries
    ON cities.country_code = countries.code;
```


```python
#Inner join (2)
#You'll now explore a way to get data from both the countries and economies tables to examine the inflation rate for both 2010 and 2015.
# 3. Select fields with aliases
SELECT c.code AS country_code, name, year, inflation_rate
FROM countries AS c
  # 1. Join to economies (alias e)
  INNER JOIN economies AS e
    # 2. Match on code
    ON c.code = e.code;
```


```python
#Inner join (3)
#for each country, you want to get the country name, its region, and the fertility rate and unemployment rate for both 2010 and 2015.
# 4. Select fields
SELECT code, name, region, year, fertility_rate
  # 1. From countries (alias as c)
  FROM countries AS c
  # 2. Join with populations (as p)
  INNER JOIN populations AS p
    # 3. Match on country code
    ON c.code = p.country_code;
# 6. Select fields
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
  # 1. From countries (alias as c)
  FROM countries AS c
  # 2. Join to populations (as p)
  INNER JOIN populations AS p
    # 3. Match on country code
    ON c.code = p.country_code
  # 4. Join to economies (as e)
  INNER JOIN economies AS e
    # 5. Match on country code
    ON e.code = c.code;
# 6. Select fields
SELECT c.code, name, region, e.year, fertility_rate, unemployment_rate
  # 1. From countries (alias as c)
  FROM countries AS c
  # 2. Join to populations (as p)
  INNER JOIN populations AS p
    # 3. Match on country code
    ON c.code = p.country_code
  # 4. Join to economies (as e)
  INNER JOIN economies AS e
    # 5. Match on country code and year
    ON c.code = e.code
    AND e.year = p.year;
```


```python
#Inner join with using
# 4. Select fields
SELECT c.name AS country, continent, l.name AS language, official
  # 1. From countries (alias as c)
  FROM countries AS c
  # 2. Join to languages (as l)
  INNER JOIN languages AS l
    # 3. Match using code
    USING (code);
```


```python
#Self-join
#In this exercise, you'll use the populations table to perform a self-join to calculate the percentage increase in population from 2010 to 2015 for each country code!
# 4. Select fields with aliases
SELECT p1.country_code, p1.size AS size2010, p2.size AS size2015
# 1. From populations (alias as p1)
FROM populations AS p1
# 2. Join to itself (alias as p2)
INNER JOIN populations AS p2
# 3. Match on country code
ON p1.country_code = p2.country_code;
# 5. Select fields with aliases
SELECT p1.country_code,
       p1.size AS size2010,
       p2.size AS size2015
# 1. From populations (alias as p1)
FROM populations as p1
  # 2. Join to itself (alias as p2)
  INNER JOIN populations as p2
    # 3. Match on country code
    ON p1.country_code = p2.country_code
        # 4. and year (with calculation)
        AND p1.year = 2010 
        AND p2.year = 2015;
SELECT p1.country_code,
       p1.size AS size2010, 
       p2.size AS size2015,
       # 1. calculate growth_perc
       ((p2.size - p1.size)/p1.size * 100.0) AS growth_perc
# 2. From populations (alias as p1)
FROM populations AS p1
  # 3. Join to itself (alias as p2)
  INNER JOIN populations AS p2
    # 4. Match on country code
    ON p1.country_code = p2.country_code
        # 5. and year (with calculation)
        AND p1.year = p2.year - 5;
```


```python
#Case when and then
#Using the countries table, create a new field AS geosize_group that groups the countries into three groups:
#If surface_area is greater than 2 million, geosize_group is 'large'.
#If surface_area is greater than 350 thousand but not larger than 2 million, geosize_group is 'medium'.
#Otherwise, geosize_group is 'small'.
SELECT name, continent, code, surface_area,
    # 1. First case
    CASE WHEN surface_area > 2000000 THEN 'large'
        # 2. Second case
        WHEN surface_area > 350000 THEN 'medium'
        # 3. Else clause + end
        ELSE 'small' END
        # 4. Alias name
        AS geosize_group
# 5. From table
FROM countries;
```


```python
#Inner challenge
#You will now explore the relationship between the size of a country in terms of surface area and in terms of population using grouping fields created with CASE.
#Using the populations table focused only for the year 2015, create a new field aliased as popsize_group to organize population size into
#'large' (> 50 million), 'medium' (> 1 million), and 'small' groups.
#Select only the country code, population size, and this new popsize_group as fields.
SELECT country_code, size,
    # 1. First case
    CASE WHEN size > 50000000 THEN 'large'
        # 2. Second case
        WHEN size > 1000000 THEN 'medium'
        # 3. Else clause + end
        ELSE 'small' END
        # 4. Alias name
        AS popsize_group
# 5. From table
FROM populations
# 6. Focus on 2015
WHERE year = 2015;
```


```python
#Use INTO to save the result of the previous query as pop_plus. You can see an example of this in the countries_plus code in the assignment text. Make sure to include a ; at the end of your WHERE clause!
SELECT country_code, size,
    CASE WHEN size > 50000000 THEN 'large'
        WHEN size > 1000000 THEN 'medium'
        ELSE 'small' END
        AS popsize_group
# 1. Into table
INTO pop_plus
FROM populations
WHERE year = 2015;
#Then, include another query below your first query to display all the records in pop_plus using SELECT * FROM pop_plus; so that you generate results and this will display pop_plus in query result.
# 2. Select all columns of pop_plus
SELECT *
FROM pop_plus;
```


```python
#Keep the first query intact that creates pop_plus using INTO.
#Write a query to join countries_plus AS c on the left with pop_plus AS p on the right matching on the country code fields.
#Sort the data based on geosize_group, in ascending order so that large appears on top.
#Select the name, continent, geosize_group, and popsize_group fields.
SELECT country_code, size,
  CASE WHEN size > 50000000
            THEN 'large'
       WHEN size > 1000000
            THEN 'medium'
       ELSE 'small' END
       AS popsize_group
INTO pop_plus       
FROM populations
WHERE year = 2015;

# 5. Select fields
SELECT name, continent, geosize_group, popsize_group
# 1. From countries_plus (alias as c)
FROM countries_plus AS c
  # 2. Join to pop_plus (alias as p)
  INNER JOIN pop_plus AS p
    # 3. Match on country code
    ON c.code = p.country_code
# 4. Order the table    
ORDER BY geosize_group;
```


```python
#Left Join
#Fill in the code based on the instructions in the code comments to complete the inner join. Note how many records are in the result of the join in the query result tab.
-- Select the city name (with alias), the country code,
-- the country name (with alias), the region,
-- and the city proper population
SELECT c1.name AS city, code, c2.name AS country,
       region, city_proper_pop
-- From left table (with alias)
FROM cities AS c1
  -- Join to right table (with alias)
  INNER JOIN countries AS c2
    -- Match on country code
    ON c1.country_code = c2.code
-- Order by descending country code
ORDER BY code DESC;
#Change the code to perform a LEFT JOIN instead of an INNER JOIN. After executing this query, note how many records the query result contains.
SELECT c1.name AS city, code, c2.name AS country,
       region, city_proper_pop
FROM cities AS c1
  -- 1. Join right table (with alias)
  LEFT JOIN countries AS c2
    -- 2. Match on country code
    ON c1.country_code = c2.code
-- 3. Order by descending country code
ORDER BY c2.code DESC;
```


```python
#Left join (2)
#Perform an inner join. Alias the name of the country field as country and the name of the language field as language.
#Sort based on descending country name.
/*
5. Select country name AS country, the country's local name,
the language name AS language, and
the percent of the language spoken in the country
*/
SELECT c.name AS country, local_name, l.name AS language, percent
-- 1. From left table (alias as c)
FROM countries AS c
  -- 2. Join to right table (alias as l)
  INNER JOIN languages AS l
    -- 3. Match on fields
    ON c.code = l.code
-- 4. Order by descending country
ORDER BY country DESC;
#Perform an inner join. Alias the name of the country field as country and the name of the language field as language.
#Sort based on descending country name.
/*
5. Select country name AS country, the country's local name,
the language name AS language, and
the percent of the language spoken in the country
*/
SELECT c.name AS country, local_name, l.name AS language, percent
-- 1. From left table (alias as c)
FROM countries AS c
  -- 2. Join to right table (alias as l)
  LEFT JOIN languages AS l
    -- 3. Match on fields
    ON c.code = l.code
-- 4. Order by descending country
ORDER BY c.name DESC;
```


```python
#Left join (3)
#Begin with a left join with the countries table on the left and the economies table on the right.
#Focus only on records with 2010 as the year.
-- 5. Select name, region, and gdp_percapita
SELECT name, region, gdp_percapita
-- 1. From countries (alias as c)
FROM countries AS c
  -- 2. Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- 3. Match on code fields
    ON c.code = e.code
-- 4. Focus on 2010
WHERE year = 2010;
#Modify your code to calculate the average GDP per capita AS avg_gdp for each region in 2010.
#Select the region and avg_gdp fields.
-- Select fields
SELECT region, AVG(gdp_percapita) AS avg_gdp
-- From countries (alias as c)
FROM countries AS c
  -- Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- Match on code fields
    ON c.code = e.code
-- Focus on 2010
WHERE year = 2010
-- Group by region
GROUP BY region;
#Arrange this data on average GDP per capita for each region in 2010 from highest to lowest average GDP per capita.
-- Select fields
SELECT region, AVG(gdp_percapita) AS avg_gdp
-- From countries AS (alias as c)
FROM countries AS c
  -- Left join with economies (alias as e)
  LEFT JOIN economies AS e
    -- Match on code fields
    ON c.code = e.code
-- Focus on 2010
WHERE year = 2010
-- Group by region
GROUP BY region
-- Order by descending avg_gdp
ORDER BY avg_gdp DESC;
```


```python
#Right join
-- convert this code to use RIGHT JOINs instead of LEFT JOINs
/*
SELECT cities.name AS city, urbanarea_pop, countries.name AS country,
       indep_year, languages.name AS language, percent
FROM cities
  LEFT JOIN countries
    ON cities.country_code = countries.code
  LEFT JOIN languages
    ON countries.code = languages.code
ORDER BY city, language;
*/

SELECT cities.name AS city, urbanarea_pop, countries.name AS country,
       indep_year, languages.name AS language, percent
FROM languages 
  RIGHT JOIN countries 
    ON languages.code = countries.code
  RIGHT JOIN cities 
    ON countries.code = cities.country_code
ORDER BY city, language;
```


```python
#Full join
#Choose records in which region corresponds to North America or is NULL.
SELECT name AS country, code, region, basic_unit
-- 3. From countries
FROM countries
  -- 4. Join to currencies
  FULL JOIN currencies
    -- 5. Match on code
    USING (code)
-- 1. Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- 2. Order by region
ORDER BY region;
#Repeat the same query as above but use a LEFT JOIN instead of a FULL JOIN. Note what has changed compared to the FULL JOIN result!
SELECT name AS country, code, region, basic_unit
-- 1. From countries
FROM countries
  -- 2. Join to currencies
  LEFT JOIN currencies
    -- 3. Match on code
    USING (code)
-- 4. Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- 5. Order by region
ORDER BY region;
#Repeat the same query as above but use an INNER JOIN instead of a FULL JOIN. Note what has changed compared to the FULL JOIN and LEFT JOIN results!
SELECT name AS country, code, region, basic_unit
FROM countries
  -- 1. Join to currencies
  INNER JOIN currencies
    USING (code)
-- 2. Where region is North America or null
WHERE region = 'North America' OR region IS NULL
-- 3. Order by region
ORDER BY region;
```


```python
#Full join (2)
#Choose records in which countries.name starts with the capital letter 'V' or is NULL.
#Arrange by countries.name in ascending order to more clearly see the results.
SELECT countries.name, code, languages.name AS language
-- 3. From languages
FROM languages
  -- 4. Join to countries
  FULL JOIN countries
    -- 5. Match on code
    USING (code)
-- 1. Where countries.name starts with V or is null
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
-- 2. Order by ascending countries.name
ORDER BY countries.name;
#Repeat the same query as above but use a left join instead of a full join. Note what has changed compared to the full join result!
SELECT countries.name, code, languages.name AS language
FROM languages
  -- 1. Join to countries
  LEFT JOIN countries
    -- 2. Match using code
    USING (code)
-- 3. Where countries.name starts with V or is null
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
ORDER BY countries.name;
#Repeat once more, but use an inner join instead of a left join. Note what has changed compared to the full join and left join results.
SELECT countries.name, code, languages.name AS language
FROM languages
  -- 1. Join to countries
  INNER JOIN countries
    USING (code)
-- 2. Where countries.name starts with V or is null
WHERE countries.name LIKE 'V%' OR countries.name IS NULL
ORDER BY countries.name;
```


```python
#Full join (3)
#Complete a full join with countries on the left and languages on the right.
#Next, full join this result with currencies on the right.
#Use LIKE to choose the Melanesia and Micronesia regions (Hint: 'M%esia').
#Select the fields corresponding to the country name AS country, region, language name AS language, and basic and fractional units of currency.
-- 7. Select fields (with aliases)
SELECT c1.name AS country, region, l.name AS language,
       basic_unit, frac_unit
-- 1. From countries (alias as c1)
FROM countries AS c1
  -- 2. Join with languages (alias as l)
  FULL JOIN languages AS l
    -- 3. Match on code
    USING (code)
  -- 4. Join with currencies (alias as c2)
  FULL JOIN currencies AS c2
    -- 5. Match on code
    USING (code)
-- 6. Where region like Melanesia and Micronesia
WHERE region LIKE 'M%esia';
```


```python
#A table of two cities
#Create the cross join as described above. (Recall that cross joins do not use ON or USING.)
#Make use of LIKE and Hyder% to choose Hyderabad in both countries.
#Select only the city name AS city and language name AS language.
-- 4. Select fields
SELECT c.name AS city, l.name AS language
-- 1. From cities (alias as c)
FROM cities AS c        
  -- 2. Join to languages (alias as l)
  CROSS JOIN languages AS l
-- 3. Where c.name like Hyderabad
WHERE c.name LIKE 'Hyder%';
#Use an inner join instead of a cross join. Think about what the difference will be in the results for this inner join result and the one for the cross join.
-- 5. Select fields
SELECT c.name AS city, l.name AS language
-- 1. From cities (alias as c)
FROM cities AS c 
  -- 2. Join to languages (alias as l)
  INNER JOIN languages AS l
    -- 3. Match on country code
    ON c.country_code = l.code
-- 4. Where c.name like Hyderabad
WHERE c.name LIKE 'Hyder%';
```


```python
#Outer challenge
#Select country name AS country, region, and life expectancy AS life_exp.
#Make sure to use LEFT JOIN, WHERE, ORDER BY, and LIMIT
-- Select fields
SELECT c.name AS country, region, life_expectancy AS life_exp
-- From countries (alias as c)
FROM countries AS c
  -- Join to populations (alias as p)
  LEFT JOIN populations AS p
    -- Match on country code
    ON c.code = p.country_code
-- Focus on 2010
WHERE year = 2010
-- Order by life_exp
ORDER BY life_exp
-- Limit to 5 records
LIMIT 5;
```


```python
#Union
#Combine these two tables into one table containing all of the fields in economies2010. The economies table is also included for reference.
#Sort this resulting single table by country code and then by year, both in ascending order.
-- Select fields from 2010 table
SELECT *
  -- From 2010 table
  FROM economies2010
	-- Set theory clause
	UNION
-- Select fields from 2015 table
SELECT *
  -- From 2015 table
  FROM economies2015
-- Order by code and year
ORDER BY code, year;
```


```python
#Union (2)
#Determine all (non-duplicated) country codes in either the cities or the currencies table. The result should be a table with only one field called country_code.
#Sort by country_code in alphabetical order.
-- Select field
SELECT country_code
  -- From cities
  FROM cities
	-- Set theory clause
	UNION
-- Select field
SELECT code
  -- From currencies
  FROM currencies
-- Order by country_code
ORDER BY country_code;
```


```python
#Union all
#Determine all combinations (include duplicates) of country code and year that exist in either the economies or the populations tables. Order by code then year.
#The result of the query should only have two columns/fields. Think about how many records this query should result in.
-- Select fields
SELECT code, year
  -- From economies
  FROM economies
	-- Set theory clause
	UNION ALL
-- Select fields
SELECT country_code, year
  -- From populations
  FROM populations
-- Order by code, year
ORDER BY code, year;
```


```python
#Intersect
#Again, order by code and then by year, both in ascending order.
#Note the number of records here (given at the bottom of query result) compared to the similar UNION ALL query result (814 records).
-- Select fields
SELECT code, year
  -- From economies
  FROM economies
	-- Set theory clause
	INTERSECT
-- Select fields
SELECT country_code, year
  -- From populations
  FROM populations
-- Order by code and year
ORDER BY code, year;
```


```python
#Intersect (2)
#As you think about major world cities and their corresponding country, you may ask which countries also have a city with the same name as their country name?
#Use INTERSECT to answer this question with countries and cities!
-- Select fields
SELECT name
  -- From countries
  FROM countries
	-- Set theory clause
	INTERSECT
-- Select fields
SELECT name
  -- From cities
 FROM cities;
```


```python
#Except
#Get the names of cities in cities which are not noted as capital cities in countries as a single field result.
#Order the resulting field in ascending order.
#Can you spot the city/cities that are actually capital cities which this query misses?
-- Select field
SELECT name
  -- From cities
  FROM cities
	-- Set theory clause
	EXCEPT
-- Select field
SELECT capital
  -- From countries
  FROM countries
-- Order by result
ORDER BY name;
```


```python
#Except (2)
#Determine the names of capital cities that are not listed in the cities table.
#Order by capital in ascending order.
#The cities table contains information about 236 of the world's most populous cities. The result of your query may surprise you in terms of the number of capital cities that DO NOT appear in this list!
-- Select field
SELECT capital
  -- From countries
  FROM countries
	-- Set theory clause
	EXCEPT
-- Select field
SELECT name
  -- From cities
  FROM cities
-- Order by ascending capital
ORDER BY capital;
```


```python
#Semi-join
#You are now going to use the concept of a semi-join to identify languages spoken in the Middle East.
#selecting all country codes in the Middle East as a single field result using SELECT, FROM, and WHERE.
-- Select code
SELECT code
  -- From countries
  FROM countries
-- Where region is Middle East
WHERE region = 'Middle East';
#Comment out the answer to the previous tab by surrounding it in /* and */. You'll come back to it!
#Below the commented code, select only unique languages by name appearing in the languages table.
#Order the resulting single field table by name in ascending order.
/*SELECT code
  FROM countries
WHERE region = 'Middle East';*/

-- Select field
SELECT Distinct(name)
  -- From languages
  FROM languages
-- Order by name
ORDER BY name;
#Now combine the previous two queries into one query:
#Add a WHERE IN statement to the SELECT DISTINCT query, and use the commented out query from the first instruction in there. That way, you can determine the unique languages spoken in the Middle East.
-- Select distinct fields
SELECT DISTINCT(name)
  -- From languages
FROM languages
-- Where in statement
WHERE code IN
  -- Subquery
  (SELECT code
   FROM countries
   WHERE region = 'Middle East')
-- Order by name
ORDER BY name;
```


```python
#Diagnosing problems using anti-join
#Begin by determining the number of countries in countries that are listed in Oceania using SELECT, FROM, and WHERE.
-- Select statement
SELECT COUNT(*)
  -- From countries
  FROM countries
-- Where continent is Oceania
WHERE continent = 'Oceania';
#Complete an inner join with countries AS c1 on the left and currencies AS c2 on the right to get the different currencies used in the countries of Oceania.
#Match ON the code field in the two tables.
#Include the country code, country name, and basic_unit AS currency.
#Observe query result and make note of how many different countries are listed here.
-- 5. Select fields (with aliases)
SELECT c1.code, c1.name, basic_unit AS currency
  -- 1. From countries (alias as c1)
  FROM countries AS c1
  	-- 2. Join with currencies (alias as c2)
  	INNER JOIN currencies AS c2
    -- 3. Match on code
    ON c1.code = c2.code
-- 4. Where continent is Oceania
WHERE continent = 'Oceania';
#Note that not all countries in Oceania were listed in the resulting inner join with currencies. Use an anti-join to determine which countries were not included!
#Use NOT IN and (SELECT code FROM currencies) as a subquery to get the country code and country name for the Oceanian countries that are not included in the currencies table.
-- 3. Select fields
SELECT code, name
  -- 4. From Countries
  FROM Countries
  -- 5. Where continent is Oceania
  WHERE continent = 'Oceania'
  	-- 1. And code not in
  	AND code NOT IN
  	-- 2. Subquery
  	(SELECT code
  	 FROM currencies);
```


```python
#Set theory challenge
#Identify the country codes that are included in either economies or currencies but not in populations.
#Use that result to determine the names of cities in the countries that match the specification in the previous instruction.
-- Select the city name
SELECT name
  -- Alias the table where city name resides
  FROM cities AS c1
  -- Choose only records matching the result of multiple set theory clauses
  WHERE country_code IN
(
    -- Select appropriate field from economies AS e
    SELECT e.code
    FROM economies AS e
    -- Get all additional (unique) values of the field from currencies AS c2  
    UNION
    SELECT c2.code
    FROM currencies AS c2
    -- Exclude those appearing in populations AS p
    EXCEPT
    SELECT p.country_code
    FROM populations AS p
);
```


```python
#Subquery inside where
#Begin by calculating the average life expectancy across all countries for 2015.
-- Select average life_expectancy
SELECT AVG(life_expectancy)
  -- From populations
  FROM populations
-- Where year is 2015
WHERE year = 2015;
#Select all fields from populations with records corresponding to larger than 1.15 times the average you calculated in the first task for 2015. In other words, change the 100 in the example above with a subquery.
-- Select fields
SELECT *
  -- From populations
  FROM populations
-- Where life_expectancy is greater than
WHERE life_expectancy >
  -- 1.15 * subquery
  1.15 *
  (SELECT AVG(life_expectancy)
   FROM populations
   WHERE year = 2015)
   AND year = 2015
;
```


```python
#Subquery inside where (2)
#Make use of the capital field in the countries table in your subquery.
#Select the city name, country code, and urban area population fields.
-- 2. Select fields
SELECT name, country_code, urbanarea_pop
  -- 3. From cities
  FROM cities
-- 4. Where city name in the field of capital cities
WHERE name IN
  -- 1. Subquery
  (SELECT capital
   FROM  countries)
ORDER BY urbanarea_pop DESC;
```


```python
#Subquery inside select
SELECT countries.name AS country, COUNT(*) AS cities_num
  FROM cities
    INNER JOIN countries
    ON countries.code = cities.country_code
GROUP BY country
ORDER BY cities_num DESC, country
LIMIT 9;
```


```python
#Subquery inside from
#Begin by determining for each country code how many languages are listed in the languages table using SELECT, FROM, and GROUP BY.
#Alias the aggregated field as lang_num.
-- Select fields (with aliases)
SELECT code, COUNT(*) AS lang_num
  -- From languages
  FROM languages
-- Group by code
GROUP BY code;
#Include the previous query (aliased as subquery) as a subquery in the FROM clause of a new query.
#Select the local name of the country from countries.
#Also, select lang_num from subquery.
#Make sure to use WHERE appropriately to match code in countries and in subquery.
#Sort by lang_num in descending order.
-- Select fields
SELECT local_name, subquery.lang_num
  -- From countries
  FROM countries
  INNER JOIN
  	-- Subquery (alias as subquery)
  	(SELECT code, COUNT(*) AS lang_num
  	 FROM languages
  	 GROUP BY code) AS subquery
  -- Where codes match
  ON subquery.code = countries.code
-- Order by descending number of languages
ORDER BY lang_num DESC;
```


```python
#Advanced subquery
#Create an inner join with countries on the left and economies on the right with USING. Do not alias your tables or columns.
#Retrieve the country name, continent, and inflation rate for 2015.
-- Select fields
SELECT name, continent, inflation_rate
  -- From countries
  FROM countries
  	-- Join to economies
  	INNER JOIN economies
    -- Match on code
    USING (code)
-- Where year is 2015
WHERE year = 2015;
#Select the maximum inflation rate in 2015 AS max_inf grouped by continent using the previous step's query as a subquery in the FROM clause.
-- Select fields
SELECT MAX(inflation_rate) AS max_inf
  -- Subquery using FROM (alias as subquery)
  FROM (
      SELECT name, continent, inflation_rate
      FROM countries
      INNER JOIN economies
       USING (code)
      WHERE year = 2015) AS subquery
-- Group by continent
GROUP BY continent;
#Append the second part's query to the first part's query using WHERE, AND, and IN to obtain the name of the country, its continent, and the maximum inflation rate for each continent in 2015. Revisit the sample output in the assignment text at the beginning of the exercise to see how this matches up.
SELECT continent, countries.name, inflation_rate
FROM countries
INNER JOIN economies
USING (code)
WHERE inflation_rate IN
(
SELECT MAX(inflation_rate) AS max_inf
  FROM (
      SELECT name, continent, inflation_rate
      FROM countries
      INNER JOIN economies
       USING (code)
      WHERE year = 2015) AS subquery
GROUP BY continent
)
;
```


```python
#Subquery challenge
#Select the country code, inflation rate, and unemployment rate.
#Order by inflation rate ascending.
-- Select fields
SELECT economies.code, inflation_rate, unemployment_rate
  -- From economies
  FROM economies
  -- Where year is 2015 and code is not in
  WHERE year = 2015 AND code NOT IN
  	-- Subquery
  	(SELECT code
  	 FROM countries
  	 WHERE (gov_form = 'Constitutional Monarchy' OR gov_form LIKE '%Republic%'))
-- Order by inflation rate
ORDER BY inflation_rate;
```


```python
#Final challenge
#Select unique country names. Also select the total investment and imports fields.
#Use a left join with countries on the left. (An inner join would also work, but please use a left join here.)
#Match on code in the two tables AND use a subquery inside of ON to choose the appropriate languages records.
#Order by country name ascending.
#Use table aliasing but not field aliasing in this exercise.
-- Select fields
SELECT DISTINCT name, total_investment, imports
  -- From table (with alias)
  FROM countries AS c
    -- Join with table (with alias)
    LEFT JOIN economies AS e
      -- Match on code
      ON (c.code = e.code
      -- and code in Subquery
        AND c.code IN (
          SELECT l.code
          FROM languages AS l
          WHERE official = 'true'
        ) )
  -- Where region and year are correct
  WHERE region = 'Central America' AND year = 2015
-- Order by field
ORDER BY name;
```


```python
#Final challenge (2)
#Include the name of region, its continent, and average fertility rate aliased as avg_fert_rate.
#Sort based on avg_fert_rate ascending.
#Remember that you'll need to GROUP BY all fields that aren't included in the aggregate function of SELECT.
-- Select fields
SELECT region, continent, AVG(fertility_rate) AS avg_fert_rate
  -- From left table
  FROM countries AS c
    -- Join to right table
    INNER JOIN populations AS p
      -- Match on join condition
      ON c.code = p.country_code
  -- Where specific records matching some condition
  WHERE year = 2015
-- Group appropriately
GROUP BY continent, region
-- Order appropriately
ORDER BY avg_fert_rate;
```


```python
#Final challenge (3)
#Select the city name, country code, city proper population, and metro area population.
#Calculate the percentage of metro area population composed of city proper population for each city in cities, aliased as city_perc.
#Focus only on capital cities in Europe and the Americas in a subquery.
#Make sure to exclude records with missing data on metro area population.
#Order the result by city_perc descending.
#Then determine the top 10 capital cities in Europe and the Americas in terms of this city_perc percentage.
-- Select fields
SELECT name, country_code, city_proper_pop, metroarea_pop,  
      -- Calculate city_perc
      city_proper_pop / metroarea_pop * 100.0 AS city_perc
  -- From appropriate table
  FROM cities
  -- Where 
  WHERE name IN
    -- Subquery
    (SELECT capital
     FROM countries
     WHERE (continent = 'Europe'
        OR continent LIKE '%America%'))
       AND metroarea_pop IS NOT NULL
-- Order appropriately
ORDER BY city_perc DESC
-- Limit amount
LIMIT 10;
```
