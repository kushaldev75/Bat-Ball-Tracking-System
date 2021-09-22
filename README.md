# Bat-Ball-Tracking-System

<p align='center'>
<img src="https://user-images.githubusercontent.com/74819807/132974658-c488b0b5-f459-4210-9213-2d8316b10d4a.gif">
</p>


This repository contains my work on developing Deep learning based Bat and Ball Tracker. This is inspired from one of the [Dockship Challenge](http://dockship.io/challenges/60f5a5ae7b01760c32a439f7/ball-&-bat-tracking-hiring-challenge/overview). The aim of the challenge was to build Real-time Object detection system using International Cricket dataset. 

## Dataset

The dataset conatins -

- train - This dataset contains “Images” folder - which has images containing bat and ball. There is a CSV file “bat_ball.csv” which contains following columns “class (label)” “X axis (Top Left X-coordinate of the image )” “Y Axis (Top Left Y-coordinate of the image)” “width(width of the class (bat or ball))” , “height (height of the class (bat or ball))” , “name (Name of the image)”, “image_width”, “image_height”
- test_videos - Contain videos on which your model needs to be tested
- train data contain images of three different resolutions i.e. 811 images (680, 720), 2 images (640, 678) and 11 images (1080, 1144)

Here are some training set images-

<p align='center'>
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/dataset/train/images/U2_4_19.png" width="400" height="420">
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/dataset/train/images/U1_22_16.png" width="400" height="420">
</p>

And below are some videos from test set-

<p align='center'>
<img src="https://user-images.githubusercontent.com/74819807/132973112-77503549-4bf0-4be3-a87e-0b21878b26c6.gif" width="400" height="420">
<img src="https://user-images.githubusercontent.com/74819807/132973285-7686af27-1269-42b9-ab63-6e00e593d613.gif" width="400" height="420">
</p>

  
To download complete dataset, please visit dockship challenge [page](https://dockship.io/challenges/60f5a5ae7b01760c32a439f7/ball-&-bat-tracking-hiring-challenge/overview) and click on Download Dataset. It is zip file of 342.19 MB.

## Approach

- In order to build Real-time object tracking system, I chose YOLO family of architectures as my main model to train on given dataset. YOLO “You Only Look Once” is one of the most popular and most favorite algorithms for AI engineers. It always has been the first preference for real-time object detection. I used newest member of this family i.e. YOLOv5.
- Since YOLOv5 requires dataset to be in YOLO format. So I converted given default dataset format to YOLO format using YOLOv5_Formatting.ipynb notebook. The major difference is that the default dataset contained annotations in single csv whereas YOLO format requires separate txt file for each image with following specifications-
  - One row per object
  - Each row is ```class x_center y_center width height``` format.
  - Box coordinates must be in normalized xywh format (from 0 - 1). If your boxes are in pixels, divide ```x_center``` and ```width``` by image width, and ```y_center``` and ```height``` by image height.
  - Class numbers are zero-indexed (start from 0).

For me, this part of project i.e. preparing separate txt file from single csv was really challenging and great learning experience.

After preparation, file structure of dataset looks like this -

```
- data
    - train
        - img1.png
        - img1.txt
        - img2.png
        - img2.txt
        ...

    - val
        - imgval1.png
        - imgval1.txt
        - imgval2.png
        - imgval2.txt
        ...
      
    - dataset.yaml

```

- Training- To train YOLOv5 on my dataset, I used YOLOv5 official tutorial and made changes required for my dataset. I used Google Colab for carrying out training using GPU. I took YOLOv5x model pretrained on COCO Object detection dataset. Than I carried out training specifically for my custom dataset for 20 epochs which took almost half an hour. Yeah, Not too long!

## Result

Below is the result of Model tracking objects in real time on 7 test_set videos. (I combined all 7 videos and slowed down to 0.5x for proper observation)

<p align='center'>
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/results/combinegiftest.gif">
</p>


<p align='center'>
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/results/results.png" >
</p>


Below is the Mosiac of some images from the validation set. **On Left**, we have images with ground truth labels whereas **On Right** we have YOLOv5 detection results.


One observation we can make is that in some images model doesn't do well at predicting bat in particular. But it is also very interesting to see 1st image of middle row. It looks like model performs better than ground truth label in detecting the bat. Point to note is that this result came from using pretty much default parameters. Still model performance looks really well on test videos. If we tune the parameters, I believe ther is still room of improvement.


<p align='center'>
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/results/val_batch1_labels.jpg" width="360" height="400">
&emsp;
&emsp;
<img src="https://github.com/kushaldev75/Bat-Ball-Tracking-System/blob/main/results/val_batch1_pred.jpg" width="360" height="400">
</p>


