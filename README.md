# Papers' Summary

## Augmentation for small object detection

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
      
      
      
## Automatic Fabric Defect Detection with a Multi-Scale Convolutional Denoising Autoencoder Network Model

The paper presented an approach: This approach is used to reconstruct image patches with a convolutional denoising autoencoder network at multiple Gaussian pyramid levels.

Main idea: reconstruct image patches and get residual maps.

Pros:
  1. using autoencoders to detect which is good for the situation where there are not much defective images.
  2. ignoring background because of original image patches and reconstructed image patches leads to good accuracy.
Cons:
  1. Using Gaussian Pyramid will trible the training time.
  2. Not good in real time production since it needs three models.
  
Thoughts:
  1. This is good if there are less defective images.
  2. The idea behind it is simple. Traditional methods will somehow affect by background and this method can ignore bg and from that, residual maps can be obtained easily.
  
  
  
  
## Face Detectin Using Improved Faster RCNN


  
