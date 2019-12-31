# Week 3: TFIDF and Content Filtering
## Introduction to content based recommenders
A recommender = Model items according to relevant attributes + Model users' preference by attributes

#### Content-based filtering (CBF)
Build a vector of attribute or keyword preference 
- Users build their own profile: ask topics that they are interested in (e.g. Medium and Reddit)
- Infer profile from `user actions` (read, buy, click) and from `explicit user ratings` (map from item preference to attribute preference); Merge two inference. 

#### Case‐based reasoning systems and Knowledge‐based navigation systems
Structure a database of cases around a set of relevant attributes (e.g. camera price, zoom, pixels); Query based on an example or attribute query and retrieve relevant cases. Most helpful for ephemerally‐personalized experiences.
- *Shopping* - suggest similar relevant items. Compared with `collaborative` which suggest items that are co-purchased or co-browsed
- *Content* - suggest similar stories 



## TFIDF and content filtering
TFIDF = Term Freq * Inverse Document Freq <br>
- TF = # of occurrences of a term in the doc 
  - Occurrence count; 
  - 0/1 boolean freq occurring above threshold; 
  - `log(TF+1)`; 
  - Normalized TF divided by doc length 
- IDF = How few doc contain this term
  - `log(#Doc/ #DocWithTerm)`
- Automatic demotion of stopwords, common terms; Promotes core terms over incidental ones
- Fails when core term/concept isn't used (much) in doc (e.g. legal contracts)
- Application in CBF
  - create a profile of doc/object (e.g. a movie could be described as a weighted vector of its tags)
  - combine TFIDF profile with ratings to create user profiles, and then matched against future docs
  
#### Examples
- Clothing attributes (color, size, etc.)
- Terms used in hotel reviews (pool, front desk, friendly)
- Terms used in news articles (election, football, local)

#### From item to user profiles
- Vector space model combine `liking` with `importance`, which works well in some cases such as query terms, but not as much in others ((I like Hollandaise sauce a lot, but don’t actually care much if it is in a dish I’m ordering)
- How to **accumulate** profiles: add together the item vectors 
  - `Normalization` Yes if an item with a more populated vector is more descriptive of preference; No if all items should be the same weight
  - `Weigh the vectors` could use ratings, currency, confidence, etc. for weight
- How to **factor** in ratings
  - Simply unary – aggregate profiles of items we rated without weights
  - Simple binary – add positives and subtract negatives?
  - Unary with threshold – only put items above a certain rating into our profile (but all likes are equal) – we
  often think 3.5 in MovieLens
  - Weight, but positive only – higher weight for things with higher scores
  - Weight, and include negative also – negative weight for low ratings (normalize rating scale)
- How to **update** profiles (with new ratings)?
  - Don't, just recompute each time (expensive)
  - Weight new/old similarly – keep track of total weight in profile and mix in new rating (linear combination); (e.g. Special case for changed rating; subtract old)
  - Decay old profile and mix in new (e.g., may decide that profile should be dominated by most recent movies – formula might be 0.95 * old + 0.05 * new, in normalized form)
  
#### Compute predictions using `cosine` of the angle between the two vectors (profile, item)
- dot‐product of normalized vectors,
or if you prefer, the dot product divided by the product of the two lengths
- Cosine ranges between ‐1 and 1 (0 and 1 if all positive values in vectors) – closer to 1 is better.
- Top‐n, or scale for rating‐scale predictions.
- Strength: entirely content-based; understandable profile; easy computation; flexibility - can integrate with query-based systems and case based approaches
- Challenges and limitations 
  - Figure out the right weight and factors e.g. ratings
  - Highly simplified model, cannot handle interdependencies. e.g. I like comedies with violence, and historical documentaries, but not historical comedies or violent documentaries 

## Content based filtering deep dive
