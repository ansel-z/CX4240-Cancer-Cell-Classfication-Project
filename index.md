 
# **Classification of Acute Lymphoblastic Leukemia (ALL) in Blood Cell Images Using Machine Learning**
## Ji Ye (JC) Chun, Phillip Galonsky, Haoran Zhang

          
# Introduction
Acute lymphoblastic leukemia (ALL) is a type of cancer where the bone marrow produces too many immature lymphocytes. ALL is the most common cancer found in children, accounting for 30% of all pediatric cancer [1]. While ALL has one of the highest cure rates in all of childhood cancers (88.5%), it progresses rapidly, so quick diagnosis is critical for timely treatment. Among various diagnosis methods for ALL, the microscopic analysis of blood cells is the most economical and has the advantage of being relatively non-invasive. However, microscopic analysis is time consuming and requires the supervision of a medical expert. Moreover, the results of such analysis are limited by their subjective nature and reliance on the expert’s skill. Over the last decade, various machine learning methods have been implemented for the diagnosis of ALL to overcome these shortcomings. 

<img align="center" src="Figures/Intro_ALL.jpg">

# Overview

<img align="center" src="Figures/BlockDiagram.PNG">

# Dataset

  We obtained our dataset of cells with labels (normal versus cancer) from [University of Arkansas for Medical Sciences website]( https://app.box.com/s/xeclwwd2xep9ntljtgyptmt4k5wone9n). The dataset contains 15,114 microscopic images of cells from a total of 118 individual subjects, of which 69 have ALL and 49 are healthy. The images have been processed so that they contain only the cell nuclei on a black background. The dataset is divided into a training set and a preliminary testing set for which the ground truth for each image has been marked by an expert oncologist.
  
  This  dataset  was  also  used  for  our  IEEE  ISBI  2019  conference  challenge: Classification  of Normal vs Malignant Cells in B-ALL White Blood Cancer Microscopic Images. The challenge is available here:
  
  [IEEE International Symposium on Biomedical Imaging](https://biomedicalimaging.org/2019/challenges/)
  
  [CodaLab](https://competitions.codalab.org/competitions/20429)

## Images of ALL and normal cells
<img align="center" src="Figures/ALLvsHem_color.PNG">
<br>

# Feature Extractions
Visually, experienced hematologists distinguish ALL from normal cells by comparing the morphological, chromatin pattern, and color characteristics or cells [2]. In this project, quantitative morphological, texture, and color features are extracted from both ALL and normal cells in RGB, grayscale, and binary images (Table 1). 

<br>
<img align="center" src="Figures/ALLvsHem.PNG">
<br>

Morphologically, ALL nucleus is characterized by large, irregular size and shape and its rough boundary. The size of nucleus is computed by counting the total number of pixels inf the nucleus in the binary images. The perimeter of the nucleus is calculated by counting the number of pixels representing the nucleus boundary. Form factor, roundness, length/diameter ratio, and compactness represent the shape of nucleus. Nucleus boundary roughness is measured by finding the variance, skewness, and kurtosis of all the distance between centroid and nucleus boundary points.


The chromatin distribution reflects the organization of RNA and DNA in nucleus, and it is an essential feature for recognizing ALL. The chromatin distribution is extracted as texture features using Haralick’s textural feature extraction method [3] and Haar wavelet. Haralick’s method uses the gray-level co-occurrence matrix (GLCM) to find statistical data which summarize the relative frequency distribution in s grayscale image. Haar wavelet texture features are obtained by applying a high pass and a low pass filter to the grayscale images and taking means and variance in the horizontal, vertical, and diagonal directions.


Color is another important descriptor of ALL nucleus. ALL nucleus has the sparse red-purple color whereas normal hemoglobin nucleus has the color of blue-purple. The means of red, green, and blue color are computed from the RGB images, and the means of hue, saturation, and value are also extracted from the HSV images. In addition to the colors, the intensity mean and variance are computed from the grayscale images.


In total, 9 Morphological, 21 texture, and 8 color features are extracted. After obtaining the numerical data, the features are scaled to lie in the interval [0,1] using min-max scaling.

<p align="center">
<b> Table 1. Features </b>
<img align="center" src="Figures/feature_chart.png">
</p>
<br>

# Dimension Reduction
We applied two differenet dimension reduction methods: Random Forest (RF) and Principal Component Analysis (PCA).

<p align="center">
<b> Table 2. Top 10 features from random forest </b>
<img align="center" src="Figures/top_feature_chart.png">
</p>
<br>

 <img align="center" src="Figures/RF%20Feature%20Importance.PNG">

<br>
<br>
<p align="center">
  <b>Pairplot of Top 5 Features</b>
<img align="center" src="Figures/Pairplot.PNG">
</p>

<p align="center">
 <b>3D Scatter Plot of ALL vs. Normal with Top 3 Features</b>
<img align="center" src="Figures/ALLvsNormal_SCatter3D_RF.gif">
</p>
<br>
<br>

As you can see, there is some slight separation of ALL and normal cells over the first few features, but there is not very much. To try to improve separability, we applied PCA to the data.

<p align="center">
<b>Principal Component Analysis</b>
</p>
<img align="center" src="Figures/Scree_plot.PNG">
<br>

By the elbow test, we first elected to reduce our data to 6 principal components.

<p align="center">
 <b>3D Scatter Plot of ALL vs. Normal with Top 3 Principal Components</b>
<img align="center" src="Figures/ALL_Normal_Scatter3D_PCA.gif">
</p>
<br>
<br>

# Classification
The following figures reflect the training procceses done on the first 6 principal components. We applied 5 different classification models to the problem: Support Vector Machine (SVM), Random Forest (RF), K-Nearest Neighbor (KNN), Majority Voting (MV) and Stacking. The last two methods are enseble methods where we combined our SVM, RF and KNN models in hopes of further improving classification results.

### Support Vector Machine
For our SVM kernel, we used a the radial basis function (RBF) because it performed better than a linear kernel. To optomomize the hyperparamaters C and gamma we employed grid search 5-fold cross validation. We found that the optimal values for C and gamma were 10 and 3 respectively, which had an average accuracy of 84.7%.

<img align="center" src="Figures/SVM_PCA6_wide.PNG">

When we predicted on the test data with the tuned SVM, we got the following results.

<img align="center" src="Figures/svm_conf_PC6.png">

### Random Forest
To optomize the hyperparameters of our Random Forest classifier, we again applied grid search 3-fold cross validation, but this time with many more hyperparameters: number of estimators, bootstrapping (yes or no), maximum depth of estimators, the maximum number of features considered for each split, minimum samples required to split a node, minimum samples required to have on a leaf node. However, this model performed perfectly on our training data, indicating that it may have over-fitted to our train data. Thus, we created a second random forest classifier with more conservative hyperparameter inputs, e.g. smaller number of estimators and smaller maximum depth. We call this second classigier RF2. Below we see the test results of RF and RF2 on the left and right respectively. 

<p float="left">
  <img src="Figures/rf_conf_PC6.png" width="400" />
  <img src="Figures/rf2_conf_PC6.png" width="400" /> 
</p>

### K-Nearest Neighbor
For our KNN classifier, we used cross validation to optimize the hyperparameter K. The K value with the highest mean accuracy was 30.

<img align="center" src="Figures/KNN_PCA6_wide.PNG">

<img align="center" src="Figures/kkn_conf_PC6.png">

### Ensemble Methods
We used two ensemble methods comprised of the SVM, RF and KNN methods that we had already tuned. First, we did a simple majority vote classifier to see if this improved results. Second, we employed a stacking method, wherein the SVM, RF and KNN classifiers served as base estimators, and we employed a Logistic Regression (LR) classifier as the meta classifier. We tuned the LR hyperparameters (C and penalty) using grid search 5-fold cross validation.

<img align="center" src="Figures/Stacking.png">

Below are the results of the majority vote and stacking ensemble methods on the left and right respectively. 

<p float="left">
  <img src="Figures/mv_conf_PC6.png" width="400" />
  <img src="Figures/stack_conf_PC6.png" width="400" /> 
</p>

# Results and Comparison of Methods
<img align="center" src="Figures/ComparisonPCA6.PNG">

<img align="center" src="Figures/ROC_PCA6_wide.PNG">

<img align="center" src="Figures/auc_PC6.png">

In terms of accuracy, each model other than KNN performs about the same: around 73%. KNN performs the worst with 70.1% accuracy and SVM performs the best with 73.6%. We see a little more disparity between models when we consider AUC scores. By this metric, RF outscores the other classifiers with 0.719, and stacking performs a lot worse with a score of 0.643. Moreover, we see that our recall for each method is quite similair, between 93% and 94%. This indicates the percentage of cancer that is correctly identified. After viewing these results, we wondered if we were eliminating too much information by reducing to only 6 components, so we ran trained the models on 17 components, which recovered 99% of the variance. The results on this feature space were essentially the same. Indeed, we got similar results when training on only the 16 most important features as determined by random forest, as well as when we trained on the original 38 features with no dimensionality reduction at all.


<p float="left">
  <img src="Figures/ComparisonPCA6.PNG" width="400" />
  <img src="Figures/ComparisonPCA17.PNG" width="400" /> 
</p>
<p float="left">
  <img src="Figures/ComparisoRF16.PNG" width="400" />
  <img src="Figures/ComparisonFull.PNG" width="400" /> 
</p>

<p align="center">
<b> AUC Scores Over Different Feature Spaces </b>
</p>
<img align="center" src="Figures/auc_comparison_wide.png">


# Conclusion

This classification task was quite difficult, as cancer cells and normal cells often appear very similair to each other not only as images but also as extracted numerical features. With the exception of KNN, which was the clear worst predicter, all of our classifiers performed similairly on the four feature spaces that we tried, with accuracies hovering a little above 73%. The best overall model is the Random Forest trained on data before any dimensionality reduction process, which achieved 0.752 AUC. The best overall accuracy was 73.6% which was achieved by SVM trained on 6 PCs as well as RF trained on 17 PCs. While our models are not as accurate as we had hoped, they are a good start for this classification problem, proving more accurate than a random guess. Moreover, while the accuracy of our models is relatively low, the recall is high. That is, these classifiers correctly identify cancer a high percentage of the time. For the problem of diagnosing ALL, it is far better to have this situation than the opposite, where healthy people are almost always correctly identified but patients with cancer are not. 

# Future Work

- More features such as fractal geometry, texture features from Fourier transform, and local binary patterns can be extracted. 
- Larger ensemble methods can be used in classification.
- Convolutional Nueral Network can be applied directly to the cell images without feature extractions.

# References
 
[1]	Howlader N, Noone AM, Krapcho M, Miller D, Brest A, Yu M, Ruhl J, Tatalovich Z, Mariotto A, Lewis DR, Chen HS, Feuer EJ, Cronin KA (eds). SEER Cancer Statistics Review, 1975-2016, National Cancer Institute. Bethesda, MD, https://seer.cancer.gov/csr/1975_2016/, based on November 2018 SEER data submission, posted to the SEER web site, April 2019.
<br>
[2] S. Mohapatra, D. Patra, and S. Satpathy, "An ensemble classifier system for early diagnosis of acute lymphoblastic leukemia in blood microscopic images," Neural Comp. & Applic., vol. 24, pp. 1887-1904, Jun. 1, 2014.
<br>
[3] R. M. Haralick, K. Shanmugam, and I. Dinstein, "Textural Features for Image Classification," IEEE Transactions on Systems, Man, and Cybernetics, vol. SMC-3, pp. 610-621, Nov. 1973.
<br>
[4]	E. A. Mohammed, M. M. A. Mohamed, C. Naugler, and B. H. Far, "Toward leveraging big value from data: chronic lymphocytic leukemia cell classification," Network Modeling Analysis in Health Informatics and Bioinformatics, vol. 6, p. 6 (17 pp.), Feb. 18, 2017.
<br>
[5]	L. Putzu, G. Caocci, and C. Di Ruberto, "Leucocyte classification for leukaemia detection using image processing techniques," Artif. Intell. Med., vol. 62, pp. 179-191, Nov. 2014.
<br>
[6]	I. Vincent, K. Kwon, S. Lee, and K. Moon, "Acute lymphoid leukemia classification using two-step neural network classifier," in 21st Korea-Japan Joint Workshop on Frontiers of Computer Vision (FCV), 2015, pp. 1-4.
<br>
[7]	L. H. S. Vogado, R. M. S. Veras, F. H. D. Araujo, R. R. V. Silva, and K. R. T. Aires, "Leukemia diagnosis in blood slides using transfer learning in CNNs and SVM for classification," Eng. Appl. Artif. Intell., vol. 72, pp. 415-422, Jun. 2018.
<br>
<br>

Source of images: 
[Intro Image](https://www.indushealthplus.ae/medical-tourism/health-tips/acute-lymphocytic-leukemia-all.html)
