# Exploratory Data Analysis (EDA)
You must understand the data before you start exploring it. I am skipping the details in the more obvious methods but will highlight them as a reminder of the functions that need to be done before exploring the data using Python. The data we will be working with is presented to us in the CSV format. 

1. Go through any write-up about the data. This data was taken from Kaggle and does come with some documentation. Please go through the [data card](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews) to understand the files and the data structures of the files.
2. Open it in Microsoft Excel. Often overlooked but one of the fastest ways to take a look at the data. Excel has a lot of functions like "Filters" which can give us a quick overview and counts of the data.
3. Using the [Pandas](https://pandas.pydata.org/docs/user_guide/index.html) "head" function to look at a sample set of data
  
      ![](https://github.com/premthomas/retail_analytics/blob/c9810208378b5ad81ff7ca8d8d82dc0c4d45a00d/EDA/head.JPG)
   
4. Using the "dtypes" method in Pandas to understand how Pandas has understood the data and the data types. Note that the column "submission_time" is a date-time object. 

      ![](https://github.com/premthomas/retail_analytics/blob/1683e0c0fc9dddeec38ddc94e26ce932da7b99b8/EDA/dtypes.JPG)

   Using the "astype" function, we can convert specific columns to the correct data types. 
   ```
   df['submission_time'] = pd.to_datetime(df['submission_time'])
   ```

      ![](https://github.com/premthomas/retail_analytics/blob/24386f00a11de4b337f41ad0cbf74a7db3d6381f/EDA/dtypes-after.JPG)
   
5. Using the "describe" function in Pandas, to understand the data

      ![](https://github.com/premthomas/retail_analytics/blob/5f1149bff859851a0f6379d09ef4b8dae085b8c9/EDA/describe.JPG)

   This step helps us understand the numeric data. Parameters such as "count" show the number of records that have a value. "Min" and "Max" variables help us understand if there is a categorical or boolean value. For the numeric data types, it shows us the mean, standard deviation, and quartile information.

So far, we have used some of the standard libraries and functions to understand the data. 
Here are some of the more interesting ways for us to explore the data

There are multiple tools to help us understand the data quickly and with little code. These tools are advanced to the point they can create HTML reports about the data and some that can answer questions about the data. The two libraries I will be exploring are 
  1. SweetViz (https://github.com/fbdesignpro/sweetviz)
  2. Sketch (https://github.com/approximatelabs/sketch)

## SweetViz
For some of the more complex analyses, the data must be correct. In the example, you can see that the "author_id" is not being recognized and has to be explicitly changed to "string". 
These resolutions are provided by SweetViz and are helpful for us to quickly change the data to execute and create the SweetViz report. 

```
# Correction of data types as suggested by the resolutions
df['author_id'] = df['author_id'].astype(str)
```

Upon correction of the data, execute the code below at it creates an HTML report. 

```
# SweetViz
import sweetviz as sv

report = sv.analyze(df)
report.show_html(filepath='SweetViz Report.html',
                 open_browser=True,
                 layout='widescreen',
                 scale=None
                 )
```
You can download a copy of the report [here](https://github.com/premthomas/retail_analytics/blob/f1652ae6a589a077157e69ffdbe8e2c521f0522b/EDA/SweetViz%20Report.html). But let's do through some of the interesting bits.

Correlations can be done with a simple Python statement.

```
df.corr()
```

BUT, it is important to understand that correlations can be done with numeric (continuous) variables. Comparing correlations for categorical variables can be a little different. For a deeper understanding, I recommend the following article on [Medium](https://medium.com/@outside2SDs/an-overview-of-correlation-measures-between-categorical-and-continuous-variables-4c7f85610365) by [Outside Two Standard Deviations](https://medium.com/@outside2SDs)

This is where we see an interesting outlook of the data by SweetViz

![](https://github.com/premthomas/retail_analytics/blob/8eb866f0727d4c4174f387d89095b3c82b4b7624/EDA/SweetViz-Associations.JPG)

Unlike other heatmaps, you notice that correlations are represented not only by the size and color of the object but also a shape.

SweetViz understands the different types of data and uses different algorithms based on the type of variables it is comparing. By using the uncertainty coefficient, it compares categorical values and shows their relationship. For example, we see that there is a relationship between the variables "rating" and "is_recommended".

What is interesting is that the relationship between "rating" and "is_recommended" is not the same as the relationship between "is_recommended" and "rating". This is asymmetrical. "rating" provides more information about the "is_recommended" field than "is_recommended" does about "rating". To explain this with an example, a review with a rating of 3 might or might not be recommended. But we can say with a high probability that a 5-star rated product would be marked to be recommended.

The other interesting titbit we gathered from the heatmap is the relationship between "helpfulness" and "total_neg_feedback_count". Besides being the only relationship that has a negative correlation (relationships that are inversely correlated), it implies that if the total_neg_feedback_count is high, the helpfulness score is low. Again, obvious if you have read the data card which states that "helpfulness" is a ratio of "total_pos_feedback_count" and "total_neg_feedback_count". 

Let's take a look at a review on the "Sephora" website and compare it with a review on "Amazon".

<b>Review from the Sephora website</b>

![](https://github.com/premthomas/retail_analytics/blob/097d84ddbf9bac36f46d1a46600c0e8efe010909/EDA/sephora-review-example2.JPG)

<b>Review from the Amazon website</b>

![]()






