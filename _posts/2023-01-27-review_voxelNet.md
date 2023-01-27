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

# Point Cloud 데이터를 다루는 인공지능

## PointNet

* Point Cloud 데이터를 그대로 입력해 학습하는 모델로, Point Cloud 데이터가 순서 없이 나열된 비정형 구조임에도 기하학적 성질을 유지할 수 있다.