# Exploratory Data Analysis (EDA)
You must understand the data before you start exploring it. I am skipping the details in the more obvious methods but will highlight them as a reminder of the functions that need to be done before exploring the data using Python. The data we will be working with is presented to us in the CSV format. 

1. Open it in Microsoft Excel
2. Using the [Pandas](https://pandas.pydata.org/docs/user_guide/index.html) Library in Python
3. Using the "head" function to look at a sample set of data
   
   ![](https://github.com/premthomas/retail_analytics/blob/c9810208378b5ad81ff7ca8d8d82dc0c4d45a00d/EDA/head.JPG)
   
4. Within the Pandas library, use the "dtypes" method to understand how Pandas has understood the data and the data types. Note that the column "submission_time" is a date-time object. Using the "astype" function, we can convert specific columns to the correct data types. 

<b>Before:</b>

   ![](https://github.com/premthomas/retail_analytics/blob/1683e0c0fc9dddeec38ddc94e26ce932da7b99b8/EDA/dtypes.JPG)

<b>After:</b>

   
6. Using the "describe" function, to understand the data

![](https://github.com/premthomas/retail_analytics/blob/5f1149bff859851a0f6379d09ef4b8dae085b8c9/EDA/describe.JPG)

There are multiple tools to help us understand the data quickly and with little code. These tools are advanced to the point they can create HTML reports about the data and some that can answer questions about the data. The two libraries I will be exploring are 
  1. SweetViz (https://github.com/fbdesignpro/sweetviz)
  2. Sketch (https://github.com/approximatelabs/sketch)
