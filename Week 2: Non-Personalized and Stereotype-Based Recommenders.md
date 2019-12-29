# Week 2: Non-Personalized and Stereotype-Based Recommenders
### Targets
- New users
- Online communities around common displays(e.g. Reddit, Slashdot)
- Applications & media where personalization is impossible
- Examples: Zaggat survey, billboards Top 200, ‘Popular Now’ on any news site

## Summary stats
- Score means Popularity, Average Rating and Probability of You Liking
- Score is computed based on frequency, average and more
  - Examples: Zaggat survey: Food 29, Decor 26, Service 28; Self-selection bias and increased diversity of raters lead to ratings not matching to expectations
- Count, average, and distribution together
  - Examples: Amazon product customer reviews 
- Averages Can be Misleading; 
  - Can adjust by summing % who like and adjust by normalizing user ratings
  - normalization addresses different rating scales
  – May want to consider credibility of individual raters (history of ratings)
### Displaying Aggregate Preferences (predict)
  - Approaches: Average rating / upvote proportion; Net upvotes / # of likes; % >= 4 stars (‘positive’); Full distribution
  - Goals: To help users decide to buy/read/view the item
### Ranking Items (recommend)
Can't rank by score: Too little data (one 5-star rating); Score may be multivariate (histogram); Domain or business considerations; Item is old; Item is ‘unfavored’
#### Considerations
- Confidence: How confident are we that this item is good?
- Risk tolerance: High-risk, high-reward; Conservative recommendation
- Domain and business considerations: Age and System goals

#### Damped means
- Items with few rating leads to low confidence
- assume that, without evidence, everything is average
- Ratings are evidence of non-averageness
- k controls strength of evidence required

![equation](http://latex.codecogs.com/gif.latex?%5Cfrac%7B%5Csum_%7Bu%7D%5E%7B%20%7D%20r_%7Bui%7D%20&plus;%20k%20%5Cmu%7D%7Bn&plus;k%7D)

#### Confidence Intervals 
- lower bound of statistical confidence interval (95%)
- Choice of bound affects risk/confidence
- Lower bound is conservative: be sure it's good; Upper bound is risky: there's a chance of amazing
- Example: Reddit uses Wilson interval (for binomial) to rank comments

#### Domain Consideration: Time
![equation](http://latex.codecogs.com/png.latex?%5Cfrac%7B%28Up-Down-1%29%5E%7B%5Calpha%7D%7D%7B%28t_%7Bnow%7D-t_%7Bpost%7D%29%5E%5Cgamma%20%7D%5Ctimes%20Penalty)
- Scoring news stories
- Net upvotes, polynomially decayed by age
- Old items scored mostly by vote
- Multiplied by item penalty terms - incorporate community goals into score
- Examples: 
  - Reddit: old stories aren't interesting even if they have many upvotes!
  - eBay: items have short lifetimes

#### Reddit example: Customize scoring to meet business needs 
![equation](http://latex.codecogs.com/png.latex?%5Clog_%7B10%7Dmax%281%2C%20%5Cleft%20%7C%20Upper-Down%20%5Cright%20%7C%29&plus;%5Cfrac%7Bsign%28Upper-Down%29t_%7Bpost%7D%7D%7B45000%7D)
- Log term applied to votes: decrease marginal value of later votes
- Time is seconds since Reddit epoch
- Buries items with negative votes
- Time vs. vote impact independent of age
- Scores news items, not comments

## Demographics and Related Approaches
Step 0: Identify available demographics (age, zipcode) and explore correlation(scatterplots, correlations) <br>
Step 1: Once found relevant demographic, break down summary stats into groups. What's the most popular item for women in age 30-39? <br>
Step 2: Regression model to predict items based on demographics stats. e.g. Linear regression for rating, logistic regression for purchase. <br>
Step 3: Make defaults for unknown demographics<br>
– May simply be overall preferences
– May reflect expected demographics of newcomers
– May be modeled separately

Note: If demographics are useful, getting data on users is key <br>
– Various sources of data, from advertising networks to loyalty club sign‐ups and surveys
– In some cases, demographics can be “predicted” from data.
- Can be used for customize products or contents for demographics; Or some products naturally appeal to different groups. 

## Product association recommenders
- Ephemeral, contextual personalization based on current products, path, links, etc. e.g. People Who Like/Bought/.... <br>
- Can compute product associations from prior transaction history: Balance between high-probability and increased probability
- Such recommendations can be targeted for
additional or replacement purchases

#### Solution 1 Bayer's Law: how much more likely Y is than it was before
![equation](http://latex.codecogs.com/png.latex?%5Cfrac%7BP%28Y%7CX%29%29%7D%7BP%28Y%29%7D)

#### Solution 2 Association rule mining, non-directional 
![Equation](http://latex.codecogs.com/png.latex?%5Cfrac%7BP%28Y%7CX%29%29%7D%7BP%28Y%29%7D)
- Failed at beer and diaper story
- Can be applied to product and link association

