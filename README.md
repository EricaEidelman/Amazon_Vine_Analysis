# Big Data

## Project Overview
The purpose of this project is to look at product reviews published to Amazon in order to judge whether there is any bias from members using the paid Vine program. This project looks at reviews specifically for items in the shoes category. Data was loaded into pgAdmin and then analyzed as a dataframe on Jupyter Notebook.

## Resources
Data Source: https://s3.amazonaws.com/amazon-reviews-pds/tsv/amazon_reviews_us_Shoes_v1_00.tsv.gz

Software: Python 3.7.6, pgAdmin 4, PostgresSQL 11.4

## Results
After loading in the dataframe, the data was first filtered to show only reviews with more than 20 votes, and then those with more than half of helpful votes. This was achieved with the code snippets shown below:
```
# Filter to show reviews with more than 20 votes
votes_more_20 = vine_df[(vine_df["total_votes"] > 20)]
votes_more_20.head()

#Filter to show reviews where more than half of votes were helpful
helpful_more_half = votes_more_20[(votes_more_20["helpful_votes"]/votes_more_20["total_votes"] > 0.5)]
helpful_more_half.head()
```

The header of the reslting dataframes can be seen in the image below (header looked the same for both filterings).4

![This is an image](https://github.com/EricaEidelman/Amazon_Vine_Analysis/blob/main/total_head.png)

The next step was to filter for reviews submitted through Vine and not through Vine, with the code snippets illustrated below.
```
# Filter for paid reviews submitted through vine
vine_yes = helpful_more_half[(helpful_more_half["vine"] == "Y")]
vine_yes.head()

# Filter for unpaid reviews not submitted through vine
vine_no = helpful_more_half[(helpful_more_half["vine"] == "N")]
vine_no.head()
```

The headers of the resulting dataframes can be seen below.

![This is an image](https://github.com/EricaEidelman/Amazon_Vine_Analysis/blob/main/vine_yes.png)

![This is an image](https://github.com/EricaEidelman/Amazon_Vine_Analysis/blob/main/vine_no.png)

The final step was to calculate the percentage of 5 star reviews for both the Vine and non-Vine programs. This was achieved as per the below.
```
# Find total number of reviews submitted through vine and not through vine
vine_yes_total = vine_yes["review_id"].count()
vine_no_total = vine_no["review_id"].count()

# Find total number of 5 star reviews submitted through vine and not through vine
vine_yes_5 = vine_yes[(vine_yes["star_rating"] == 5)].count()["review_id"]
vine_no_5 = vine_no[(vine_no["star_rating"] == 5)].count()["review_id"]

# Find percentage of 5 star reviews submitted through vine and not through vine
vine_yes_5_percent = vine_yes_5/vine_yes_total
vine_no_5_percent = vine_no_5/vine_no_total
```

The results of the analysis are summarized below:
- There were a total of 20 reviews in the narrowed dataset submitted through the Vine program and 25,104 not sumbitted through Vine.
- 12 of the Vine reviews gave 5 stars and 13,479 of the non-Vine reviews gave 5 stars.
- 60% of the Vine reviews gave 5 stars and 53.69% of the non-Vine reviews gave 5 stars.

## Summary
At first glance it would appear that there is a positivity bias from the Vine reviews as the percentage of 5 star reviews is higher from Vine program users than from non-users. However, the number of non-Vine reviews was about 1,255 times the number of Vine reviews, while the number of 5 star non-Vine reviews was about 1,123 times the number of 5 star Vine reviews. The similarity in magnitude between total reviews and 5 star reviews indicates that the bias might not be that great and may be attributable to the smaller sized sample of Vine reviews. A further analysis step might be to look at a breakdown of Vine and non-Vine reviews for all the different star numbers. If the other numbers of stars show similar differences in magnitude when looking at the total number of reviews and the reviews with that particular number of stars, then there can be said to not really be a positivity bias for Vine reviews. Alternatively, there may be a very large percentage of 1 and 2 star reviews for non-Vine users and none at all for Vine users, which would support a positivity bias for Vine.
