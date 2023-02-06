---
title: "[공부] VoxelNet Implementation"

categories:
  - study
tags:
  - [study]

toc: true
toc_sticky: true

date: 2023-02-06
last_modified_at: 2023-02-06

published: false
---

** 실제로 실험에 사용할지는 모르겠고, 그냥 어떻게 진행이 되는지 알아보기 위해서 VoxelNet을 구현한 코드로 돌려보기로 하는데, 진행이 매끄럽게 되지 않는다. **

# 발생한 문제

## fatal error : numpy/arrayboject.h : No such file or directory

* 말 그대로 위 이름으로 존재하는 파일이 없다는 뜻. 근데 내가 import numpy 하고 들어가면 해당 파일이 있는 폴더에 들어가진다. 그래서 왜인지 잘 모르겠다. 바꿔봐야하나...?
* fatal error 문제는 위치 경로가 잘못 설정되어 있는 경우라서 원래
<code>import numpy 
numpy.get_include()</code> 
하면 나오는 경로를 usr/local/ 아래로 바꾸는 것이 핵심인 듯 하다. 다만
<code>ln -s /usr/local/lib/python2.7/dist-packages/numpy/core/include/numpy /usr/local/include/numpy</code>
이런 코드를 추천 받았지만, Windows에 걸맞는 코드가 아닌 것 같아서 Windows에서 경로 설정하는 방법을 찾아보도록 한다.

## Windows에서 import 경로 설정하기

<a>https://m.blog.naver.com/yoojung428/222590456883</a>
여기를 참고하면 
<code>sys.path.append('경로')</code>
이런 코드로 import하라고 한다. 그런데 우리는 Cython을 사용하므로 Cython에서 include 할 때 경로 설정하는 방법을 알아내야 한다는 것을 순간 깨닫는다.

경로를 모두 절대 경로로 바꿔 해결하고, 
<code>python setup.py build_ext --inplace</code>
를 실행해 파일이 하나 더 만들어진 것을 확인하였음. box_overlaps.cp310... 이런 파일이 생겼음
build 폴더 역시 변화되었다.

## GCC 사용하기
<img 1>
그림과 같이 g++는 사용할 수 없다고 뜨는 것을 확인하였다. GCC가 무엇인지 간단하게 검색을 통해 알고 넘어갈 필요가 있다고 판단하였다.

윈도우의 개발환경은 Visual Studio의 툴셋을 기반으로 한다.
cross-compiler 기반의 어플리케이션을 개발하거나 관심을 가지면서 윈도우에서도 GCC 컴파일러를 사용하기 위해 환경 구측을 했다.

따라서 GCC와 G++은 모두 컴파일러의 일종이며, GCC는 C 컴파일러이고 G++는 C++의 컴파일러이다.

<a>https://www.msys2.org/</a>

해당 링크에서 G++를 설치하고 설정할 수 있다고 해서 따라가보기로 했다.
<img 2>
compile하는 데 몇 가지 tool을 추가로 설치하고 싶다면 따라하라고 해서 이까지 따라한다.

conda를 사용중이라는 걸 까먹고 또 local에 설치해버렸다 ㅜㅜ 
<code>conda install -c conda-forge m2w64-gcc</code>
코드로 다시 설치한다.

참고: <a>https://richardworld.tistory.com/entry/MinGW-%EC%9C%88%EB%8F%84%EC%9A%B0%EC%97%90%EC%84%9C-g-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0</a>

## Boost Library

드디어 g++ 설치를 무사히 마치고 실행하니 g++로 오류가 나지 않는다. 하지만 include하는 모듈에 대해 No such file error가 발생하는데, 찾아보니 C++ Library인 Boost Library이다.
별개로 설치하고 앞전의 모듈들처럼 절대 경로를 통해 include 할 수 있도록 한다.

그런데 이런식으로 경로 설정을 하는 것은 시간적으로 너무 손해보는 일이고...
<img 4>
이렇게 절대 경로로 모두 직접 손으로 바꾸는 일은 체력적으로도 안 될 것 같아서 그만, 포기했다.
