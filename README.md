# SQL Vehicle Sales Analysis

## Excel
Before I imported the dataset to mysql, I first reduced the size the dataset in excel. Instead of working with ~500,000 rows of data, I limited the number to 9,435 rows. The new dataset only includes details for cars that are from 2015.
I used the filter function in excel to reduce the dataset size.<br>
-Go to Data\
-Filter\
-Select the button that has appeared on the "year" column\
-In the pop up, deselect 2015\
-Delete all remaining the data\
-Now only data from 2015 is shown\

Deleting blank cells\
-commad A \
-control G \
-press "Special"  \
select "Blanks" \
press "OK" \
control - \
select "Entire row" \
press "OK" \
7,993 rows remain.

## Importing Error
I encountered an error when uploading the csv file to mysql. The error message was: "**MySQL Error: Unhandled exception: 'ascii' codec can't decode byte 0xef in position 0: ordinal not in range(128)**". The solution to this error is to convert the csv file to a json file. The code in the **csv2json.ipynb** file was used to achieve this.

## Code Breakdwon
### Query 1
#Showing each car brand in the table and their count \
select make, count(*) as 'Number of cars' \
from cars2015.mytable \
group by make; 


### Query 2
#Finding out the relation between condition and milage. \
#My assumption is that the higher the milage, the higher the condition score would be. \
select mytable.condition, round(avg(odometer),0) as average_milage \
from cars2015.mytable \
where mytable.condition <> "" \
group by mytable.condition \
order by mytable.condition+0;


### Query 3
#Finding the brand with the highest aveage condition \
select make \
from cars2015.mytable \
group by make \
order by avg(mytable.condition) desc \
limit 1;


### Query 4
#Finding the most popular brand in each state \
select state, make \
from ( \
    select state, make, \
           ROW_NUMBER() over (partition by state order by COUNT(*) desc) as brand_rank \
    from cars2015.mytable \
    group by state, make \
) as ranked_brands \
where brand_rank = 1;


### Query 5
#Finding the distribution of color for the cars (most popular colors) \
SELECT color, COUNT(*) AS count \
FROM cars2015.mytable \
GROUP BY color \
order by count desc; 


### Query 6 
#Showing the most popular color for each car brand \
SELECT make, color \
FROM ( \
    SELECT make, color, \
           ROW_NUMBER() OVER (PARTITION BY make ORDER BY COUNT(*) DESC) AS row_num \
    FROM cars2015.mytable \
    GROUP BY make, color \
) AS ranked_colors \
WHERE row_num = 1;


### Query 7
#Showing the average body type for each car brand \
SELECT make, body \
FROM cars2015.mytable \
GROUP BY make, body;


### Query 8
#Showing the highest and lowest selling prices \
SELECT make, model, sellingprice \
FROM cars2015.mytable \
WHERE CAST(sellingprice AS UNSIGNED) = (SELECT MAX(CAST(sellingprice AS UNSIGNED)) FROM cars2015.mytable) \
   or CAST(sellingprice AS UNSIGNED) = (SELECT MIN(CAST(sellingprice AS UNSIGNED)) FROM cars2015.mytable);


### Query 9 
#Showing the conditions of the highest and lowest priced cars \
SELECT make, model, CAST(sellingprice AS UNSIGNED) AS sellingprice, t.condition \
FROM cars2015.mytable t \
JOIN ( \
    SELECT MAX(CAST(sellingprice AS UNSIGNED)) AS max_price, MIN(CAST(sellingprice AS UNSIGNED)) AS min_price \
    FROM cars2015.mytable \
) AS prices \
ON CAST(t.sellingprice AS UNSIGNED) = prices.max_price OR CAST(t.sellingprice AS UNSIGNED) = prices.min_price \
ORDER BY t.condition + 0;


### Query 10
#Showing the relation between selling price and condition \
SELECT mytable.condition, round(AVG(sellingprice), 2) AS average_selling_price \
FROM cars2015.mytable \
GROUP BY mytable.condition \
order by mytable.condition+0;


### Query 11
#Displaying expensive and affordable cars \
SELECT make, model, sellingprice, \
    CASE \
        WHEN sellingprice > 80000 THEN 'Expensive' \
        WHEN sellingprice < 40000 THEN 'Affordable' \
        ELSE 'Average' \
    END AS price_category \
FROM cars2015.mytable \
order by make;
