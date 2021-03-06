# CMPE-272-Enterprise Software Technologies
Assignment on Apache Spark

This assignment is to detect fraud in user reviews. Below are the approaches adopted to perform fraud analysis:

Fraudulent reviews often carry telltale signs, which are picked up by software and flagged for review by
moderators. Some of the signs are illustrated in the below given examples:

1. One reviewer's opinions consistently run counter to the majority.
2. Multiple reviews share many of the same phrases and typos.
3. Reviewer id, which uniquely identifies reviewer is the same on multiple reviews for the same business.

Other Indicators:
1. The writer is reviewing multiple products from the same company
2. One group of users is reviewing the same product.
3. Many reviews share identical timestamps.

Below are the SQL queries to implement each of the above  fraud detection method in User reviews:

a. One reviewer's opinions consistently run counter to the majority.
select reviewerID, reviewerName, reviewText, overall, reviewTime from User_Reviews where overall <=1.5
Output: Below data shows that reviewer's with reviewer id 1 and 2 consistently gives negative feedback about the product.

----------+------------+--------------------+-------+-----------+
|reviewerID|reviewerName|          reviewText|overall| reviewTime|
+----------+------------+--------------------+-------+-----------+
|         1| J. McDonald|Worst!! Will neve...|    1.0|09 13, 2015|
|         1| J. McDonald|Not use it again ...|    0.5|10 13, 2015|
|         1| J. McDonald|             Worst!!|    1.0|11 13, 2015|
|         1| J. McDonald|Worst product eve...|    0.5|12 13, 2015|
|         2|   J. Bakshi|          Really Bad|    1.5|09 13, 2015|
|         2|   J. Bakshi|                 Bad|    1.5|09 14, 2015|
|         2|   J. Bakshi|          Really Bad|    1.5|09 13, 2015|
|         2|   J. Bakshi|                 Bad|    1.5|09 14, 2015|
|         2|   J. Bakshi|          Really Bad|    1.5|09 14, 2015|
|         2|   J. Bakshi|                 Bad|    1.5|09 14, 2015|
+----------+------------+--------------------+-------+-----------

b. Multiple reviews share many of the same phrases and typos.
select reviewText from User_Reviews where reviewerID in (select reviewerID from User_Reviews where overall <=1.5)

Output: Shows the same negative keywords used by the reviewers in their reviews

c. Reviewer id, which uniquely identifies reviewer is the same on multiple reviews for the same business.
select reviewerID from User_Reviews where overall <=1.5

Output: Below data shows the same  reviewer id's for negative reviews.
ReviewerID|
+----------+
|         1|
|         1|
|         1|
|         1|
|         2|
|         2|
|         2|
|         2|
|         2|
|         2|
+--------- +

d. The writer is reviewing multiple products from the same company
select reviewerID, asin as productid, overall from  User_Reviews where overall <=1.5 group by reviewerID, asin , overall

Output: Below data shows that same reviewer is giving negative feedback for different products of the company
reviewerID| productid|overall|
+----------+----------+-------+
|         1|0000013714|    1.0|
|         1|0000013714|    0.5|
|         2|0000013714|    1.5|
|         2|0000013715|    1.5|
|         2|0000013716|    1.5|
+----------+----------+-------+

e. Total Persons vs Review Text
select count(*) as totalpersons , reviewText from User_Reviews group by reviewText

Output: Below data shows that  reviewers with id 1 and 2  only gives negative reviews

totalpersons|          reviewText|
+------------+--------------------+
|           2|           excellent|
|           1|Not use it again ...|
|           3|                 Bad|
|           1|Worst!! Will neve...|
|           3|          Really Bad|
|           1|Worst product eve...|
|           2|                null|
|           9|                good|
|           1|             Worst!!|
|           5|           very good|
+------------+--------------------+
