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
- Efficient implementation: At least in cases where |U| >> |I|; Benefits of precomputability; Pre-compute item similarities over all pairs of items
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

## Item-Item Algorithm
Summary: Compare `user-user` with `item-item`
- `user-user`: Average over **users**
![useruser](http://latex.codecogs.com/gif.latex?S_%7Bv%2C%20i%7D%20%3D%20%5Cbar%7Br_%7Bv%7D%7D&plus;%20%5Cfrac%7B%5Csum_%7Bv%5Cepsilon%20u%7D%28r_%7Bvi%7D-%5Cbar%7Br_%7Bv%7D%7D%29%5Ccdot%20w_%7Buv%7D%7D%7B%5Csum_%7Bv%5Cepsilon%20u%7Dw_%7Buv%7D%7D)

  - u: the target user
  - v: the other user
- `item-item`: Average over **item**
![itemitem](http://latex.codecogs.com/gif.latex?S%5Cleft%20%28%20u%2C%20i%20%5Cright%20%29%20%3D%20%5Cfrac%7B%5Csum_%7Bj%5Cin%20N%5Cleft%20%28%20i%3B%20u%20%5Cright%20%29%7Dw_%7Bij%7D%5Cleft%20%28%20r_%7Buj%7D%20-%20%5Cbar%7Br_%7Bj%7D%7D%20%5Cright%20%29%7D%7B%5Csum_%7Bj%5Cin%20N%5Cleft%20%28%20i%3B%20u%20%5Cright%20%29%7D%5Cleft%20%7Cw_%7Bij%7D%20%5Cright%20%7C%7D%20&plus;%20%5Cbar%7Br_%7Bi%7D%7D)
  - i: the target item
  - j: the other item


### Part 1 - How to compute item similarities ![wij](http://latex.codecogs.com/gif.latex?w_%7Bij%7D) ? 
Also called interpolation weight (or correlation); stable, usually pre-computed using **cosine similarity**. 

![similarity_matrix](http://latex.codecogs.com/gif.latex?w_%7Bij%7D%20%3D%20sim%28i%2Cj%29%20%3D%20%5Ccos%28%5Chat%7B%5Cvec%7Br_%7Bi%7D%7D%7D%2C%20%5Chat%7B%5Cvec%7Br_%7Bj%7D%7D%7D%29%20%3D%20%5Cfrac%7B%5Chat%7B%5Cvec%7Br_%7Bi%7D%7D%7D%2C%20%5Chat%7B%5Cvec%7Br_%7Bj%7D%7D%7D%7D%7B%5Cleft%20%5C%7C%20%5Chat%7B%5Cvec%7Br_%7Bi%7D%7D%7D%20%5Cright%20%5C%7C_%7B2%7D%20%5Cleft%20%5C%7C%20%5Chat%7B%5Cvec%7Br_%7Bj%7D%7D%7D%20%5Cright%20%5C%7C_%7B2%7D%7D%20%3D%5Cfrac%7B%5Csum_%7Bu%7D%5Chat%7Br_%7Bui%7D%7D%5Chat%7Br_%7Buj%7D%7D%7D%7B%5Csqrt%7B%5Csum_%7Bu%7Dr_%7Bui%7D%5E%7B2%7D%7D%20%5Csqrt%7B%5Csum_%7Bu%7Dr_%7Buj%7D%5E%7B2%7D%7D%7D)

**Item mean-centered**: Normalize by subtracting item means (instead of user means in `user-user`). When we treat missing values as 0, it introduces a useful self-damping effect when items have few users in common but many ratings. It is equivalent to the Pearson correlation, so it is statistically meaningful.

![similarity_matrix_mean-centered](http://latex.codecogs.com/gif.latex?w_%7Bij%7D%3D%5Cfrac%7B%5Csum_%7Bu%7D%28%5Chat%7Br_%7Bui%7D%7D-%5Cbar%7Br%7D_%7Bi%7D%29%28%5Chat%7Br_%7Buj%7D%7D-%5Cbar%7Br%7D_%7Bj%7D%29%7D%7B%5Csqrt%7B%5Csum_%7Bu%7D%5Cleft%20%28r_%7Bui%7D%20-%5Cbar%7Br%7D_%7Bi%7D%20%5Cright%20%29%5E%7B2%7D%7D%20%5Csqrt%7B%5Csum_%7Bu%7D%5Cleft%20%28r_%7Buj%7D%20-%5Cbar%7Br%7D_%7Bj%7D%5Cright%20%29%5E%7B2%7D%7D%7D)

- Naively ![E](http://latex.codecogs.com/gif.latex?O%28%5Cleft%20%7C%20I%20%5Cright%20%7C%5E%7B2%7D%29). But in reality it's symmetric ![e](http://latex.codecogs.com/gif.latex?w_%7Bij%7D%20%3D%20w_%7Bji%7D) and we can skip pair when user didn't rate this item

### Part 2 - How to pick neighbors?
![n](http://latex.codecogs.com/gif.latex?N%5Cleft%20%28%20i%3B%20u%20%5Cright%20%29) top `k` most similar items to `i` that user `u` has rated. `k` too small -> inaccurate score; `k` too large -> too much noise; usually pick `k` = 20

### Part 3 - Score Item: do we need to normalize the rating ![E](http://latex.codecogs.com/gif.latex?r_%7Buj%7D)? 
normalize item j rating by subtracting the mean rating of item j ![rj](http://latex.codecogs.com/gif.latex?%5Cbar%7Br_%7Bj%7D%7D), denormalize by adding back ![ri](http://latex.codecogs.com/gif.latex?%5Cbar%7Br_%7Bi%7D%7D) 

![norm_score](http://latex.codecogs.com/gif.latex?S%5Cleft%20%28%20u%2C%20i%20%5Cright%20%29%20%3D%20%5Cfrac%7B%5Csum_%7Bj%5Cin%20N%5Cleft%20%28%20i%3B%20u%20%5Cright%20%29%7Dw_%7Bij%7D%5Cleft%20%28%20r_%7Buj%7D%20-%20%5Cbar%7Br_%7Bj%7D%7D%20%5Cright%20%29%7D%7B%5Csum_%7Bj%5Cin%20N%5Cleft%20%28%20i%3B%20u%20%5Cright%20%29%7D%5Cleft%20%7Cw_%7Bij%7D%20%5Cright%20%7C%7D%20&plus;%20%5Cbar%7Br_%7Bi%7D%7D)

### Truncating the model
- Don't need to keep the whole ![E](http://latex.codecogs.com/gif.latex?O%28%5Cleft%20%7C%20I%20%5Cright%20%7C%5E%7B2%7D%29) model
- Need enough neighbors to find neighbors at score time. Since user hasn’t rated everything, need 𝑀≫𝑘 neighbors per item in model
- Can drop nonpositiveneighbors
- Balance memory use with accuracy and coverage: Mild runtime improvements as well

### Model Building Algorithm
```python
for i in I:
  for j in I s.t. i!=j:
    if sim(i,j)>0:
      wij=sim(i,j)
# clear all but 𝑀 largest values of row wi vector
```
