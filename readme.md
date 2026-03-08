# Weakly-suprevised Crack Segmentation

## Overwiev of the problem
This problem is based on [Kaggle Crack Detection Dataset](https://www.kaggle.com/datasets/lakshaymiddha/crack-segmentation-dataset) containing over 11k of images. The goal was to detect whether there is a crack and create the segmentation map. The model however should be trained using only binary labels (crack/ no crack). For this problem it was also specified that it cannot be used domain-specific preprocessing or feature extraction.

# Dataset overwiev
All the images in dataset come in size of 448x448. For this problem main augmentation forms were rotation and flip. Some other methods were tested (e.g. ColorJitter or CutMix) but they did not contribute to better results so they were not used. The dataset was internally split into train and test. In this code additional validation subset is created from the train set.

# Architectures
## CNN with CAM
First proposed solution revolves around **Class Activation Maps (CAMs)**. They are popular solution in the field of xAI as they do not need heavy architecture modifications and produce relatively good results and since they are used as extension of traditional classifier they don't extend training time. 
### Classification Result
Since dataset is heavilly imbalanced accuracy is not valid metric to use. Therefore during training whole confusion matrix were yield. Having this it's possible to calculate: 
- Precision: 0.997 
- Recall: 0.969
- F1 score: 0.983 
### Segmentation Result
To measure segmentation results two metrics were introduced: *PR curve* and *ROC curve*. Area under those curves give rough estimator of model performance, but the shape (especially for PR curve) is also important.
Furthermore some attention map postprocessing functions were tested to see if they can contribute to better results. By using gaussian blur with treshold slightly better PR-auc was achieved.

## Transformer based with attention rollout
Second solution is based on Vision Transformes (ViT) and process called attention rollout. To be able to train them in reasonable time on home PC the images were resized to 224x224.
### Classification Result
- Precision: 0.988
- Recall: 0.96
- F1 score: 0.974 
### Segmentation Result
# Conclusions
- Classification task is easy for this dataset
- Both methods may provide usefull informations but are far from correct
- CNN with CAMs achieved better results when it comes to both segmentation and classification. This is probably caused by cracks being more "local" than "global" feature and CNNs perform better in such scenarios.
- Problem specific preprocessing (like blur, playing with contrast or edge detection) could be usefull but was not allowed for this task