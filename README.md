# Amazon_Vine_Analysis

## Overview of the analysis

## Results

### Perform ETL on Amazon Product Reviews

```sql
SELECT COUNT(*) FROM review_id_table;
SELECT * FROM review_id_table FETCH FIRST 10 ROW ONLY;
```



![1_review_id_table_count](Resources/1_review_id_table_count.png "Figure 1 - Count of review_id_table")

***Figure 1 - Count of review_id_table***





![2_Top_Rows_of_review_id_table](Resources/2_Top_Rows_of_review_id_table.png "Figure 2 - Top Rows of review_id_table")

***Figure 2 - Top Rows of review_id_table***





```sql
SELECT COUNT(*) FROM products_table;
SELECT * FROM products_table FETCH FIRST 10 ROW ONLY;
```





![1_review_id_table_count](Resources/3_products_table_count.png "Figure 3 - Count of products_table")

***Figure 3 - Count of products_table***





![4_Top_Rows_of_products_table](Resources/4_Top_Rows_of_products_table.png "Figure 4 - Top Rows of product_table")

***Figure 4 - Top Rows of products_table***





```sql
SELECT COUNT(*) FROM customers_table;
SELECT * FROM customers_table FETCH FIRST 10 ROW ONLY;
```



![5_customers_table_count](Resources/5_customers_table_count.png "Figure 5 - Count of customers_table")

***Figure 5 - Count of customers_table***









![6_Top_Rows_of_customers_table](Resources/6_Top_Rows_of_customers_table.png "Figure 6 - Top Rows of customers_table")

***Figure 6 - Top Rows of customers_table***





```sql
SELECT COUNT(*) FROM vine_table;
SELECT * FROM vine_table FETCH FIRST 10 ROW ONLY;
```



![7_vine_table_count](Resources/7_vine_table_count.png "Figure 7 - Count of vine_table")

***Figure 7 - Count of vine_table***









![6_Top_Rows_of_vine_table](Resources/8_Top_Rows_of_vine_table.png "Figure 8 - Top Rows of vine_table")

***Figure 8 - Top Rows of vine_table***



### Determine Bias of Vine Reviews

```python
# Create the vine_table. DataFrame
vine_df = df.select(["review_id", "star_rating", "helpful_votes", "total_votes", "vine", "verified_purchase"])
vine_df.show()
```



![9_DataFrame_of_Vine_Table_Data](Resources/9_DataFrame_of_Vine_Table_Data.png "Figure 9 - DataFrame of Vine Table Data")

***Figure 9 - DataFrame of Vine Table Data***



```python
# The data is filtered to create a DataFrame where there are 20 or more total votes
vine_df_filtered = vine_df.filter(vine_df.total_votes >= "20")
vine_df_filtered.show()
```



![10_Filtered_Dataframe_with_20_or_More_Votes](Resources/10_Filtered_Dataframe_with_20_or_More_Votes.png "Figure 10 - Filtered Dataframe with 20 or More Votes.")

***Figure 10 - Filtered Dataframe with 20 or More Votes***



```python
# The data is filtered to create a DataFrame where the percentage of helpful_votes is equal to or greater than 50%
help_total_50_plus = vine_df_filtered.filter(vine_df_filtered.helpful_votes/vine_df_filtered.total_votes >= .5)
help_total_50_plus.show()
```



![11_Helpful_Votes_Equal_or_Greater_than_50_Percent](Resources/11_Helpful_Votes_Equal_or_Greater_than_50_Percent.png "Figure 11 - Helpful Votes Equal or Greater than 50 Percent.png")

***Figure 11 - Helpful Votes Equal or Greater than 50 Percent***



```python
# The data is filtered to create a DataFrame or table where there is a Vine review
helpful_paid = help_total_50_plus.filter(help_total_50_plus.vine == 'Y')
helpful_paid.show()
```

![12_DataFrame_with_Vine_Review](Resources/12_DataFrame_with_Vine_Review.png "Figure 12 - DataFrame with Vine Review")

***Figure 12 - DataFrame with Vine Review***



```python
# The data is filtered to create a DataFrame where there isn’t a Vine review
helpful_unpaid = help_total_50_plus.filter(help_total_50_plus.vine == 'N')
helpful_unpaid.show()
```



![13_DataFrame_without_Vine_Review](Resources/13_DataFrame_without_Vine_Review.png "Figure 13 - DataFrame without Vine Review")

***Figure 13 - DataFrame without Vine Review***



As can be seen in figure 14 below a very small of all 5 star review  are paid.

```python
# The total number of reviews, the number of 5-star reviews, and the percentage 5-star reviews are calculated
# for all Vine and non-Vine reviews 

total_reviews = help_total_50_plus.count()
print("Total Reviews = ", total_reviews)

total_paid_5 = helpful_paid.filter(helpful_paid.star_rating == '5').count()
percent_paid_5 = total_paid_5/total_reviews*100
print("Total Paid 5 Star Reviews = ", total_paid_5, "(",percent_paid_5,"%)")

total_unpaid_5 = helpful_unpaid.filter(helpful_unpaid.star_rating == '5').count()
percent_unpaid_5 = total_unpaid_5/total_reviews*100
print("Total Unpaid 5 Star Reviews = ", total_unpaid_5, "(",percent_unpaid_5,"%)")

```



