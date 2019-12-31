# Critique based recommendation (Pearl Pu)
Critique based recommendation VS rating based recommendation 
- Users express preference over feature of products 
- High stake domain, single shot, short term
- People’s preference change based on the availability of products on the market, but this algorithm is supposed to capture long-term and stable preference

We change, and more precisely we change our preferences based on the available options shown to us ---- The adaptive decision by John Payne 


multi attribute utility theory or MAT
1. User rates each feature on a scale of 0-5, which provides priority of the features to the customer
2. For each product system, we assign utility score to each feature and get a weighted sum. E.g. if the user values the brand the most, a product with the matched brand will gain a lot of score. 
3. Linear algorithm, very easy to compute; preference is independent of each other, assume that the screen size doesn’t affect the weight of laptop- that’s why we use compound critique 
Direction of utility: some preferred features gain score, and unpreferred feature lose score (price too high, etc.)
4. How to deal with the users preference change of time - compound engine educate users  

- Universal preference: to laptop - the lighter, the better; the larger memory size, the better …
- Personal preference: brands, etc.

