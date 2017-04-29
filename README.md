# Vehicle Detection


The Project
---

## Data Exploration
---
I used images from the GTI and KITTI Dataset. The resulting Dataset is not balanced. So I downsampled the number of noncar-images to the number of car images. Also I deleted in the GTI-Dataset several images of cars which had more than 4 images.



## HOG-Feature Extraction
---
The HOG parameters have been extracted by "ALL"-HOG channels. The parameters have been set to:
orient=12 , pix_per_cell=8, cel_per_block=2, color_space=YCrCb. 
All parameters have been determined by an iterative approach. 
To gain more Information for identification tasks features from spatial binning and color histograms have been extracted. Before feeding our classifier with the extracted features we normalize all features and shuffle the data. (Codeblock 11).
The results are presented in figure 1:
### image in here
---
## Sliding Window
To detect cars in an image we use the sliding window technique. The sliding window extracts all features inside it's window and feed it to the classifier for prediction tasks. If the classifier detects a car we store the actual windowposition inside a vector. 
This approach is computationally expensive because we slide through all images. To reduce computing time we slide through a given region for different scales. 
The result is presented in figure 2:



To optimize my classifiers performance I used the heatmap-technique. Heatmaps are arrays which are fed with binary classification results of our trained classifier. If the classifier detects a car we store it's window position inside our heatmap. If we don't classify an object,  we don't store a window position inside our heatmap. 

To improve the detection rate of our pipeline we have to create a high accuracy neighbourhood based on overlapping windows. The more overlapping windows / areas we have, the more accurate the classification is. This is implemented to avoid to draw false positives in our video. To realize this neighbourhood we store all detections of the last 10 frames in a "deque". In the following step we calculate the sum of, feed it to our heatmap and threshold it. The tresholdvalue is 6.
A detection will be drawn if our classifier detects at least 6 overlapping windows. 

We do this to remove the number of false positives -> Inside an image detected spots are classified as a car, which is not a car.



### Discussion
---
The classification task by the use of SVM has high computational cost and its hard to use it for real-time classification. The HOG-feature extraction is unefficient. In the next step I'm going to implement a more cost effective approach by only extract the Hog features once and evaluate them for every window. This should reduce the computational cost. Anoter approach is to implement a DNN to detect cars.

This pipeline will fail if we drive on the right or middle lane of the track. For this video I just scan the lower right triangular part of the image to reduce the computational cost.
