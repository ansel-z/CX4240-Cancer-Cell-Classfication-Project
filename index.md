 
# **Classification of Acute Lymphoblastic Leukemia (ALL) in Blood Cell Images Using Machine Learning**
## Ji Ye (JC) Chun, Phillip Galonsky, Haoran (Ansel) Zhang

          
# **Introduction**
Acute lymphoblastic leukemia (ALL) is a type of cancer where the bone marrow produces too many immature lymphocytes. ALL is most common in young children and progresses rapidly, so quick diagnosis is critical for ALL patients to receive timely treatment. Among various diagnosis methods for ALL, the microscopic analysis of blood cells is the most economical and has the advantage of being relatively non-invasive. However, microscopic analysis is time consuming and requires the supervision of a medical expert. Moreover, the results of such analysis are limited by their subjective nature and reliance on the expert’s skill. Over the last decade, various machine learning methods have been implemented for the diagnosis of ALL to overcome these shortcomings. 


# **Dataset**

  A dataset of cells with labels (normal versus cancer) is obtained from [University of Arkansas for Medical Sciences website]( https://app.box.com/s/xeclwwd2xep9ntljtgyptmt4k5wone9n). The dataset contains 15,114 microscopic images of cells from a total of 118 individual subjects, of which 69 have ALL and 49 are healthy. The images have been processed so that they contain only cells on a black background. The dataset is divided into a training set and a preliminary testing set for which the ground truth for each image has been marked by an expert oncologist.
  
  This  dataset  was  also  used  for  our  IEEE  ISBI  2019  conference  challenge: Classification  of Normal vs Malignant Cells in B-ALL White Blood Cancer Microscopic Images. The challenge is available here:
  
  [IEEE International Symposium on Biomedical Imaging](https://biomedicalimaging.org/2019/challenges/)
  
  [CodaLab](https://competitions.codalab.org/competitions/20429)

## Images of ALL and normal cells
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsHem_color.PNG">
<br>

# Feature Extractions
The RGB cell images are converted to grayscale and binary for feature extractions.
<br>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsHem.PNG">

 9 Morphological, 21 texture, and 8 color features are extracted.
<center>
 
|Morphologial                         |Texture                                             |Color                                    |  
|:-----------------------------------:|:--------------------------------------------------:|:---------------------------------------:|
|Cell Size                       |Haralick Angular Second Moment                   |Red Mean                       |
|Perimeter                       |Haralick Contrast                                |Green Mean                     |
|Form Factor                     |Haralick Correlation                             |Blue Mean                      |
|Roundness                       |Haralick Variance                                |Hue Mean                       |
|Length/Diameter Ratio           |Haralick Inverse Difference Moment               |Saturation Mean                |
|Compactness                     |Haralick Sum Average                             |Value Mean                     |
|Boundary Roughness Variance     |Haralick Sum Variance                            |Intensity Mean                 |
|Boundary Roughness Skewness     |Haralick Sum Entropy                             |Intensity Variance             |
|Boundary Roughness Kurtosis     |Haralick Entropy                                 |                               |
|                                |Haralick Difference Variance                     |                               |
|                                |Haralick Difference Entropy                      |                               |
|                                |Haralick Information Measures of Correlation 1   |                               |
|                                |Haralick Information Measures of Correlation 2   |                               |
|                                |Haar Wavelet Approximation Mean                  |                               |
|                                |Haar Wavelet Horizontal Mean                     |                               |
|                                |Haar Wavelet Vertical Mean                       |                               |
|                                |Haar Wavelet Diagonal Mean                       |                               |
|                                |Haar Wavelet Approximation Variance              |                               |
|                                |Haar Wavelet Horizontal Variance                 |                               |
|                                |Haar Wavelet Vertical Variance                   |                               |
|                                |Wavelet Diagonal Variance                        |                               |
 
</center>
<br>
<br>

# Dimension Reduction
We applied two differenet dimension reduction methods: Random Forest (RF) and Principal Component Analysis (PCA)

**Top 10 features from random forest**

|Ranking|Feature   |
|:-----:|:--------:|
|1|Cell Size|
|2|Perimeter|
|3|Haralick Difference Entropy|
|4|Haralick Contrast|
|5|Red Mean|
|6|Value Mean|
|7|Haralick Information Measures of Correlation 1|
|8|Haralick Information Measures of Correlation 2|
|9|Hue Mean|
|10|Saturation Mean|

<p align="center">
</p>
 <img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/RF%20Feature%20Importance.PNG">

<br>
<br>
<p align="center">
  <b>Pairplot of Top 4 Features</b>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/Pairplot.PNG">
</p>

<p align="center">
 <b>3D Scatter Plot of ALL vs. Normal with Top 3 Features</b>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsNormal_SCatter3D_RF.gif">
</p>
<br>
<br>

<p align="center">
<b>Principal Component Analysis</b>
</p>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/Scree_plot.PNG">
<br>

<p align="center">
 <b>3D Scatter Plot of ALL vs. Normal with Top 3 Principal Components</b>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALL_Normal_Scatter3D_PCA.gif">
</p>
<br>
<br>

# Classifications
We applied 5 different classification models to the problem: Support Vector Machine (SVM), Random Forest (RF), K-Nearest Neighbor (KNN), Majority Voting (MV) and Stacking. The last two methods are enseble methods where we combined our SVM, RF and KNN models in hopes of further improving classification results.

## Support Vector Machine
For our SVM kernel, we used a the radial basis function (RBF) because it performed better than a linear kernel. To optomomize the hyperparamaters C and gamma we employed grid search cross validation.

## Random Forest
For our Random Forest classifier, we applied grid search cross validation to optomize many hyperparameters: number of estimators, bootstrapping (yes or no), maximum depth of estimators, the maximum number of features considered for each split, minimum samples required to split a node, minimum samples required to have on a leaf node. However, this model performed perfectly on our training data, indicating that it may have been over-fitted. Thus, we created a second random forest classifier with more conservative hyperparameter inputs, e.g. smaller number of estimators 

## K-Nearest Neighbor
For our KNN classifier, we used cross validation to optomize the hyperparameter K. The best performing value was 9.

## Ensemble Methods
We used two ensemble methods comprised of the SVM, RF and KNN methods that we had already tuned. First, we did a simple Majority vote classifier to see if this improved results. Second, we employed a Stacking method, wherein the SVM, RF and KNN classifiers served as base estimators, and we employed a Logistic Regression (LR) classifier as the meta classifier. 

<p align="center">
  <b>Pairplot of Top 4 Features</b>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/Stacking.png">
</p>

<p align="center">
 <b></b>
 
</p>
<br>

<p align="center">
 <b>ROC curve</b> 
</p>
<br>

<p align="center">
 <b>Accuracy comparison</b>
 
</p>
<br>

<p align="center">
 <b>Ensemble</b>
 
</p>
<br>

# Conclusion

<br>

# References
 
[1]	R. M. Haralick, K. Shanmugam, and I. Dinstein, "Textural Features for Image Classification," IEEE Transactions on Systems, Man, and Cybernetics, vol. SMC-3, pp. 610-621, Nov. 1973.
<br>
[2]	E. A. Mohammed, M. M. A. Mohamed, C. Naugler, and B. H. Far, "Toward leveraging big value from data: chronic lymphocytic leukemia cell classification," Network Modeling Analysis in Health Informatics and Bioinformatics, vol. 6, p. 6 (17 pp.), Feb. 18, 2017.
<br>
[3]	S. Mohapatra, D. Patra, and S. Satpathy, "An ensemble classifier system for early diagnosis of acute lymphoblastic leukemia in blood microscopic images," Neural Comp. & Applic., vol. 24, pp. 1887-1904, Jun. 1, 2014.
<br>
[4]	L. Putzu, G. Caocci, and C. Di Ruberto, "Leucocyte classification for leukaemia detection using image processing techniques," Artif. Intell. Med., vol. 62, pp. 179-191, Nov. 2014.
<br>
[5]	I. Vincent, K. Kwon, S. Lee, and K. Moon, "Acute lymphoid leukemia classification using two-step neural network classifier," in 21st Korea-Japan Joint Workshop on Frontiers of Computer Vision (FCV), 2015, pp. 1-4.
<br>
[6]	L. H. S. Vogado, R. M. S. Veras, F. H. D. Araujo, R. R. V. Silva, and K. R. T. Aires, "Leukemia diagnosis in blood slides using transfer learning in CNNs and SVM for classification," Eng. Appl. Artif. Intell., vol. 72, pp. 415-422, Jun. 2018.
<br>

