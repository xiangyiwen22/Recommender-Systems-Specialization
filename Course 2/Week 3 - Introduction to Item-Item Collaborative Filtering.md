# Item to Item Collaborative Filtering
## Introduction
### Motivation 
- User‐User CF sparsity: When we have millions of items but only thounds of users with small number of ratings, too often there are points where no recommendation can be made. Solutions: “filterbots”, item‐item, and dimensionality reduction.
- Computational performance: With millions of users (or more), computing allpairs correlations is expensive; Even incremental approaches were expensive; And user profiles could change quickly since users expect the recommendation to be changed frequently –> needed to compute in real time to keep users happy

### Item-Item Insights
Item-Item similarity is fairly stable - this is dependent on **having many more users than items**
- Average item has many more ratings than an anverage user - Doesn't work well in an application where there is a relatively small number of customers, and many more products. E.g., if you have a fixed customer base of 50,000 people but millions of products you are selling to them. 
- Intuitively, items don’t generally change rapidly – at least not in ratings space (special case for time‐bound
items e.g. Christmas trees and calendars)

Item similarity is a route to computing a prediction of a user’s item preference
  

### Benefits of Item‐Item
- Good prediction accuracy; Good performance on top‐N predictions
- Efficient implementation: At least in cases where |U| >> |I|; Benefits of precomputability
- Broad applicability and flexibility: As easy to apply to a shopping cart as to a user profile

### Item‐Item Top‐N
- Simplify model by limiting items to small “neighborhoods” of k most‐similar items (e.g., 20)
- For a profile set of items, compute/merge/sort the k‐most similar items for each profile item
[Straightforward matrix operation from Deshpande and Karypis](http://glaros.dtc.umn.edu/gkhome/fetch/papers/itemrsTOIS04.pdf)

### Core Assumptions/Limitations
Item‐item relationships need to be stable …
- Mostly a corollary of stable user preferences
- Could have special cases that are difficult (e.g., calendars, short‐lived books, etc.)
- Many of these issues are general temporal issues

Main limitation/complaint: lower serendipity
- This is a user/researcher complaint, not fully studied; intuition is clear
