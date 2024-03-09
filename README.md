# sql_vehicle_sales_analysis
I encountered an error when uploading the csv file to mysql. The error message was: "MySQL Error: Unhandled exception: 'ascii' codec can't decode byte 0xef in position 0: ordinal not in range(128)". The solution to this error is to convert the csv file to a json file. The code in the csv2json.ipynb file was used to achieve this.
