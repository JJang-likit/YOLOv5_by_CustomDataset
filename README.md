# YOLOv5_by_CustomDataset(box)

## Project
- Jetson Nano를 탑재한 드론이 기동하면서 장애물(박스)을 탐지해서 발견했을 때 충돌 방지를 위하여 자동으로 멈춘다.

## Problem
- Jetson Nano에 YOLO v5를 설치했을 때 약 2프레임의 저조한 성능을 보였다.

## How to solve
- Jetson Nano에서 모델이 작동할 때 좀 더 빠르게 작동할 수 있도록 모델의 크기를 줄인다.
  - 모델의 탐지 Class 수를 80개에서 1개로 줄인다.
  - depth_multiple/width_multiple/anchors 수치를 조절해서 모델을 간단하게 설정한다.
  - Custom_Dataset을 이용해서 1개의 Class(box)를 인식하도록 훈련시킨다.
  - Custom_Dataset은 Roboflow.ai를 이용해서 약 1000장의 box 사진으로 구성되었다.

## Modification
1. Custom_dataset.yaml File Setting
  - Number of Class : 80 -> 1
  - names : ['box']
 
2. /models/yolov5n.yaml File Setting
  - Number of Class : 80 -> 1
  - depth_multiple : 0.33 -> 0.25
  - width_multiple : 0.50 -> 0.20
  - anchors
  [10,13, 16,30, 33,23] -> [16,30, 33,23]
  [30,61, 62,45, 59,119] -> [62,45, 59,119]
  [116,90, 156,198, 373,326] -> [156,198, 373,326]

3. 가중치 변경
  - yolov5s.pt -> yolov5n.pt

## Result
- 결과는 runs/detect/ 에서 확인이 가능하다.

## Limit
- box의 feature가 많지 않아서 훈련된 모델이 box와 비슷한 색상, 모양의 물체를 box로 인식하는 경우가 있다.
- 더 많은 데이터셋으로 훈련을 진행하면 성능 향상을 기대할 수 있지만 box의
