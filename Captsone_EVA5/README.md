#   Monocular Depth Estimation And Object Detection [potFork model]

### Project Description
In this project we are creating a CNN-Network that can do monocular depth estimation and object detection simultaneously.
*Use case: *
For autonomous robots to detect objects and estimate the depth

The potFork network
Input: Images containing objects to detect them and estimate the depth.
Output: 1. Depth estimate image
        2. Object detections with bounding boxes 

## Architecture
 Used transfer learning with few custom bottle neck layers in between
 The models used:
 1. Encoder:
            Resnext-101
 1. Decoder:
            MIDAS: For depth estimation
            YOLOv3: For object detection
            
  ![Image of model](https://octodex.github.com/images/yaktocat.png)
            
            ![Image of Yaktocat](https://octodex.github.com/images/yaktocat.png)
