# Paper Summary
## Spring 2020

### Augmentation for small object detection

This paper presented current issues for Mask RCNN to detect small objects.
  1. A few images contain small objects.
  2. Usually, small objects appear few times.

Proposed ways:
  1. Oversampling for small objects
  2. Copy small objects and paste somewhere else in the same images.
  1 and 2 achieved about 10% improvement.
  
Thoughts:
  1. Backbone in Mask RCNN is to extract features. However, small objects are ignored in most cases.
      So it is natural to think about how to enhance small objects. Methods of data augmentation in the paper seem a good way to do.
      Another way would be thinking about how to change structure of backbone so that it can extract small objects better.
      FPN is a good way.
      But for a typical combination of ResNet + FPN, we need to think about which feature maps we really need for small objects.

### Face Detectin Using Improved Faster RCNN
The paper presented a light-head RCNN. Attached a deformable layer after backbone(Resnet) to exploit image context and be robust to variations. The authors carefully designed anchors' size and a voted-based NMS straegy. They also used multi-scale training and testing.

Thoughts:
  1. it is worthy adding deformable layer to backbone. But where and how many layers?
  2. When dealing with small objects. not only anchors, but also IoU are required to be designed carefully. Because usually, objects do not take up all space in a RoI or in a box. If thinking in this way, then normal IoU is not good for small objects.
  3. designing a light-head can speedup the inference.
  4. Hard example mining could improve accuracy.


### An Improved Faster RCNN for Small Object Detection
This paper proposed a new way to calculate IoU so that not only intersaction areas being considered, but also non-overlap areas. In this way, faster RCNN could detect small objects ((0, 32] pixels) better. The authors also utilized many other technologies like Soft-NMS, RoI Align and FPN...

Thoughts:
  1. For some small objects, or some objects that only account for ~50% of its Bbox with different shapes, traditional IoU is really bad. Even if IoU is greater than some threshold, it is hard to say that really defect being included in that overlap areas. So this can be improved.
  2. One reason that makes 1 is that usually we are using normal boxes, that is, no matter how your object is being rotated, its ground truth box is still regular rectangle. If gt boxes can be also rotated, then this will improve accuracy.


### Autonomous Structural Visual Inspection Using Region-Based Deep Learning for Detecting Multiple Damage Types

This paper explores how to implement Faster RCNN on videos for defect detection.
The paper was published in 2017 as same as Faster RCNN was introduced, so there is no improvement.

Thoughts:
  1. Could be helpful when dealing with videos.


### Surface Defects Recognition of Wheel Hub Based on Improved Faster R-CNN
The paper deployed faster RCNN for wheel hub defect detection. Its backbone is ZF-net. Then, the paper added drop-out layers between FC layers in RoI-Heads.

Thoughts:
  1. The paper gives me a term: RoB(region of background), this is a term that needs to think about. 
  2. Is loss function for RPN is the best? How to understand the loss and how to improve it.
  3. Again, anchor's size need to be set carefully.
  4. IoU seems like a problem for abnormal objects.

## Fall 2019

### Automatic Fabric Defect Detection with a Multi-Scale Convolutional Denoising Autoencoder Network Model

The paper presented an approach: This approach is used to reconstruct image patches with a convolutional denoising autoencoder network at multiple Gaussian pyramid levels.

Main idea: reconstruct image patches (with noise) and get residual maps.

Pros:
  1. using autoencoders to detect which is good for the situation where there are not much defective images.
  2. ignoring background because of original image patches and reconstructed image patches leads to good accuracy.

Cons: 
  1. Using Gaussian Pyramid will trible the training time.
  2. Not good in real time production since it needs three models.
  
Thoughts:
  1. This is good if there are less defective images.
  2. The idea behind it is simple. Traditional methods will somehow affect by background and this method can ignore bg and from that, residual maps can be obtained easily.