![14_5_Star_Review_Results](Resources/14_5_Star_Review_Results.png "Figure 14 - Final Results of the 5 Star Review Analysis")

***Figure 14 - Final Results of the 5 Star Review Analysis***

In the code fence below we have calculated the percentage of paid and unpaid reviews that resulted in a 5 star outcome.

```python
# 5. Determine the total number of reviews, the number of 5-star reviews, and the
#    percentage of 5-star reviews for the two types of review (paid vs unpaid).

total_reviews = help_total_50_plus.count()
print("Total Reviews = ", total_reviews)
total_paid_reviews = helpful_paid.count()
print("Total Paid Reviews = ", total_paid_reviews)

total_paid_5 = helpful_paid.filter(helpful_paid.star_rating == '5').count()
percent_paid_5 = total_paid_5/total_paid_reviews*100
print("Total Paid 5 Star Reviews = ", total_paid_5, "(",percent_paid_5,"%)")

total_unpaid_reviews = helpful_unpaid.count()
print("Total Unpaid Reviews = ", total_unpaid_reviews)

total_unpaid_5 = helpful_unpaid.filter(helpful_unpaid.star_rating == '5').count()
percent_unpaid_5 = total_unpaid_5/total_unpaid_reviews*100
print("Total Unpaid 5 Star Reviews = ", total_unpaid_5, "(",percent_unpaid_5,"%)")
```

At 45.6% for paid and 49.3% for unpaid it would appear that the paid reviews are similar to unpaid and perhaps slightly less favorable.

![15_5_Star_Review_Results_by_Group](Resources/15_5_Star_Review_Results_by_Group.png "Figure 15 - 5 Star Review Results by Group")

***Figure 15 - 5 Star Review Results by Group***

In an effort to validate the finds found in the five star reviews we conducted further and more extensive analysis.  I the code fence below we created a dataframe to look at the distribution of paid ratings.

```python
# Distribution of Paid Rating 
star_groups_paid = helpful_paid.groupBy(helpful_paid.star_rating).count()
star_groups_paid = star_groups_paid.sort(star_groups_paid.star_rating)
star_groups_paid.show()
```

In figure 16 below you can see that the paid ratings are heavily weight to the high end of the star scale.

![16_Distribution_of_Paid_Reviews](Resources\16_Distribution_of_Paid_Reviews.png "Figure 16 - Distribution of Paid Reviews.png")

***Figure 16 - Distribution of Paid Reviews***

This dataframe was then converted to a Pandas dataframe using the code fence below.

```python
import pandas as pd
paid_pandasDF = star_groups_paid.toPandas()
paid_pandasDF
```

In figure 17 is displayed the Pandas dataframe that we conducted further analysis on.

![17_Pandas_Distribution_of_Paid_Reviews](Resources/17_Pandas_Distribution_of_Paid_Reviews.png "Figure 17 - Pandas Distribution of Paid Reviews")

***Figure 17 - Pandas Distribution of Paid Reviews***

The following code fence was used to calculate the weighted average of paid reviews.

```python
import numpy as np
weighted_average_paid = np.average(a =paid_pandasDF['star_rating'] , weights = paid_pandasDF['count'])
weighted_average_paid
```

The resulting average for paid reviews was calculated to be 4.170984455958549.



We conducted the same analysis for upnpaid reviews.   Using the code fence below we created a dataframe to look at the distribution of unpaid ratings.

```python
# Distribution of Unpaid Rating 
star_groups_unpaid = helpful_unpaid.groupBy(helpful_unpaid.star_rating).count()
star_groups_unpaid = star_groups_unpaid.sort(star_groups_unpaid.star_rating)
star_groups_unpaid.show()
```

In figure 18 below you can see that the unpaid ratings distributed quite differently than those of the paid.  The distribution is almost binary, with at large number of stars rating at both ends of the scale.

![18_Distribution_of_Unpaid_Reviews](Resources\18_Distribution_of_Unpaid_Reviews.png "Figure 18 - Distribution of Unpaid Reviews.png")

***Figure 18 - Distribution of Unpaid Reviews***

This dataframe was then converted to a Pandas dataframe using the code fence below.

```python
import pandas as pd
unpaid_pandasDF = star_groups_unpaid.toPandas()
unpaid_pandasDF
```

In figure 19 is displayed the Pandas dataframe that we conducted further analysis on.

![19_Pandas_Distribution_of_Unpaid_Reviews](Resources/19_Pandas_Distribution_of_Unpaid_Reviews.png "Figure 19 - Pandas Distribution of Unpaid Reviews")

***Figure 19 - Pandas Distribution of Unpaid Reviews***

The following code fence was used to calculate the weighted average of unpaid reviews.

```python
import numpy as np
weighted_average_unpaid = np.average(a =unpaid_pandasDF['star_rating'] , weights = unpaid_pandasDF['count'])
weighted_average_unpaid
```

The resulting average for unpaid reviews was calculated to be 3.6559722478806167.

Conducting this analysis it is clear that the paid reviews are in fact bias.  Using the code fence below we calculated that bias.

```python
# Calculate the Paid Ratings Bias
paid_bias = (weighted_average_paid/weighted_average_unpaid - 1)*100
paid_bias
```

It was determined the the paid reviews were 14.086874110614133 % biased towards providing favorable reviews.

## Summary

