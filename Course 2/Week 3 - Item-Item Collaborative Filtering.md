# Item to Item Collaborative Filtering

<center> <h1>Introduction </h1> </center>

### Motivation 
- User‐User CF sparsity: When we have millions of items but only thounds of users with small number of ratings, too often there are points where no recommendation can be made. Solutions: “filterbots”, item‐item, and dimensionality reduction.
- Computational performance: With millions of users (or more), computing allpairs correlations is expensive; Even incremental approaches were expensive; And user profiles could change quickly since users expect the recommendation to be changed frequently –> needed to compute in real time to keep users happy

### Item-Item Assumptions
- Item-Item similarity is fairly stable - this is dependent on **having many more users than items**. Average item has many more ratings than an anverage user - Doesn't work well in an application where there is a relatively small number of customers, and many more products. E.g., if you have a fixed customer base of 50,000 people but millions of products you are selling to them. 
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

<center> <h1>Item-Item Algorithm </h1> </center>

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
- Can drop non-positive neighbors
- Balance memory use with accuracy and coverage: Mild runtime improvements as well

### Model Building Algorithm
```python
for i in I:
  for j in I s.t. i!=j:
    if sim(i,j)>0:
      wij=sim(i,j)
# clear all but 𝑀 largest values of row wi vector
```
<center> <h1>Item-Item on Unary Data - Implicit Feedback </h1> </center>
Item-Item not only works on rating data (explicit), but also works on unary data (implicit) such as clicks, plays, and purchases
### Represented 
  - as logical (1/0) user-item `purchase` matrix 
  - purchase count matrix (how many times listened to this song)
  - ignore 0: 0 may not represent user doesn't like this song, but we just ignore it due to the sparsity of matrix
  
### Normalization
  - Standard mean‐centering not meaningful. But we can normalize user vectors to unit vectors - users who like many items provide less information about any particular pair
  - Could also consider logging counts
  
### Computing similarities 
  - Consine similarity still works 
  - Can also use conditional probability, similar to association rules; not symmatrical ![notsymmatrical](http://latex.codecogs.com/gif.latex?P%28r_%7Bi%7D%7Cr_%7Bj%7D%29%5Cneq%20P%28r_%7Bj%7D%7Cr_%7Bi%7D%29) (Deshpande and Karypis paper)
  
### Aggregating Scores
- non‐binary (counts): Weighted average works
- binary (0/1): sum neighbor similarities; Weighted average doesn't work
  - fixed neighborhood size means this isn't unbounded <br>
  ![unbounded](http://latex.codecogs.com/gif.latex?s%28i%2C%20u%29%20%3D%20%5Csum_%7Bj%5Cin%20N%7D%20w_%7Bij%7D)
  - Truncate `N` to top `k` item. Neighborhood selection unchanged, we still pick the most similar item
  
<center> <h1> Extensions </h1> </center>
Item‐item is good for extending directly. Simple parts with well‐defined interfaces provide a lot of flexibility. It's easy to understand what extensions do. Many interesting recommenders can be built by reconfiguring it

### Example: User Trust
- Goal: incorporate user trustworthiness into item relatedness computation
  - User's global reputation, not per‐user trust
- Solution: weight users by trust before computing item similarities; use trust as coefficient to the normed ratings 
- High‐trust users have more impact
- Massa and Avesani. 2004. ‘Trust‐Aware Collaborative Filtering for Recommender Systems’

### Extension: Papers and PageRank
- Goal: incorporate paper 'importance' into recommender. Recommending research papers: useful to consider items as users who purchase the paper's citations. Same idea can apply to web pages
- Solution: weight paper user vectors by the paper's PageRank (or HITS hub score)
- Ekstrand et al., 2010. Automatically Building Research Reading Lists.

### Restructuring: Item‐Item CBF
- Basic item‐item algorithm structure doesn't care how similarity is computed. So why not use content‐based similarity?
- Resulting algorithm really isn't a collaborative filter. But it can work pretty well!
- Example: using Lucene (search engine) to compare documents as neighborhood & similarity function

### Restructuring: Deriving Weights
- Item‐item compares individual item pairs
- Alternative approach: infer coefficients from data
  - Find coefficients ![w](http://latex.codecogs.com/gif.latex?w_%7Bij%7D) that minimize squared error
  - Learn coefficients with standard machine learning/optimization algorithm (gradient descent)


<center> <h1> Item‐Item Strength and Weakness </h1> </center>

Item‐Item is more **conservative** than User-User in its recommendations and predictions; Item‐Item is **faster and more stable** (allowing pre-computation) for domains with many more users than items

- Very difficult for item‐item to discover highly different items to recommend, since they were grounded in more data
- User‐User by default will elevate items that a close neighbor loves, even without much evidence: If `Bob` think movie `Pets` is good, this is enough evidence for his close neighbor that `Pets` is good
- Can be good for shopping, consumption tasks but frustrating for browsing/entertainment
  - Very Successful in Commercial Applications: Amazon used Item‐Item widely and claims great success in recommendation; Helps people find products of interest
  - Big Disappointment in MovieLens: MovieLens users were switched, and many complained that recommendations were too obvious; Lack of bold recommendations and predictions

