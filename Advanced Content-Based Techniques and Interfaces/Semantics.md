# Beyond TFID - Semantics (Pasquale Lops) 

- Matrix factorization and general latent factor model - poor explanation 
- Re-emergence of interest in content based filtering 
- Paper: Recommending new movies: even a few ratings are more valuable than metadata
- Social media: opinions and feelings 


Advanced content based filtering: Deep comprehension of the information conveyed by content crucial to improve the quality of recommender system 
# semantics 
- Entity linking: keywords in wiki entries 
- Distributional hypothesis: meaning of words determined by analyzing their usage (co-occurrence with other words) in large corpora of documents - beer vs wine
  - Word embedding: low dimensional vectors of real numbers. Methods to generate mappings:
    - SVD on term co-occurrence matrix (latent semantic indexing)
    - Random projection on term co-occurrence matrix, no need for factorization, random indexing 
    - Word2Vec: Neural Networks 


Advantage of semantic-aware RS: richer representations allow to 
- Overcome limited content analysis
- Provide better explanations 
- Foster unexpectedness and serendipity (机缘巧合)


What factors have led to the increased incorporation of content-based techniques into recommendation?
- The increased availability of content information through social tagging and reviews
- The increased availability of semantic relationship data through Wikipedia, BabelNet, etc.
- The desire to overcome the limitations of collaborative techniques such as the lack of meaningful explanations.

 
