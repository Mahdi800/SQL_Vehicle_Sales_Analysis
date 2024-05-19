# sql_vehicle_sales_analysis

****Excel****
Before I import the dataset to mysql, I first reduce the size the dataset in excel. Instead of working with ~500,000 rows of data, I limit the number to 9,435 rows. The new dataset only includes details for cars that are from 2015.
I used the filter function in excel to reduce the dataset size. 
**-Go to Data
-Filter
-Select the button that has appeared on the "year" column
-In the pop up, deselect 2015
-Delete all remaining the data 
-Now only data from 2015 is shown**

Deleting blank cells
commad A > control G > press "Special" > select "Blanks" > press "OK" > control - > select "Entire row" > press "OK"
7,993 rows remain.

**Importing Error**
I encountered an error when uploading the csv file to mysql. The error message was: "**MySQL Error: Unhandled exception: 'ascii' codec can't decode byte 0xef in position 0: ordinal not in range(128)**". The solution to this error is to convert the csv file to a json file. The code in the **csv2json.ipynb** file was used to achieve this.
