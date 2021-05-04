# cat_detector

This is the implementation of a cat detector for a camera triggered alarm that goes off when a cat is nearby, or there are no birds in the frame of view. The procedure is as follow:

1. Using pretrained YOLO5x it creates a pseudo-ground truth for a dataset containing images of cats and birds.
2. It trains YOLO5x on the labeled data.
3. The trained detector is used to detect cats and birds in the image/video and alarms when reqiured.

The code for YOLO is taken from this repository: https://github.com/ultralytics/yolov3  


# Installation
```shell
cd yolo
pip install -r requirements.txt  # install the Python requirements 
```
# Usage

## 1. Download (or create) the Pseudo Ground Truth 
You can download the data containing images and labels from here: https://drive.google.com/drive/u/0/folders/1IeVnkjAW72G6wqXDOLA4Vs9B6Q8PgNlz 

```
$data
|-- images
    -- train
    -- val
`-- labels
    --train
    -- val
|   
```

Otherwise to create the pseudo ground truth for object detection annotations using YOLO5x run the following command:

```shell
python detect.py --source "pathtoimages" --weights yolov5x.pt --save-txt --nosave --name "datatype(train/val)"
```


## 2. Train
You can download the checkpoint for YOLO5x trained on cat and bird dataset from here (this model in trained for only 16 epochs): https://drive.google.com/drive/u/0/folders/1BgvAXS8-89aFjp_1BitjBk4cWMU9Vyw0 and putting it in yolo directory.


Otherwise train YOLO5x on this dataset using the following command:

```shell
python train.py --img 640 --batch 16 --epochs 10 --data cat-bird.yaml --weights yolov5x.pt  --cache
```



## 3. Evaluate 

Run the following to test the model:

```shell
python test.py --weights best.pt --data data/cat-bird.yaml --batch-size 16 --img-size 640
```

## 4. Deploy
Now the model is ready to be used in the camera tirggered alarm.

```shell
python detect.py --source "pathtoimage" --weights best.pt  --alarm
```

![alt text](https://github.com/sMamooler/cat_detector/blob/main/results/bird9289.jpeg)

![alt text](https://github.com/sMamooler/cat_detector/blob/main/results/cat266.jpeg)

![alt text](https://github.com/sMamooler/cat_detector/blob/main/results/no_bird.jpeg)


