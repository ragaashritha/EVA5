#   Monocular Depth Estimation And Object Detection [potFork model]

### Project Description
In this project we are creating a CNN-Network that can do monocular depth estimation and object detection simultaneously.

 **Use case:**
For autonomous robots to detect objects and estimate the depth

The potFork network
Input: Images containing objects to detect them and estimate the depth.
Output: 1. Depth estimate image
        2. Object detections with bounding boxes 

## Architecture
 Used transfer learning with few custom bottle neck layers in between.
 The models used are:
 1. Encoder:
            Resnext-101
 2. Decoder:
            MIDAS: For depth estimation
            YOLOv3: For object detection
 The model architecture looks like below
            
  ![Image of model](https://github.com/ragaashritha/EVA5/blob/main/Captsone_EVA5/pot_fork_model.png)
  
  ### Encoder: 
  Resnext-101 is used as the encoder part of the model. YOLOv3 had darknet, a form of densenet as the encoder part which is replaced with renext-101 for the current
  architecture.
  
  The encoder takes in the image as input and gives four different resolutions as output
  o1: Imagesize/4
  o2: Imagesize/8
  o3: Imagesize/16
  o4: Imagesize/32
  
  ### YOLO decoder:
  Outputs of the encoder are connected to the YOLO decoder through a bottleneck layer.The architecture had 9 anchors which are to be specified in the cfg file. 
  The output of YOLO is a convolutional feature map that contains the bounding box attributes along the depth of the feature map. The attributes bounding boxes predicted by a     cell are stacked one by one along each other. So, if you have to access the second bounding of cell at (5,6), then you will have to index it by map[5,6, (5+C): 2*(5+C)]. This   form is very inconvenient for output processing such as thresholding by a object confidence, adding grid offsets to centers, applying anchors etc.
  
  Detections happen at three scales, the dimensions of the prediction maps will be different. Although the dimensions of the three feature maps are different, the output
  processing operations to be done on them are similar. We resize the detections map to the size of the input image. The bounding box attributes here are sized according to the   feature map (say, 13 x 13). If the input image was 416 x 416, we multiply the attributes by 32, or the stride variable.
  
  ### Depth decoder:
  The encoder outputs o1, o2, o3, o4 are passed into the decoder where they are connected to the respective upsampled blocks called feature fusion blocks through scratch
  layer. Feature fusion blocks are used to bring the resolution of the ouputs to the input resolution. ouput: [batch_size, img_size, img_size]
  
  The command to train the potFork model
  ````
  !python fork_train.py --cfg /content/EVA5-JEDI/Captsone_EVA5/cfg_yolo/yolov3-custom.cfg  --data '/content/drive/MyDrive/YoloV3/data/customdata/custom.data' --batch 32 --cache   --epochs 20 --nosave --train_decoder=depth
  ````
  
  The arguments to use:
  
    --cfg : Text file to generate base yolo model based on which fork model is built
   
    --data: Location file of labels and images for both train and test data
  
    --train_decoder: The argument to specify which part of fork to train
  
                       
                       all - train both yolo and depth
                       
                       depth - train depth decoder
                       
                       yolo - train yolo decoder
                       
  Training of one of the model i.e., yolo or depth happens as per the argument given. 
  
  The loss functions used are:
   
    Depth loss: SSIM(structural similarity index measure) and MSE
   
    YOLO loss: stock yolov3
  
  
  The repository structure is mentioned below:
  
    model.py	Script where fork model is defined. Fork model consists of encoder, depth_decoder, yolo_decoder, fork class

    model_yolo.py:	Base model of yolo v3 on which yolo_decoder modules are taken and a network is built

    train_fork.py and test.py:	Train and evaluaton of the model during Training
 
    utils_yolo:	All the util functions like parsing and loading data e.t.c to run DarknetV3(yolov3) model

    blocks.py:	utility functions for building Depth decoder

    depth_loss:	SSIM function for evaluation for depth loss
    
   The loss for yolo object detection, monocular depth estimation and the pot fork model are found to be decreasing with increase in epochs.
    
   YOLO loss 
   
   ![Image of model](https://github.com/ragaashritha/EVA5/blob/main/Captsone_EVA5/yolo_loss.PNG)
   
   Pot fork loss
   
   ![Image of model](https://github.com/ragaashritha/EVA5/blob/main/Captsone_EVA5/pot_fork_loss.PNG)
   
   
   
  
  
  
  
            
        
