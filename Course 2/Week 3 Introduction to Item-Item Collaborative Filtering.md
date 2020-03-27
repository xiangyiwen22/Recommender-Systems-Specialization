# Item to Item Collaborative Filtering
## Introduction
### Motivation 
- User‐User CF sparsity: When we have millions of items but only thounds of users with small number of ratings, too often there are points where no recommendation can be made. Solutions: “filterbots”, item‐item, and dimensionality reduction.
- Computational performance: With millions of users (or more), computing allpairs correlations is expensive; Even incremental approaches were expensive; And user profiles could change quickly –> needed to compute in real time to keep users happy

