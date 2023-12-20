# Exploratory Data Analysis (EDA)
You must understand the data before you start exploring it. I am skipping the details in the more obvious methods but will highlight them as a reminder of the functions that need to be done before exploring the data using Python. The data we will be working with is presented to us in the CSV format. 

1. Go through any write-up about the data. This data was taken from Kaggle and does come with some documentation. Please go through the [data card](https://www.kaggle.com/datasets/nadyinky/sephora-products-and-skincare-reviews) to understand the files and the data structures of the files.
2. Open it in Microsoft Excel. Often overlooked but one of the fastest ways to take a look at the data. Excel has a lot of functions like "Filters" which can give us a quick overview and counts of the data.
3. Use the DataFrame.shape function. This gives you the total number of records and columns in the DataFrame
  ```
  print(f'The shape of the dataframe is: {df.shape}')
  print(f'The number of records in the dataframe: {df.shape[0]}')
  print(f'The number of columns in the dataframe: {df.shape[1]}')
  ```
  ![](https://github.com/premthomas/retail_analytics/blob/cf64d6e36bbebc6f321d4a56f7045945dde28b5b/EDA/shape.JPG)
   
5. Using the [Pandas](https://pandas.pydata.org/docs/user_guide/index.html) "head" function to look at a sample set of data
  
      ![](https://github.com/premthomas/retail_analytics/blob/c9810208378b5ad81ff7ca8d8d82dc0c4d45a00d/EDA/head.JPG)
   
6. Using the "dtypes" method in Pandas to understand how Pandas has understood the data and the data types. Note that the column "submission_time" is a date-time object. 

      ![](https://github.com/premthomas/retail_analytics/blob/1683e0c0fc9dddeec38ddc94e26ce932da7b99b8/EDA/dtypes.JPG)

   Using the "astype" function, we can convert specific columns to the correct data types. 
   ```
   df['submission_time'] = pd.to_datetime(df['submission_time'])
   ```

      ![](https://github.com/premthomas/retail_analytics/blob/24386f00a11de4b337f41ad0cbf74a7db3d6381f/EDA/dtypes-after.JPG)
   
7. Using the "describe" function in Pandas, to understand the data

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

![](https://github.com/premthomas/retail_analytics/blob/f14ecf566edd23826fa602944b8bd174273e931b/EDA/amazon-review-example.JPG)

What is common is that both websites want an understanding of a particular review that was considered "helpful". The difference is that "Sephora" allows unverified users to give their input.

![](https://github.com/premthomas/retail_analytics/blob/9a3cfc6af73410c91b58fd4e8f69288e3f13a675/EDA/sephora-review-example3.JPG)

On Amazon, I am allowed to mark a review as "Helpful" but otherwise I am allowed to "Report" the review and provide an optional reason for the report. 

![](https://github.com/premthomas/retail_analytics/blob/ca28a68d4dd3a29258bc446cc6a787918d8b73b1/EDA/amazon-review-example2.JPG)

I believe that the "Amazon" design of collecting information on the "helpfulness" of a review is more robust compared to the "Sephora" design, which allows bad actors to push down reviews by marking them as "unhelpful". Considering "Sephora" does host a large collection of brands, a competitor might take advantage of this design flaw.

I might try and analyze this further to see if there is any evidence of review tampering. 

The next type of output coming from the SweetViz report is the variable-level analysis. Here is an example of the "ratings" field available in the data

![](https://github.com/premthomas/retail_analytics/blob/0b96abdc8ac2573e485262867e6e1a24a4b5a15a/EDA/SweetViz-Variable.JPG)

We see information such as 
  * The total number of values in the column
  * The total number of values that are missing
  * The number of distinct values
  * and The distribution of the values

Analysis at this level is important to understand the type of data, the distribution, and the amount of data that is missing. A large number of missing values which we cannot fill with default values, might not be useful for regression models. 

Outliers are another interesting bit of information we can get about some variables. For example, the average number of total_pos_feedback_count is 3. At 95%, it is 13. The max is a staggering 3481! We need to take a closer look at the graphs for both the "total_pos_feedback_count" and "total_neg_feedback_count" columns. 

## Sketch
A brilliant tool that is AI-backed to help understand the context of the data and provide suggestions. 
Here is an example of a question that I asked about the data

![](https://github.com/premthomas/retail_analytics/blob/a7a34ac7b2b6b56dc607575e30627dadfa9f8b64/EDA/sketch1.JPG)

Here is another sample question

<b>Question: </b> Is there a relationship between the "total_neg_feedback_count" and the "helpfulnesss" columns?

<b>Answer: </b> Yes, there is likely a relationship between the "total_neg_feedback_count" and the "helpfulness" columns. The higher the total negative feedback count, the lower the helpfulness rating is likely to be. This suggests that more negative feedback is associated with less helpful reviews.

The code example is as follows
```
import sketch

question = 'What does the "helpfulness" column tell us about the data?'
obj = df.sketch.ask(question, call_display=False)
print(f'\nQ:{question}')
print(f'{obj}')
```

This library cannot be used to directly answer questions about the data. However, it can create code that can help answer the question.
For example, 'Fetch the total number of records that have the "submission_time" values between "2022-01-01" and "2022-12-31"' does not work

But ask it to generate code to do this and it works!

![](https://github.com/premthomas/retail_analytics/blob/e317d05cead24da8d76adfe81fb440ba600482d1/EDA/sketch2.JPG)

![]()

![]()

## Conclusion
I am interested in understanding what makes a review "helpful" or "not helpful". There are about 16,000 reviews that are greater than 2 standard deviations away from the mean that might provide us with either indications or key phrases that are important. OR we might be able to understand if there is some foul play in these numbers. 








