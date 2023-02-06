---
title: "[논문] VoxelNet-1"

categories:
  - review
tags:
  - [review]

toc: true
toc_sticky: true

date: 2023-01-30
last_modified_at: 2023-01-30

published: false
---

** 혼자 공부하기 위해서 적는 글, 다른 리뷰나 논문을 읽으며 모르거나 이해가 어려운 부분은 <span style='background-color: #ffdce0'>빨간 형광펜으로 표시하고 뒤에 내용을 따로 첨부한다.</span>

# Abstract

   RPN을 sparse한 특성이 있는 point cloud에 적용하기 위해서 hand-crafted feature에 많은 노력이 기울여졌다.
   Voxel은 manual feature engineering의 필요성을 줄이기 위해 제안된 모델로, 1-stage 모델이며 bounding box detection을 수행한다.
   3D 공간의 Voxel로 point cloud를 동일하게 분배하고, 새로 소개되는 VFE layer를 포함한다. 

# Introduction

<span style='color: #808080'>
이전 연구에서 
<span style='background-color: #ffdce0'>hand-crafted feature</span>
를 사용해 encode하는 시도들이 많았는데 hand-crafted feature는 엔지니어의 노력이 많이 들어가며 3D 정보를 잘 활용하지 못하므로 information bottleneck(병목현상) <span style='background-color: #ffdce0'> 두 사건 사이의 인과관계가 어렵다. </span> 이 된다.
   PointNet, PointNet++는 machine learned feature를 사용하지만, 각 포인트마다 연산이 요구되므로 high computation. 이러한 문제를 해결하기 위해 VoxelNet을 제시
</span>


      




# Contribution

<span style='color: #808080'>
1. VoxelNet, manual feature에서 오는 information bottlenecks를 해결한 machine learned feature를 사용한 network
2. sparse point 구조를 고려한, VoxelNet을 구현하는 효율적인 방법
3. KITTI SOTA
</span>

# VoxelNet Architecture

<span style='color: #808080'>
<img 01>
Feature learning net, Conv layer, RPN으로 구성됨

1. Feature learning net
<img 02> 여러 개의 Voxel Feature Encoding(VFE)를 stack하여 voxel단위 feature를 뽑아냄
VFE: 각 point들의 feature에 voxel 내부의 locally aggregated feature를 concat 시킴
마지막 VFE 이후에는 Voxel마다 Voxel 내부의 포인트 feature들의 Element-wise Maxpool을 통해 복셀 단위의 feature를 만들어냄

2. Conv 
3.1의 Voxel feature에 <span style='background-color: #ffdce0'>3D conv</span>를 적용
3D convolutional middle layer to consolidate the vertical axis: PointPillars

3. RPN
크기는 같고 각도가 0도, 90도인 2개의 anchor box를 사용
foreground score와 box regression값을 output
</span>

# Conclusion

# Additional

## hand-crafted feature

전문가에 의해 고안된 아이디어를 바탕으로 직접 설계된 수제 특징
반댓 개념은 end-to-end 방식으로 입력과 출력만 보고 중간 과정은 기계가 알아서 학습하는 것 