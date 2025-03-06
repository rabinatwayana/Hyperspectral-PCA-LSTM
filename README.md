# PCA-Driven Dimensionality Reduction and LSTM-Based Classification for Hyperspectral Imagery


## Background
Hyperspectral imagery (HSI) provides a rich spectral information by capturing hundreds of contiguous spectral bands for each pixel, making it a powerful for detailed classification tasks. However, the high dimensionality of HSI data presents significant computational challenges and often includes redundant information due to high inter-band correlation. To address these issues, Principal Component Analysis (PCA) is frequently employed to reduce dimensionality by selecting the most relevant components while preserving critical spectral variance. This study explores the application of Long Short-Term Memory (LSTM) networks, which are well-suited for sequential data, to classify hyperspectral imagery. By treating the spectral bands as a sequence, the LSTM model can effectively learn the spectral dependencies between bands, enabling more accurate classification. 

The **aim** of this case study is to classify the hyperspectral imagery by combining PCA for dimensionality reduction with LSTM model for spectral dependency learning, optimizing both computational efficiency and classification performance. 

## Study Area and Data Source
The Indian Pine hyperspectral dataset and its corresponding ground truth data were downloaded from  University of the Basque Country  [website](https://www.ehu.eus/ccwintco/index.php/Hyperspectral_Remote_Sensing_Scenes). This data was collected by Airborne Visible/Infrared Imaging Spectrometer (AVIRIS) over north-western Indiana in USA, 1992 which contains 145×145 pixels with 20m spatial resolution, and 200 spectral bands after removing water absorption wavebands covering from 400nm to 2500nm. The ground truth datasets consist of 16 classes and approximately 10000 samples.

## Methodology
The model architecture consists of four sequential LSTM layers. Following the LSTM layers, the output is passed through a flattening layer, followed by a dropout layer (with a dropout rate of 0.2) to mitigate overfitting. A fully connected dense layer with ReLU activation is applied, accompanied by batch normalization to stabilize and accelerate training. The output layer consists of a dense layer and softmax activation, enabling multi-class classification by providing class probabilities.The model was trained using 75% data, with 5% allocated for validation, while the remaining 25% of the sample points were reserved for testing. To train the model, the RMSprop optimizer was used with a learning rate of 0.001, 50 epochs, and a batch size of 64.  To reduce the high dimensionality of the dataset, Principal Component Analysis (PCA) was applied with n_components values of 0.98, 0.99, and 0.999, to select the minimum number of principal components required to retain 98%, 99%, and 99.9% of the variance in the data, respectively. After applying PCA, the dimensionality of the dataset was reduced from the original 200 features to 15, 25, and 64 principal components for the respective variance thresholds. 

## Results and Discussions
Table 1 represents classification accuracy for three different principle components. The classification accuracy using 64 principal components was observed to be lower than that with 25 components. This is likely because the additional components, while capturing more variance, also include noise and less discriminative features, which may reduce the classifier's ability to generalize effectively. By retaining only 25 components, corresponding to 99% of the total variance, the PCA transformation emphasized the most critical features while discarding redundant or irrelevant information, leading to improved classification performance.

<div align="center">
  
| PCA             | PCA 15 | PCA 25 | PCA 64 |
|-----------------|--------|--------|--------|
| Overall Loss    | 0.67   | 0.355  | 1.127  |
| Overall Accuracy (%) | 83.36 | 94.08  | 67.36  |

Table 1: Classification accuracy for different principle components

![image](https://github.com/user-attachments/assets/16c0c16b-3cdf-4f3f-b049-35a3925d5949)
Figure 1: [Left]- Classification Map for labelled data  [Right]-Crop Classification Map obtained from 25 principle componentt
</div>

Figure 1 (right) depicts the crop classification map, which consists of 16 distinct classes derived using 25 principal components from the hyperspectral dataset.

### Reference 
https://github.com/mtalebizadeh/hyperspectral-image-processing
