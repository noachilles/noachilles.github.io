---
title: "[공부] point cloud object detection이 그래서 뭔데?!"

categories:
  - study
tags:
  - [study]

toc: true
toc_sticky: true

date: 2023-01-27
last_modified_at: 2023-01-27

published: false
---

** object detection을 point cloud로 할 때 image data 기반의 object detection과 차이점이 무엇인지 정리하고 갈 필요가 있다고 생각했습니다.
** 찾아볼 내용: 네트워크 구조와 데이터세트, 성능을 찾아서 요약 및 정리할 예정입니다.
** 데이터세트와 성능은 32750799 논문을 참고해 작성하고, 네트워크 구조만 살피고 넘어갑시다. 지금은 구체적으로 보는 단계가 아님을 참고하세요!

# Point Cloud

현실에서 3D 공간 정보를 시각적으로 표현해주는 데이터
   Point Cloud 데이터를 기반으로 한 Classification, Object Detection, Semantic Segmentation 등의 연구가 활발히 이루어졌다.

# Point Cloud 데이터의 성질

## 정렬되지 않고 정형화되지 않은 데이터

* Point Cloud 데이터는 2D 이미지 데이터와 달리 정형화 되어있지 않다.
* Point Cloud 데이터는 3D 공간상의 수많은 점들을 순서 없이 기록하므로 딥러닝 모델에 있어 치명적이다.
* Point Cloud 데이터를 Voxel 형태의 정형화된 데이터로 전처리해 위같은 문제를 해결할 수 있지만 다음과 같은 단점을 가지고 있다.

### Sparse한 성질

주어진 데이터의 3D 공간 안에 Point의 수에 비해 empty space가 너무 많다.
   이러한 데이터는 연산량을 증가의 이유 중 하나일 뿐이다.
   전처리 작업을 통해 Point Cloud 데이터를 정형화 데이터로 변환해도 동일한 성질을 가진다.

# 다양한 Point Cloud Object Detection 모델

## VoxelNet

<img 01>
Feature learning net, Conv layer, RPN으로 구성되어 있다.

여러개의 Voxel Feature Encoding(VFE)를 stack하여 voxel 단위 feature를 뽑아낸다.
1. Feature Learning Network
각 point들의 feature에 voxel 내부의 locally aggregated feature를 concat 시키는 구조이며, 마지막 VFE 이후에는 Voxel마다 내부 포인트 feature들의 Element-wise Maxpool을 통해 Voxel 단위의 feature를 만들어 낸다.

   <b>Voxel Partition</b>: 주어진 point cloud를 D x H x W(z, y, x축에 해당)의 개수와 v_D, v_H, v_W 크기의 equally space voxel로 나눈다.
   
   <b>Grouping</b>: 하나의 voxel grid 내에 있는 point를 같은 group으로 묶는다. Point cloud는 sparse한 특징을 가지고 있기 떄문에 voxel은 다양한 숫자의 point를 포함한다.

   <b>Random Sampling</b>: 일반적으로 LiDAR sensor로부터 얻은 point cloud는 1 frame당 10만 개 정도의 point를 가지고 있다. 이는computation, memory 부담이 심하고 point density의 variance가 큰 요인이 된다. 이를 줄이기 위해 T개 이상의 point를 가진 voxel에서 T개의 point를 random하게 sample하여서 computation을 줄인다.
   sampling bias를 줄임과 동시에 training variation을 준다.

   <b>Stacked Voxel Feature Encoding</b>: VFE layer의 연속이다. VFE layer를 먼저 살피는 것이 의미 있을듯

   <b>VFE layer</b>: VFE 는 Point-wise Input가 Fully connected layer를 거치면 Point-wise feature를 얻을 수 있다. maxpool을 스킵한 것과 Element-wise Maxpool을 거친 Locally Aggregated Feature를 Point-wise에서 Concatenate한다.
   각 point들의 feature에 voxel 내부의 locally aggregated feature를 concat 시킴


2. Convolutional middle layers
1의 결과로 제시된 Voxel feature에 3D Conv를 적용한다.

3. Region Proposal Network
크기는 같고 각도가 0도, 90도인 2개의 anchor box를 사용한다.

### Additional

Point-wise: 점에 대한 것이라는 뜻이므로, Point-wise Input이라 한다면 점단위의 Input을 의미하는 것이다. 한편, Element-wise는 행렬 단위에 대한 것이다. wise의 의미를 알았으므로 위의 내용을 다시 한번 작성할 수 있다. 
point-wise input이란 무엇일까? 점 단위의 input이라고 볼 수 있다.

