Dacon AI프렌즈 시즌1 온도 추정 경진대회
======================

## 기상청 공공데이터를 활용한 온도추정 대회
[대회안내](https://dacon.io/competitions/official/235584/overview/)

### 목표
센서로 관심 지역의 온도를 단기간 측정  
센서 데이터와 기상청의 관측 데이터와의 상관관계 모델을 만들기  
생성된 모델을 통해 관심 지역의 온도를 추정하여 제공하는 것  

### 데이터 설명
- X00-X35
  - 관심지역 근처의 5군데 기상청 데이터 
  - 기온, 현지기압, 풍속, 일일 누적강수량, 해면기압, 일일 누적일사량, 습도, 풍향
- Y00-Y17, Y18
  - 실내외 19곳의 센터 데이터(온도)
  - Y18: 예측 대상
- 10분 단위

||30일(train)|3일(train)|80일(test)|
|-|-|-|-|
|X00-X35(기상청 데이터)|O|O|O|
|Y00-Y17(센서 측정 온도)|O|X|X|
|Y18(관심지역 센서 측정 온도)|X|O|예측대상|

### EDA - eda.ipynb
[코드보기](https://nbviewer.jupyter.org/github/YunjinPark/dacon_temperature_forecasts/blob/master/eda.ipynb)
### MODEL - dacon_temperature_model.ipynb
[코드보기](https://nbviewer.jupyter.org/github/YunjinPark/dacon_temperature_forecasts/blob/master/dacon_temperature_model.ipynb)
1. LSTM 
- dacon의 baseline 코드 활용. 
- 30일 동안의 기상청 데이터로 관심지역과 유사한 지역들의 평균 온도를 예측하는 LSTM 모델 학습. 
- 3일 동안의 기상청 데이터로 관심지역의 온도를 예측하도록 모델의 일부분만 튜닝.
- 80일 동안의 기상청 데이터로 관심지역의 온도 예측. 
2. LightGBM
- 30일 동안의 기상청 데이터로 관심지역과 유사한 지역들의 평균 온도를 예측하는 모델 학습. 
- 위의 모델을 initail model로 하여 3일 동안의 기상청 데이터로 관심지역의 온도를 예측하도록 모델 학습.
- 80일 동안의 기상청 데이터로 관심지역의 온도 예측. 
3. XGBoost
- 30일 동안의 기상청 데이터로 관심지역과 유사한 지역들의 평균 온도를 예측하는 모델 학습. 3일간의 데이터를 validation data로 활용. 
- 80일 동안의 기상청 데이터로 관심지역의 온도 예측. 
=> 3가지 예측값을 평균내어 관심지역의 온도를 예측함. 
