---
title: (R) 시계열 분석 (3) - xts class의 인덱스 사용(주기별 통계량 구하기, 기간이동연산 Rollapply)
date: 2024-09-28 16:30:00 +0900
categories: [Data, Analysis]
tags: [Data, DataAnalisys, 데이터분석, R, 시계열분석, Rollapply, 주기별통계량, 기간이동연산]
---

\*이 글은 Youtube 곽영기 교수님의 R을 활용한 시계열 분석 (2) 영상을 참고해 쓰였습니다.

## 지난 내용 및 개요

xts 클래스를 사용해 시계열 데이터를 표현하는 방법을 알아보고 시각화해보았습니다. xts는 관측값과 날짜 및 시간을 구분할 수 있다는 점이 가장 큰 특징이며, 이외에도 활용할 수 있는 다양한 기능과 함수가 있습니다.

오늘은 xts 함수를 사용해 시계열 데이터를 분할하고 이에 대한 통계량을 구해보겠습니다.

## CODE

### xts에서 인덱스(날짜) 사용

xts 객체는 아래와 같이 날짜 인덱스를 사용할 수 있습니다.  
특히 코드의 4번째 줄 코드는 1991년도의 Electricity 열에 대한 값들을 불러오는 코드입니다.

```R
ym <- c("199009", "199012", "199103")
utility.xts[ym]

utility.xts["1991", "Electricity"]
```

OUTPUT:

```
        Temperature
1990-09          62
1990-12          37
1991-03          39
        Electricity
1990-09         432
1990-12         408
1991-03         343
```

### 시계열 데이터 주기별 분석과 endpoints

시계열 데이터를 일별, 주별, 월별 등으로 분할해 통계량 분석이 가능합니다.  
예시를 들기 위해 연도가 서로 달라지는 시점의 인덱스 포인트들을 저장해주겠습니다. `endpoints()`가 해당 기능을 수행하는 함수입니다.  
이후 이 endpoints를 바탕으로 각종 통계량을 계산합니다. 해당 기능은 `period.apply()` 함수를 사용합니다.

```R
period.apply(data, INDEX=endpoints, FUN=func)
```

`apply` 함수는 아래 5번째줄처럼 endpoints를 지정하지 않고도 사용할 수 있습니다.

```R
ep <- endpoints(utility.xts, on="years")

period.apply(utility.xts, INDEX=ep, FUN=mean)

apply.yearly(utility.xts, FUN=mean)
# + apply.daily(), apply.monthly(), apply.quarterly(), apply.weekly()
```

OUTPUT: 2개 함수 사용 결과가 동일합니다.

```
        Temperature Electricity
1990-12    50.00000    412.0000
1991-12    51.41667    450.4167
1992-12    48.00000    471.3333
1993-12    49.16667    455.1667
1994-12    49.00000    454.1667
1995-12    48.25000    510.5000
1996-12    50.83333    616.8333
1997-05    40.80000    803.4000
```

### Rollapply / Rolling Window 기간 이동 계산

기간 이동 계산이란, 시계열 데이터에 대해 특정 크기(기간)의 슬라이드를 통한 슬라이딩 기법으로 반복 연산을 수행하는 것을 의미합니다.
인덱스를 사용해 일부 구간에 대한 통계량 연산을 슬라이딩 기법으로 반복할 수 있습니다. 예를들어, (1990-09 ~ 1991-08)에 대한 전기사용량을 구해보겠습니다.

아래 코드에서 각 인자의 의미를 적어보겠습니다.  
`Rutility.xts["1990-9/1991-9", "Electricity"]`: 1990년 9월부터 1991년 8월까지 Electricity 열의 데이터에 대해
`width=2`: width는 슬라이드 윈도우의 크기를 의미합니다. 여기서는 2이므로 2개의 데이터에 대해 함수 실행이 이뤄집니다.  
`FUN=mean`: FUN은 통계 연산 종류를 지정합니다. 아래 예시에서는 mean 즉 평균을 구합니다.

따라서, 아래 함수는 **utility.xts 데이터 중 1990년 9월부터 1991년 8월까지의 전기사용량에 대해 2개월분의 평균을 반복적으로 구하는 함수**입니다.

```R
roll1 <- rollapply(utility.xts["1990-9/1991-8", "Electricity"],
                    width=2, FUN=mean)
head(roll1)
```

OUTPUT: head로 출력했기 때문에 앞의 6개 data만 출력됩니다.

```R
         Electricity
Sep 1990          NA
Oct 1990       450.5
Nov 1990       404.0
Dec 1990       373.5
Jan 1991       533.0
Feb 1991       642.5
```

위의 출력 결과에서 1990년 9월의 측정값이 NA로 출력되는 것은, 1990년 9월이 시작 인덱스이기 때문에 2개의 데이터의 평균을 구할 수 없는 시점이기 때문입니다.

```R
roll2 <- rollapply(utility.xts["1990-9/1991-8", "Electricity"],
                   width=3, FUN=sum)
head(roll2)
```

OUTPUT

```R
         Electricity
Sep 1990          NA
Oct 1990          NA
Nov 1990        1240
Dec 1990        1216
Jan 1991        1405
Feb 1991        1693
```

마찬가지로 width=3으로 설정한 경우에는 위와 같이 앞의 두 개 출력값이 NA로 표기됩니다.

```R
tail(roll2)
```

OUTPUT

```R
         Electricity
Mar 1991        1628
Apr 1991        1369
May 1991        1245
Jun 1991        1342
Jul 1991        1173
Aug 1991        1044
```

위처럼 tail을 출력할 경우엔 NA가 발생하지 않습니다.

아래는 n번쨰 행마다 출력하는 코드입니다. `rollapply` 함수를 동일하게 사용하나 `by=3`의 인자가 추가되었습니다.

```R
roll3 <- rollapply(utility.xts["1990-9/1991-8", "Electricity"],
                   width=3, FUN=sum, by=3)
head(roll3)
```

OUTPUT

```R
         Electricity
Sep 1990          NA
Oct 1990          NA
Nov 1990        1240
Dec 1990          NA
Jan 1991          NA
Feb 1991        1693
```

## 마무리 및 다음 내용

오늘 포스팅에서는 xts의 인덱스 저장 기능을 활용할 수 있는 다양한 함수를 다뤄보았습니다.
지난번 활용한 데이터에 Rollapply 함수를 적용해 기간 이동 연산을 수행하고, 시계열 데이터를 주기별로 분석할 수 있었습니다.  

강의 영상 내용을 따라 다음 포스트에서는 계절 데이터/비계절 데이터로 분류하고 데이터를 성분 분해 해보겠습니다.
