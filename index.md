 
#**Classification of Acute Lymphoblastic Leukemia (ALL) in Blood Cell Images Using Machine Learning**
## Ji Ye (JC) Chun, Phillip Galonsky, Haoran (Ansel) Zhang

          
# **Introduction**
Acute lymphoblastic leukemia (ALL) is a type of cancer where the bone marrow produces too many immature lymphocytes. ALL is most common in young children and progresses rapidly, so quick diagnosis is critical for ALL patients to receive timely treatment. Among various diagnosis methods for ALL, the microscopic analysis of blood cells is the most economical and has the advantage of being relatively non-invasive. However, microscopic analysis is time consuming and requires the supervision of a medical expert. Moreover, the results of such analysis are limited by their subjective nature and reliance on the expert’s skill. Over the last decade, various machine learning methods have been implemented for the diagnosis of ALL to overcome these shortcomings. 


# **Dataset**

  A dataset of cells with labels (normal versus cancer) is obtained from University of [Arkansas for Medical Sciences website]( https://app.box.com/s/xeclwwd2xep9ntljtgyptmt4k5wone9n). The dataset contains 15,114 microscopic images of cells from a total of 118 individual subjects, of which 69 have ALL and 49 are healthy. The images have been processed so that they contain only cells on a black background. The dataset is divided into a training set and a preliminary testing set for which the ground truth for each image has been marked by an expert oncologist.
  
  This  dataset  was  also  used  for  our  IEEE  ISBI  2019  conference  challenge: Classification  of Normal vs Malignant Cells in B-ALL White Blood Cancer Microscopic Images. The challenge is available here:
  [IEEE International Symposium on Biomedical Imaging](https://biomedicalimaging.org/2019/challenges/)
  [CodaLab](https://competitions.codalab.org/competitions/20429)

## Images of ALL and normal cells
<img align="center' src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsHem_color.PNG">

<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsHem.PNG">

# Feature Extractions

 9 Morphological, 21 texture, and 8 color features are extracted.
<br>
<center>
 |Morphologial                 |Texture                                       |Color                 |  
 |:---------------------------:|:--------------------------------------------:|:--------------------:|
 |Cell Size                    |Haralick Angular Second Moment                |Red Mean              |
 |Perimeter                    |Haralick Contrast                             |Green Mean            |
 |Form Factor                  |Haralick Correlation                          |Blue Mean             |
 |Roundness                    |Haralick Variance                             |Hue Mean              |
 |Length/Diameter Ratio        |Haralick Inverse Difference Moment            |Saturation Mean       |
 |Compactness                  |Haralick Sum Average                          |Value Mean            |
 |Boundary Roughness Variance  |Haralick Sum Variance                         |Intensity Mean        |
 |Boundary Roughness Skewness  |Haralick Sum Entropy                          |Intensity Variance    |
 |Boundary Roughness Kurtosis  |Haralick Entropy                              |                      |
 |                             |Haralick Difference Variance                  |                      |
 |                             |Haralick Difference Entropy                   |                      |
 |                             |Haralick Information Measures of Correlation 1|                      |
 |                             |Haralick Information Measures of Correlation 2|                      |
 |                             |Haar Wavelet Approximation Mean               |                      |
 |                             |Haar Wavelet Horizontal Mean                  |                      |
 |                             |Haar Wavelet Vertical Mean                    |                      |
 |                             |Haar Wavelet Diagonal Mean                    |                      |
 |                             |Haar Wavelet Approximation Variance           |                      |
 |                             |Haar Wavelet Horizontal Variance              |                      |
 |                             |Haar Wavelet Vertical Variance                |                      |
 |                             |Wavelet Diagonal Variance                     |                      |
</center>


# Dimension Reduction

Top 10 features from random forest.

<p align="center">
  Scatter Plot of ALL vs. Normal with Top 3 Features
</p>
<img align="center" src="https://github.com/ansel-z/CX4240-Cancer-Cell-Classfication-Project/blob/master/Figures/ALLvsNormalScatter3D.gif">




