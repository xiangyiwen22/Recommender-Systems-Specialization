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
- TF = # of occurrences of a term in the doc (count; 0/1 boolean freq occurring above threshold; `log(tf+1)`; Normalized freq divided by doc length 
- IDF = How few doc contain this term; e.g. `log(#Doc/ #DocWithTerm)`
- Automatic demotion of stopwords, common terms; Promotes core terms over incidental ones
- Fails when core term/concept isn't used (much) in doc (e.g. legal contracts)
- Application in CBF
  - create a profile of doc/object (e.g. a movie could be described as a weighted vector of its tags)
  - combine TFIDF profile with ratings to create user profiles, and then matched against future docs
  



## Content based filtering deep dive
