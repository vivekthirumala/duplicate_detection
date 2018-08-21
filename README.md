# Duplicate Detection
Identifying key variables that can help to identify the duplication in listing of products. Score based approach was used to compare the similarity between any two given products.

Objective: To identify the duplicates products which are appearing with different productId’s.
Data Subset Chosen: I have chosen to work on Tunics.
High level steps followed in arriving at the solution:
1) Filtered the dataset to Tunics and saved them in separate file.
2) Identified the relation between different variables in the data.
3) Based on data understanding and objective, unnecessary columns are dropped, and only useful columns are retained.
4) Products belonging to same family as well as category are taken in a group and similarity score was calculated for all possible combinations within the group. The process is repeated to all such groups.

Understandings:
1) From manual exploration of data, it is found that first 7 characters of two productIds are same means that the products belong to same family (most of the cases). Only first 6 characters are same in few of the cases.
2) For all productIds having first 8 characters common, they also have categories part in common. Therefore, productId gives information on categories for a given family of products.
3) All pairs of duplicate products had first 8 characters of productId common. However, it is also found that the vice versa is not true (i.e., products with same first 8 characters of productId need not necessarily be same/duplicate).
4) Above information can be used in checking the similarity of a product with other product in which both the products have first 8 characters of productId in common. This will reduce the total number of iterations/comparisons.

Assumptions:
1) For two similar/duplicate products, mrp, title and detailedSpecStr will be same.
2) Duplicates need to be detected whether or not a product belongs to same seller. Therefore, seller information does not greatly signify any importance in detecting the duplicates.
3) For the purpose of product listing, size does not matter whereas color matters.

Grouping of Products:
1) The first 8 characters are same for the products falling under same family and category. This has been used to identify the first 8 characters for each productId. Any check for duplication will happen between productIds falling under same group only.
2) Total number of unique productIds = 43964
3) Total number of unique 8-character groups = 4788
4) Out of these 8-character groups, number of groups with more than one productId = 3609
5) Remaining groups i.e., 1179 have only one productId within the group and there is no chance of duplicates. Therefore, these can be ignored.

Similarity Score Calculation:
The four-metrics used for calculating the similarity score are:
1. image_score: This is a score calculated between the two given images based on their structural similarity. This method was proposed by Zhou Wang and Al Bovik.
(Score output value: 0.0 to 1.0)
2. mrp_score: This is a score calculated between the two mrp’s of productIds given.
(Score output value: 0 or 1)
3. title_score: This is a score calculated between the two titles of productIds given.
(Score output value: 0 or 1)
4. detailedSpecStr_score: This is a score calculated between the two detailed specifications of productIds given.
(Score output value: 0 or 1)
The above four metrics are used in calculating the total similarity score between the two given productIds by calculating the mean of these scores.
Score = mean(Image_score, Mrp_score, Title_score, detailedSpecStr_score)

Approach for comparison between products:
Grouping products using first 8 characters of productId. For each group, matching score between all possible combinations of products is obtained without any repetition of computation for matching between same productIds.

Iteratively for each group, ith product is compared with all the below jth products instead of n x n combinations.
As a result of comparison, the similarity score is obtained for all possible combinations. Combinations with score more than 0.5 only are retained and the rest are ignored.
Result of productId with respect to duplicate pair with similarity score is prepared in JSON format.