### Fabric Defect Detection Using Activation Layer Embedded Convolutional Neural Network
Main idea: get a motif by autocorrelation, substract it from image by sliding window, and then take the "node map(probablity map)" as an activaion layer for CNN to enhance defects.

Pros:
  1. A hybrid method which combines correlation with CNN.
  2. bg will have less effect.

Cons:
  1. the first step is critical since the later activation layer is used. But the correlation is bad for 	getting a good motif.
  2. for each image, correlation is needed so that it becomes slow for sure.
  3. edge is somehow being ignored by correlation.
  4. Some small defects are omitted.

### A fast and robust convolutional neural network-based defect detection model in product quality control
Purely CNNs. First to classify based on different motifs (6 motifs in their dataset), then detect if defective or not.

Thoughts:
  1. For given dataset, this is a good way to detect.
  2. Not good for reality.

### A compact convolutional neural network for textured surface anomaly detection
Same as above, the model performs well on given dataset.

### Autoencoder-Based Fabric Defect Detection with Cross-Patch Similarity
The paper presented an unsupervised way for fabric detection. For training process, AEs is to learn how to reconstruct image patches. For inference, the model will select high-loss image patches from AEs, then calculate resdual patch. 

Thoughts:
  1. AE to reconstruct image patches.
  2. Calculate resdual image patches.
  3. No need to much images to train.

### Deformable Patterned Fabric Defect Detection With Fisher Criterion-Based Deep Learning
The paper is similar to above, to get resdual patches.
The paper has two models, one is for classify patches into 2 classes(d or non-d), another one is to reconstruct patches. 

### Distance-IoU Loss: Faster and Better Learning for Bounding Box Regression
Idea:
  1. find problems in GIOU loss and improve it.

Problem in GIOU:
  1. Compared with IOU loss, GIOU loss takes the area C that enclose GT and P boxes into account. This will make the NN learn that it should improve IOU, which means in the later evaluation(this depends on IoU very much), there will be improvements.
  2. GIOU is more useful when GT and P are far away from each other. This will give the NN a direction that where it should go to improve IOU.
  3. GIOU does not help much when P and GT have overlapes.
  4. GIOU becomes IOU when GT and P far away.

Distance-IOU:
  1. This IOU takes P center and C centers into account. Add a penalty term based on these Pcenter/Ccenter.
  2. Compared with GIOU, this still work when GT and P far away. So in non-overlap cases, this GIOU los will help a lot
  3. In the paper, SSD and YOLO v3 have a bigger accuracy improvement than Faster RCNN, this is because in faster rcnn, there are not too much cases where GT and P are not overlopped each other so that the improvement is about 2%(8-9% in one stage detector).

Thoughts:
  1. GIOU will help NN do a better job on evaluation. Originally, Smooth L1 loss is used in box regression, but IoU is used for accuracy evaluation. Smooth L1 is based on 4 coners instead of their IOUs.
  2. If use GIOU loss for box regression, this improves accuracy metrics (APs).

## Review

### Automated fabric defect detection—A review
A review for fabric defect detection.

1. 17 groups of wall paper
2. 70 kinds of defects.
3. non-motif vs. motif(more than 1 motif)

methods:
  1. statistical methods + NNs
	correlation + CNN
	Morphology
  2. Spectral + NN
	Fourier transform
	Wavelet transform
  3. ...

Issues:
  1. imbalanced dataset (more non-defective, less defective)
  2. defects are small w.r.t the whole image ( 1% - 10 %) depending on datasets.
  3. size varies. (pixels)
  3. more than 1 motifs.
 



## About to read

### Real-time Detection of Steel Strip Surface Defects Based on Improved YOLO Detection Network

### A semi-supervised convolutional neural network-based method for steel surface defect recognition

### A High-Efficiency Fully Convolutional Networks for Pixel-Wise Surface Defect Detection

### Segmentation-Based Deep-Learning Approach for Surface-Defect Detection


  
