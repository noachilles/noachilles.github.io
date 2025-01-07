---
title: (R) 시계열 분석 (2) - xts class를 활용한 시계열 객체 생성
date: 2024-09-25 16:30:00 +0900
categories: [Data, Analysis]
tags: [Data, DataAnalisys, 데이터분석, R, 시계열분석]
---

\*이 글은 Youtube 곽영기 교수님의 R을 활용한 시계열 분석 (2) 영상을 참고해 쓰였습니다.

## 개요

ts와 xts 모두 R 내에서 사용되는 클래스로, 필요에 따라 다르게 사용할 수 있습니다. xts 클래스가 편리한 함수를 다수 제공하기 때문에 오늘은 xts 클래스를 사용해 time series 객체를 생성하고 xts와 ts 클래스의 차이점을 정리해보겠습니다.

## ts class VS xts class

| ts 클래스                                    | xts 클래스                                                         |
| -------------------------------------------- | ------------------------------------------------------------------ |
| stats 패키지 내 클래스, **별도 설치 필요 X** | xts 패키지 내 클래스, **별도 설치 필요 O**                         |
| 관측값에 일정한 주기 부여                    | 임의 시간 간격 데이터(이를테면 영업일 기준) 제약 없이 생성 및 처리 |
| 시간 나타내는 인덱스 저장X                   | 관측값 + 인덱스 명시적 저장                                        |
| start, end, freq 메소드를 사용한 연산 기능   |                                                                    |

xts는 임의 시간 간격 데이터 분리가 가능하다는 장점을 추가로 다루고 이외 내용은 모두 강의와 동일한 코드를 사용했습니다.

## Code

아래 작성된 코드를 통해 xts 클래스의 특징을 직관적으로 이해해 보겠습니다.

### 예시 데이터 생성

앞에서 본 내용과 같이 xts 클래스는 시간(날짜) 데이터와 관측값을 명시적으로 정의할 수 있는 클래스입니다.  
다음과 같은 예시를 사용해 이 특징을 확인할 수 있습니다.

```R
# 날짜 데이터 정의
date <- c("2030-01-02", "2030-01-03", "2030-01-04", "2030-01-05",
          "2030-01-06", "2030-01-07", "2030-01-08", "2030-01-09")
# 시각 데이터 정의
time <- c("09:00:01", "10:05:13", "11:10:30", "12:00:01",
          "13:15:12", "14:20:01", "15:11:01", "16:20:03")
# 날짜 + 시각 데이터 datetime에 대입
datetime <- paste(date, time)
datetime

# 관측값 price 변수 생성
price <- c(200, 300, 420, 380, 490, 550, 600, 650)
# [datetime, price] 변수를 갖는 data frame, tsdata 생성
tsdata <- data.frame(datetime=datetime, price=price)
tsdata
```

이어서 xts 패키지를 설치하고 xts 클래스로 전환합니다.  
xts 객체를 생성할 때 인자 `order.by`는 시간을 나타내는 class를 요구합니다.

문자로 표현되어 있는 datetime을 날짜형으로 바꾸기 위해 `POSIXct` 함수를 사용했습니다.

```R
library(xts)
price.xts <- xts(x=tsdata$price,
                 order.by=as.POSIXct(tsdata$datetime,
                                     format="%Y-%m-%d %H:%M:%S"))
price.xts
class(price.xts) # output: "xts", "zoo"
```

위 예시로 xts class를 사용하면 class가 뭐라고 출력되는지 확인할 수 있었습니다.  
이어서 지난번 ts와 같이 직접 데이터에 적용해보겠습니다.

### data 불러오기, 객체 생성

우선, [이전 ts 게시글](https://www.noachilles.github.io)과 같은 링크에서 데이터를 불러옵니다.

```R
url <- "https://jse.amstat.org/datasets/utility.dat.txt"
utility <- read.table(url)
utility
```

이어서 시계열 객체를 다루기 위한 xts 클래스와 날짜 데이터를 다루기 위한 lubridate 패키지&클래스를 설치합니다.  
아래 코드 중 주석으로 1이라 표기한 이후 코드는 lubridate의 `my` 함수로 문자를 날짜데이터로 바꾸는 내용입니다. 다만 아래와 같이 함수를 적용하면 '년/월'로 존재하던 데이터에 **임의의 '일(Day)'이 추가**되기 때문에 조심해야 합니다.  
이를 해결하기 위해 `#2`의 yearmon 함수를 사용할 수 있습니다.

```R
library(xts)
install.packages("lubridate")
library(lubridate)
# 1
utility.xts <- xts(x=utility[c(3, 7)],
                   order.by=my(utility[[1]]))

class(utility.xts) # Output: > "xts", "zoo"

#2
utility.xts <- xts(x=utility[c(3, 7)],
                   order.by=as.yearmon(my(utility[[1]])))
```

\*\* (데이터 출력 결과를 추후에 추가하겠습니다.)

### 속성 이름 지정 및 시각화

위의 코드에서 ``utility[c(3, 7)]`, 즉 3번째와 7번째 열의 데이터만 가져와 utility.xts를 만들었습니다. 각 열에 대한 이름을 지정하는 코드입니다.

```R
names(utility.xts) <- c("Temperature", "Electricity")
```

시각화 코드는 결과만 확인하고 자세히 다루지 않겠습니다!

```R
plot(utility.xts, format.labels="%b-%Y",
     lty=c("dotted", "solid"), col=c("red", "blue"),
     lwd=2, main="Utility Consumption in Boston")
addLegend("topleft", lty=c("dotted", "solid"), col=c("red", "blue"),
          legend.names=c("Temperature", "Electricity"))
```

![image](https://github.com/user-attachments/assets/5019bc4b-40bd-42b2-a71a-4d082da7450d)

### xts 특징, 관측값 / 인덱스 구분

xts 클래스 객체의 특징인 데이터 / 시간 분리 파트입니다.

```R
# 데이터 부분만 추출
coredata(utility.xts)
# 시간 부분만 추출
index(utility.xts)
```

추가로, 특정 시점에 대한 인덱스 사용 역시 가능합니다(ts와 차이점)

```R
# 특정 시점의 데이터, index 사용 가능
utility.xts["199011"]
utility.xts["1990-11"]
utility.xts[3]

# 뒷 슬래시도 포함함
utility.xts["199103/199202"]
utility.xts["1991/199202"]
utility.xts["1991/1992"]
utility.xts[]
```

## 다음 내용

xts는 일부 구간에 대해 연산을 지정하고 값을 구할 수 있는 기능을 제공하고 있습니다. 강의가 아직 끝나진 않았지만 분량이 있는 편이라 오늘은 이만 마치고 다음 포스트에서 다뤄보겠습니다!
