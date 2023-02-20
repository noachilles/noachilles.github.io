---
title: "[공부] OpenPCDet 설치 및 실행기-(1)"

categories:
  - study
tags:
  - [study]

toc: true
toc_sticky: true

date: 2023-02-13
last_modified_at: 2023-02-13

published: true
---

** VoxelNet을 구현한 코드를 사용해보기 위해서 OpenPCDet을 활용하고자 하나, cuda version 충돌로 인해 진행되지 않는 상황임. cuda의 download부터 다시 시작해볼 것 **

# CUDA 설치하기
* cuda version 변경으로 검색하고, 윈도우 환경에서 CUDA 버전을 변경하는 게시글을 따라한다.

우선 설치하고자 하는 version의 CUDA의 Development와 Runtime 항목을 설치하고, CUDA 설치 후, 고급 시스템 설정 -> 환경 변수 항목을 통해 CUDA 설정을 확인해야 한다.
시스템 변수에서 새 항목을 만들어 CUDA_PATH 를 이용하고자 하는 버전으로 변경한다.

cmd로 nvcc -V를 확인!
<img 1>

# OpenPCDet 환경 세팅
그리고 이제 본격적으로 OpenPCDet을 사용하기 위한 환경 세팅이 시작된다. CUDA를 설치하는 것은 아주아주아주 기본적인 일이기 때문에 금방 해결하고 다음 절차로!

<a>https://download.pytorch.org/whl/torch_stable.html</a>
위 링크에 들어가서 설치된 cuda version과 맞는 pytorch를 설치한다.
코드는 아래처럼
~~~
$ pip install torch==1.10.1+cu114 torchvision==0.11.2+cu114 -f https://download.pytorch.org/whl/torch_stable.html
~~~
실행이 안 된다. 왜냐면 CUDA114를 지원하는 pytorch version이 없어서 ㅋㅋ... 우선 spconv를 사용하기 위해서 cu116 version이 있는지 확인하자.

있을 것 같다는 막연한 생각으로 다음의 pytorch, cudatoolkit을 우선 설치했다.
<a>https://pytorch.org/get-started/previous-versions/</a>
이 링크를 참고해서 cuda 11.6 버전의 
~~~
$ conda install pytorch==1.12.0 torchvision==0.13.0 torchaudio==0.12.0 cudatoolkit=11.6 -c pytorch -c conda-forge
~~~
이렇게 설치

그래서 다음과 같은 version 정보로 정리할 수 있다.
--version--
python: 3.9.14
CUDA: 11.6
Pytorch: 1.12.0
torchvision: 0.13.0
torchaudio: 0.12.0
cudatoolkit: 11.6

## 참고

여기서 nvcc --version 혹은 nvcc -V 코드를 실행하면, 아까 설치해둔 11.4 version만 표기되는 것을 확인할 수 있다. nvcc --version은 conda 내에 설치된 cuda가 아니라, base의 cuda 버전을 출력하기 때문에, 다른 방식으로 확인해야 한다.
~~~
$ conda list cudatoolkit
$ conda info
~~~

위의 두 가지 코드로 확인할 수 있다.
<img 2>

출처: <a>https://nuggy875.tistory.com/146</a>

# 문제 상황

여기도 마찬가지로 계속 error가 발생하는데, 문제 상황은 여러 개 있지만 우선
~~~
python setup.py develop
~~~
위 코드가 실행되지 않았다. 그래서 찾아보니 command ['ninja', '-v']에 대한 이야기가 있었고, 관련 검색으로 ninja를 pip을 통해 설치하는 등의 시도를 했지만, 결국 위 -v를 --version으로 바꾸는 것이 핵심이었다.

결국 pcdet 모듈이 없다는 오류와 함께 실행이 되지 않고야 만다. 그래서 확인해보니, 제일 처음 install을 성공해야 하고, 그 다음에야 quick_mode를 진행할 수 있었다.
install을 하기 전에, requirements.txt를 pip으로 다운 받는 부분에서 error가 발생했다.
확인해보니 SharedArray를 설치할 수 없다는 내용이었는데, 또 사이트에 가서 확인해보니 numpy version이 1.8+여야 하고, 내가 설치한 numpy version은 1.23.0이었다.