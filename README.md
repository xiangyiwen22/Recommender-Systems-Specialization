# Week 1: Introducing Recommender Systems

## Preferences and Ratings<br>
- Explicit: Just ask the users what they think! e.g. rating, review, vote;
- Implicit: user action is for some other purpose, not expressing preference. e.g. Click, Purchase, Follow;

## Predictions and Recommendations <br>
**Predictions** estimates of how much you’ll like an item<br>
- Often scaled to match some rating scale;<br>
- Often tied to search or browsing for specific products;<br>
- Pro: helps quantify item;<br>
- Con: provides something falsifiable;<br>

**Recommendations** are suggestions for items you might like (or might fit what you’re doing)<br>
- Often presented in the form of “top-n lists”;<br>
- Also sometimes just placed in front of you;<br>
- Pro: provides good choices as a default;<br>
- Con: if perceived as top-n, can result in failure to explore (if top few seem poor);

## Taxinomy of Recommenders <br>
### Major Recommendation Algorithms
1. Non-personalized summary stats
- External community data: best seller, most popular, trending hot;
- Summary of community ratings: best liked
- Examples: Zagat restaurant ratings; Billboard music rankings; TripAdvisor hotel ratings;

2. Content-Based Filtering: Information Filtering, Knowledge-Based
- User Ratings x Item Attributes => Model (applied to new items via attributes)
- Examples: Personalized news feeds and Artist or Genre music feeds
- Alternative: knowledge-based, Item attributes form model of item space - Users navigate/browse that space

3. Personallized Collaborative Filtering <br>
- Use opinions of others to predict/recommend
- Sparse matrix: User model w/ ratings vs Item model w/ratings
  - Fill in missing values (predict)
  - Select promising cells (recommend)
- techniques: 
  - User-user: select neighborhood of similar-taste people and use their opinions
  - Item-item: pre-compute similarity among items via ratings, use own ratings to triangulate for recommendations
  - Dimensionality reduction: compress and use taste reprensentation:  yields a lower-d matrix
### Model elements and evaluation metrics 
- Basic model elements: Users, Items, Ratings, and Community (optional) <br>
User (demographics) -------> Ratings <------- Items (properties, genres, etc.)
- Choose evaluation metrics
  - Accuracy of prediction
  - Usefulness of recommendations: correct, non-onvious and diverse
  - Computational expense


### Examples Movielens, Amazon 

# Week 2: Non-Personalized and Stereotype-Based Recommenders
### Targets
- New users
- Online communities around common displays(e.g. Reddit, Slashdot)
- Applications & media where personalization is impossible
- Examples: Zaggat survey, billboards Top 200, ‘Popular Now’ on any news site

### Summary stats
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

![equation](http://www.sciweavers.org/upload/Tex2Img_1577216501/render.png)

# Week 3: Content-Based Filtering -- Part I

# Week 4: Content-Based Filtering -- Part II
