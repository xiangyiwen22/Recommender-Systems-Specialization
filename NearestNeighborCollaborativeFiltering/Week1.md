# User-User Collaborative Filtering
Equation 1 - Predictive Score for ith item and uth user <br>
![equation](http://latex.codecogs.com/gif.latex?S_%7Bu%2C%20i%7D%20%3D%20%5Cfrac%7B%5Csum_%7Bv%5Cepsilon%20u%7Dr_%7Bvi%7D%7D%7B%5Cleft%20%7C%20u%20%5Cright%20%7C%7D)

Equation 2 - Normalize it due to high-biased and low-biased users! <br>
![equation](http://latex.codecogs.com/gif.latex?S_%7Bv%2C%20i%7D%20%3D%20%5Cbar%7Br_%7Bv%7D%7D&plus;%20%5Cfrac%7B%5Csum_%7Bv%5Cepsilon%20u%7D%28r_%7Bvi%7D-%5Cbar%7Br_%7Bv%7D%7D%29%7D%7B%5Cleft%20%7C%20U%20%5Cright%20%7C%7D)

Equation 3 - Predictive Score weighted by user similarity between user u and user v <br>
![equation](http://latex.codecogs.com/gif.latex?S_%7Bu%2C%20i%7D%20%3D%20%5Cfrac%7B%5Csum_%7Bv%5Cepsilon%20u%7Dr_%7Bvi%7D%5Ccdot%20w_%7Buv%7D%7D%7B%5Csum_%7Bv%5Cepsilon%20u%7Dw_%7Buv%7D%7D)

Equation 4 (combine 2 and 3) Predictive Score weighted by user similarity between user u and user v <br>
- v not equal to U
- limit size
- min similarity <br>
![equation](http://latex.codecogs.com/gif.latex?S_%7Bv%2C%20i%7D%20%3D%20%5Cbar%7Br_%7Bv%7D%7D&plus;%20%5Cfrac%7B%5Csum_%7Bv%5Cepsilon%20u%7D%28r_%7Bvi%7D-%5Cbar%7Br_%7Bv%7D%7D%29%5Ccdot%20w_%7Buv%7D%7D%7B%5Csum_%7Bv%5Cepsilon%20u%7Dw_%7Buv%7D%7D)