<img 02>

VoxelNet은 특징적인 feature representation을 학습하는 동시에 point cloud로부터 3D bounding box를 end-to-end로 찾아낸다. 
VFE layer라는 voxel내의 point들간의 상관관계를 학습하는 새로운 design을 제시하였다.여러 개의 VFE layer를 통해 point cloud를 equally space 3D voxel로 변환하고 3D convolution을 적용해 local 3D information을 얻은 다음에 RPN을 통해 detection result를 얻는 식으로 진행된다. 

Locally Aggregated Feature란? 이 부분은 해결하지 못했다.

## PointPillars

point cloud encoding을 hyper parameter 없이 진행해 vertical한 방향으로 encoding을 진행한다.
이러한 encoding 덕분에 3D Convolution이 아닌 2D Convolution 연산이 가능해져 빠른 연산이 가능하다.
end-to-end 학습이 가능한 방식이다.
3D detection을 하는 방법 중 voxel과는 다른 방법으로 접근하는 논문으로, 다른 논문들에서 자주 언급되는 논문이다.
* voxel은 x, y, z를 단위로 나누어 grid 형태로 grouping 했지만 pillar는 x, y에 대한 단위로 나누고 z축으로는 한계 없이 grouping한다.

### PointPillars Network

<img 03>
총 3개의 main stage로 구성되며,
Pillar Feature Net, Backbone(2D CNN), Detection Head(SSD)이 그것이다.
1. <b>Pillar Feature Net</b> 
2D Convolution 연산을 위해서 pseudo-image로 point cloud를 변환한다.
그림에서 보이는 것처럼 pillars이기 때문에 x, y 좌표는 있지만 z축으로는 막대기처럼 생기고 z 축에 대한 limit가 없기 때문에 hyper parameter가 필요 없다. point cloud 좌표로 구성되고 이를 변환할 때는 z축에 대한 limit이 없으므로 hyper parameter가 필요하지 않다. 
   pillar의 대부분은 비어있는 상태이며, 이는 voxel과 마찬가지로 point cloud의 sparse한 특성 때문이다. 이어서 (D, P, N)의 tensor로 encoding 된다.
   <span style='color: #808080'>(D는 pillar의 coordinate 이며 P는 샘플 당 non-empty pillar의 개수, N은 pillar 당 point의 개수이다.)</span>
   Batch Norm과 ReLU를 지나 최종적으로 (C, P, N) size의 tensor를 생성한다.
   그 이후 channel에 대해 max pooling을 이용해 (C, P) size tensor를 생성한다.
   Encoding을 한 후에 original pillar의 위치로 feature들을 돌려놓아 (C, H, W)의 pseudo-image를 생성

2. <b>Backbone(2D CNN)</b>
backbone은 두 가지 sub-network로 이루어진다. top-down network는 점점 작은 spatial resolution feature을 생성하고, 각 top-down network의 feature를 deconvolutional network로 디테일 및 화질 저하를 줄이고 concatnate한다.

3. <b>Detection Head</b>
SSD를 사용해 3D object detection을 한다.
2D와 차이점은 height, elevation이 추가된 regression target으로 사용된다.

### Additional

<b>SSD</b>
Single-Shot Detector로, backbone 모델과 SSD head를 가진다.
backbone 모델은 feature extractor로서 image classifiaction network이다.


## PointRCNN

### PointRCNN for Point Cloud 3D Detection

<img 03>
<img 03_1>
proposal을 먼저 생성하는 2-stage 기법 중 하나로, 2D세어 3D로의 2단계 직접적인 확장은 포인트 클라우드의 불규칙한 포맷과 매우 큰 3D 탐색 공간 때문에 필요하다.

그림의 윗부분이 bottom-up 3D proposal generation을 수행하는 stage-1이 되고, PointNet++가 backbone network가 되어 point-wise feature vector를 얻으며 feature를 이용해 Bin-based 3D box generation과 Foreground Point segmentation을 수행한다.
foreground point는 object에 속하는 point를 말하며, Foreground Point Segmentation으로 분류한다. 
segmentation을 통해 얻은 foreground point들에 대해서만 3D Box Generation을 수행한다. Foreground Point Segmentation을 먼저 수행하고, 그 결과를 이용해 Bin-based 3D Box Generation을 수행한다.