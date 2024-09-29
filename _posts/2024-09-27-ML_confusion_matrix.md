---
title: (ML)Confusion Matrix 이해하기
date: 2024-09-27 23:30:00 +0900
categories: [ML, Eval]
tags: [ML, Confusion Matrix, Evaluation, DL, AI]
---

## Confusion Matrix란,

혼동행렬이라 하며, test data에 대한 머신러닝 모델의 수행 결과를 요약한 것입니다.  
즉, 해당 모델이 몇 개의 데이터를 Positive로, 몇 개의 데이터를 Negative로 예측했는지와 그 중 각각 몇 개의 데이터가 실제 Positive 혹은 Negative값을 지니는지 나타낸 것입니다.

Confusion Matrix는 다음과 같이 나타낼 수 있습니다. 각 값을 이제부터 알아보겠습니다.

### Confusion Matrix

<table>
    <tr>
        <td></td>
        <td></td>
        <td colspan="2">Predict</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>Positive</td>
        <td>Negative</td>
    </tr>
    <tr>
        <td rowspan="3">Actual
        </td>
        <td>Positive</td>
        <td>TP</td>
        <td>FN</td>
    </tr>
    <tr>
        <td>Negative</td>
        <td>FP</td>
        <td>TN</td>
    </tr>
</table>

Actual은 실제값, Predict는 예측 결과값을 나타냅니다.  
익숙한 '바이러스 감염 진단 결과: 양성(Positive)/음성(Negative)'으로 예를 들어보겠습니다.

- Actual Positive: **실제로** 바이러스에 감염됨
- Actual Negative: **실제로** 감염되지 않음
- Prediction Positive: 감염되었다고 **예측함**
- Prediction Negative: 감염되지 않았다고 **예측함**

이후 값에 맞춰 위의 표를 챙겨보면 Confusion Matrix를 다음과 같이 이해할 수 있습니다.

<table>
    <tr>
        <td></td>
        <td></td>
        <td colspan="2">Predict</td>
    </tr>
    <tr>
        <td></td>
        <td></td>
        <td>Positive</td>
        <td>Negative</td>
    </tr>
    <tr>
        <td rowspan="3">Actual
        </td>
        <td>Positive</td>
        <td>TP</td>
        <td>FN</td>
    </tr>
    <tr>
        <td>Negative</td>
        <td>FP</td>
        <td>TN</td>
    </tr>
</table>

### TP/TN/FP/FN 구분

- <span style="color:#0088ff">TP(True Positive)</span>: 감염자를 감염자로 예측함
- <span style="color:#0088ff">TN(True Negative)</span>: 비감염자를 비감염자로 예측함
- <span style="color: #f08080">FP(False Positive)</span>: 비감염자를 감염자로 예측함
- <span style="color:#f08080">FN(False Negative)</span>: 감염자를 비감염자로 예측함

이때, T/F는 정답/오답으로, P/N은 모델 예측값의 P/N여부로 생각하면 쉽게 이해할 수 있습니다.

## 분류 모델 평가 지표

위의 값들로 Classifier 모델 평가 지표인 Accuracy, Precision, Recall, F1-Score를 구할 수 있습니다.

### Accuracy

Accuracy(정확도)는 모델의 수행능력을 평가할 때 사용됩니다. 모든 인자 중 정답 인자의 비율을 나타냅니다.

$$Accuracy =\frac{TP+TN}{TP+TN+FP+FN}$$

### Precision

Precision(정밀도)은 model이 Positive(양성)으로 예측한 값들의 정확도를 나타냅니다. 모델이 Positive로 예측한 모든 값들 중 정답 비율을 나타냅니다.

$$Precision = \frac{TP}{TP+FP}$$

### Recall

Recall(재현율)은 모델이 실제 Positive 클래스를 얼마나 잘 잡아내는지 나타내는 지표입니다. 분류 모델의 효과성을 평가하는 데 사용됩니다. 실제값이 positive인 경우 중 모델이 Positive로 판정한 비율을 나타냅니다.

$$Recall = \frac{TP}{TP+FN}$$

### F1-Score

F1-Score은 분류 모델의 전반적인 수행능력을 평가하기 위해 사용됩니다. Precision과 Recall의 조화평균(harmonic mean)으로 이루어져있습니다.

F1-score는 $0 ~ 1$ 사이 값이며 1에 가까울 수록 분류 성능이 뛰어납니다.

$$F1-Score = 2\times \frac{Precision\times Recall}{Precision+Recall}$$

Precision과 Recall은 trade-off 관계이기 때문에, Precision이 올라가면 Recall이 낮아지고, Recall이 높아지면 Precision이 낮아집니다. 
Decision threshold를 통해 trade-off 관계를 조절할 수 있습니다. 