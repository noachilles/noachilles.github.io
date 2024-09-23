---
title: (R) 시계열 분석 (1) - 시계열 데이터 객체 생성
date: 2024-09-23 23:30:00 +0900
categories: [Data, Analysis]
tags: [Data, DataAnalisys, 데이터분석, R, 시계열분석]
image: /posts/2024-09-23-R-analysis-timeseries-1/thumbnail.png
---

\*이 글은 Youtube 곽영기 교수님의 R을 활용한 시계열 분석 (1) 영상을 참고해 쓰였습니다.

## 시계열 분석

### 시계열 분석이란

<span style="color: #f08080">시간 흐름</span>에 따라 <span style="color: #f08080">일정한 간격</span>으로 사건을 관찰하고, 과거의 데이터 분석을 통해 미래 관측값을 예측하는 데이터 분석 방법

### 변동 요인과 분석 과정

**시계열 데이터 값 변동 요인**

1.  trend component
2.  seasonal component
3.  irregular component

**시계열 분석 과정**  
 <span style="color: #f08080">1. 데이터 생성(시계열 객체/관측값, 시작 시점, 주기 등)</span>  
 2. EDA  
 3. 미래 관측값에 대한 예측(exponential modeling 혹은 ARIMA를 주로 이용)

위의 세 단계 중 이 글에서 다룰 내용은 분홍색 글씨로 표시간 1번, 데이터 생성 내용입니다.  
R에서 시계열 데이터를 생성하는 툴은 ① stats 패키지의 ts class / ② xts 패키지의 xts class로 구분할 수 있고, 오늘은 특히 기본적인 ts 클래스를 사용해 데이터 객체를 만들어 보겠습니다.

## 데이터 불러오기

우선 예시 데이터를 불러와야 하는데, 이는 다음 [링크](https://jse.amstat.org/jse_data_archive.htm)를 클릭해 접속한 뒤 사진의 데이터를 검색해 <span style="color:#0088ff">utility.dat.txt</span>를 사용합니다. <span style="color:#0088ff">utility.txt</span>는 데이터의 정보를 담고 있습니다.

<div align="center"><img src="https://github.com/user-attachments/assets/eda403eb-8f96-4c76-84c5-b9dbbd345848" width=90%/></div>  
<div align="center">fig 1. JSE data</div>  
  
  
아래 코드에서는 위 데이터를 불러온 뒤 출력하는 내용입니다.

```R
url <- "https://jse.amstat.org/datasets/utility.dat.txt"
utility <- read.table(url)
utility
```

utility.ts 변수를 생성해 ts(time series) 대입합니다. 각 메소드는 다음과 같이 사용합니다.  
**data=utility[7]** 은 전체 데이터의 7열만 사용한다는 뜻이며,  
**start=c(1990, 9)** 는 1990년(시간 단위)의 9월(일련번호)의 데이터부터 수집한다는 뜻입니다.  
동일한 방식으로 end(종료시점)을 지정할 수 있습니다.  
**frequence** 는 시간 단위당 데이터의 개수를 표현하는 것으로, 현재 시간 단위는 연단위이고, 매달 시계열 값을 산출하기 위해 12를 입력합니다.

```R
utility.ts <- ts(data=utility[7], start=c(1990, 9),
                 frequency=12)
```

이후 class를 출력해 "ts" 객체임을 확인할 수 있습니다.

```R
class(utility.ts)
> "ts"
```

이후 plot 함수를 사용해 데이터 관측값을 그래프로 확인해봅니다.

```R
plot(1:40)

plot(utility.ts)
plot(utility.ts, col="salmon", lwd=2,
     xlab="Year", ylab="Electricity Usage",
     main="Electricity Usage Trend of Boston Area")
```

<div align="center"><img src="https://github.com/user-attachments/assets/02ba1c5e-3bba-40d4-abfd-2ce3f9391ee0" width=90%/></div>  
<div align="center">fig 2. plot example(Year-Electricity Usage)</div>

이외에 ts class 내 함수들은 다음과 같이 사용할 수 있습니다.

```R
# 시작 관측값 시각 추출
start(utility.ts)
# 종료 관측값 시각 추출
end(utility.ts)
# 시간 단위당 데이터 개수 확인
frequency(utility.ts)
# 관측값 간 주기 = 1/12
1/12
deltat(utility.ts)
# 각 관측값이 추출되는 시점을 동일한 간격의 시계열 값으로 반환
# time은 x축으로 사용됨
# 시작시점이 어디인지는 상관 X, 다만 시간단위에 대한 간격을 나타냄
# = 1월이 기준(0.000) 2월은 deltat(0.083), 3월은 2*deltat(0.167)...
time(utility.ts)
# 주기의 일련번호 반환 = 여기선 월
cycle(utility.ts)
# 시계열 함수 subset 생성
window(utility.ts, start=c(1991, 1), end=c(1992, 6))
window(utility.ts, start=c(1991, 1), frequency=1)
```

다음 강의인 xts를 정리하면서 ts와 기능 및 특징을 비교해 정리해보겠습니다.
