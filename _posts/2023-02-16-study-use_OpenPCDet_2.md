---
title: "[공부] OpenPCDet 설치 및 실행기-(1)"

categories:
  - study
tags:
  - [study]

toc: true
toc_sticky: true

date: 2023-02-16
last_modified_at: 2023-02-17

published: true
---

** 실험을 위해 OpenPCDet을 설치하고자 하나, 지난번에 제대로 설치 되지 않았다. 이번에는 python 3.9를 바탕으로, 환경 설정을 모두 다시 해보고자 한다. **

# OpenPCDet 설치기

<img 1>
spconv와 OpenPCDet의 version이 맞아야 한다는 글을 확인한 후 spconv의 버전을 맞추기 위해서 사이트에 들어왔다. 사진에 보이는 것처럼 spconv를 설치하기 전에 확인해야 하는 여러가지 주의 사항이 있다.
나는 windows 환경에서 동작하기를 바라고 있으므로, windows 환경에 맞게 설정하고자 한다.
## spconv version 2.x 설치 조건

1. python >= 3.7
2. CUDA toolkit >= 11.0
3. (Windows 10/11) python 3.7-3.11 and CUDA 10.2/11.4/11.7/12.0
    (Linux) python 3.7-3.11 and CUDA 10.2/11.3/11.4/11.7/12.0

현재 python은 3.9 버전이 설치되어 있으므로 변경할 필요 없는 것 같고, CUDA를 11.7 version으로 설치하자.
CUDA toolkit 설치 이후 PIP으로 spconv를 설치할지 development를 위한 source로 설치할지 결정할 수 있는데, 명확한 기준을 잘 모르겠다. 우선 읽어본다.
C++ 코드를 변경할 경우에 development를 위한 source로 설치해 사용할 수 있는 것으로 보인다. PIP을 사용해 설치해보자.

cuda toolkit 11.7과 spconv를 설치하였다.
# 문제상황

이제 문제가 슬슬 발생하기 시작한다. 한 번에 될 거라고는 생각조차 안 했기 때문에 이전에 떴던 오류가 동일하게 발생하지 않는 것만으로도 감사하다.

## CUDA를 찾을 수 없음
<img 2>

cuda 설치가 제대로 되었는지 version 확인도 하지 않은 채 사용한 나의 잘못이다... 바로 가서 확인을 해보자!

## Easy_install error
<img 3>

이런 식으로 오류가 떠서 확인해보니 pip을 통해 setuptool을 설치하라고 한다.
~~~
pip install setuptool==58.2.0
~~~
으로 58.2.0 version을 설치한다.

그래도 안 된다... 확인해보니 Windows에서 OpenPCDet을 설치 완료했다는 사람은 보이지 않고 그저 Error 뜬다는 사람만 보인다.
