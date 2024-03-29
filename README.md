# 머신러닝을 이용한 기본 트레이딩 전략 향상

이용한 데이터: 코스닥150 분봉

### 트레이딩 전략:

매도세가 약해지고 매수세가 강하게 이어졌을 때를 포착하여 매수를 진행하겠다는 것을 목적으로 하였다.
기본적으로 롱 타이밍보다 숏 타이밍을 잡기 힘들고, 거래 비용이 높기에 long-only 전략을 택하였다. 정확한 타이밍을 구하는 것 대신 익절/손절 임계치를 설정하여 매도 타이밍을 잡았다. 
사전에 정의한 signal f값을 이용하여, 임계치 보다 낮은 구간을 매도세가 강하다고 보았고 다시 원점으로 회복 시 매수세가 강하게 일어났을 시에 매수시그널을 생성하도록 하였다. 

횡보장에서 진입을 막기 위해 parkinson volatility를 이용하여 일정 범위 이상의 'H/L'에 대한 평균이 나왔을 때 매수 시그널을 생성하도록 하여 변동성이 낮은 구간에서의 진입 시그널을 제거했다. 

### triple-barrier method:
데이터에 triple barrier 방식을 통해 2개의 수평 배리어, 1개의 수직 배리어를 형성하여 박스권을 만들고, 그 안에서의 변동성을 확인해주려 하였다. 우선, 시작점으로부터 일정 범위 이상의 누적 변동성이 나타났을 때 그 시점에 수직 배리어를 생성해주었다. 그리고 박스권 내에서 수직 배리어를 가장 먼저 터치하면 그대로, 수평 배리어를 먼저 터치하면 예상치 못한 박스권 밖의 추세가 있다고 판단하여 다시 그 시점으로부터 박스권을 설정하여 샘플링을 진행하였다.

### meta-labeling method:
기본 전략에서 생성한 진입/청산 시그널과 triple barrier를 통해 얻은 구간을 이용하여 기본전략의 시그널이 실제로 수익률을 가져다줄 수 있는 시그널인지 이차 검증하는 방식이다. 따라서 실제 수익률을 계산했을 때 음수가 나오면 진입하면 안되는 가짜 시그널이라고 보아 0, 양수가 나오면 진입을 했는데 진짜 수익을 얻는 경우이므로 진짜 시그널이라고 판단하여 1이라는 label을 갖도록 하였다.

### ROC-Curve:


- Sensitivity (True Positive(TPR), Recall) : The rate of positive in prediction in terms of positive in actual $\Rightarrow \frac{TP}{TP+FN}$
- Specificity (True Negative(TNR)) :  The rate of negative in prediction in terms of negative in actual.
- False positive rate(FPR) = 1 - specificity $\Rightarrow \frac{FP}{TP+TN}$
- Accuracy : The rate of well-classified data in total data sets $\Rightarrow \frac{TP+TN}{TP+FP+TN+FN}$
- Precision : The rate of positive in actual in datas predicted to positive $\Rightarrow \frac{TP}{TP+FP}$

### random forest:
마지막으로 앞서 얻은 meta label과 OHLC를 이용하여 meta-label을 예측하는 classification task을 진행하였다. false positive 비율을 줄이는 것을 목적으로 진행하였으며 결과 roc curve는 다음과 같다.
<p align="center"> 
 <img src= "https://github.com/2020147544/Advances_in_Financial_Engineering/assets/80660498/4c700f6b-fe22-42e2-9679-2195a51642a9">
</p>

