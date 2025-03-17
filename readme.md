## 🏆 Dacon Stock Price Prediction - 1st Place Solution
![Python](https://img.shields.io/badge/Python-3.8-blue.svg)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Success-green)

## Introduction
- 국내 KOSPI200, KOSDAQ150 종목들에 대해 미래 5일간의 주식 종료 가격을 예측하는 task
- Public기간: 11월 1일 ~ 11월 5일 / Private기간: 11월 29일 ~ 12월 3일
- [Solution Link](https://dacon.io/competitions/official/235857/codeshare/4095?page=2&dtype=recent)

## Dataset
- 개별종목 주가 데이터
- 개별종목 재무 데이터
- 인덱스 지수 데이터
- 환율 데이터

## CV Strategy
- 전체 데이터 내 가장 최근일부터 일주일 간격으로 5-CV 검증데이터셋 구축

## Preprocessing & Feature Engineering
- 현재일로부터 5일 뒤 까지 Multi-Target 구성
- Multi-Target에 대해 smoothing 함수를 적용하여 일반화 성능 향상 도모
- 주가 데이터로부터 다양한 투지지표를 산출하여 추가 feature로 활용

## Modeling
- 단일 **선형회귀모델**만을 활용하여 추론했을 때 가장 높은 성능
- 단기 주가예측에 있어서는 복잡한 패턴을 반영할 필요성이 크지 않다는 사실을 보임

## Core Strategy
### 1. Target Smooting
- 급등락이 심한 종목에 대해서는 이상치로 간주하였고, 이를 훈련 샘플에서 제외시키는 대신 **가격(target)을 조정하는 연산**을 적용하였다. 이는 본 아키텍처만의 차별화된 핵심 전략이고 산식은 아래와 같다.
 
$Cutoff = A * (1 + B * T)$

*A : 주가 등락 조정 threshold, B : theshold multiplier, T : 최근 알려진 종가와 떨어진 거리 (단위: 일)*
- 최근 종가와 예측 종가의 변화율이 위에 의해 계산된 Cutoff 보다 높거나(혹은 낮으면) Cutoff와 같게 종가를 조정한다. **(내부 검증 결과 약 2.5% 성능 향상)**

### 2. Market Trend Factor
$Adj.Pred = Pred * (1 + ROUND(USDtoJPY, 2))$

- 시기마다 시장에 영향을 주는 주요 macro indicator들이 많은데, 실시간으로 거래가 되는 future자산을 활용하면 성능 개선에 도움을 줄 수 있을 것이라고 생각하여 추가하였다.
- 내부 연구 결과 시장에 영향을 주는 핵심 macro indicator는 **USD/JPY**로 보았고, 이를 이용해 예측치를 조정하는 후처리 연산을 추가하였다. 엔화는 널리 알려진 안전자산으로 일반적으로 엔화가치가 상승하면 안전자산(Ex.채권) 선호심리가 증가하고 반대면 위험자산(Ex.주식) 선호심리가 증가한다.
- USDtoJPY의 경우 당일 변화율이며, **반올림 연산을 통해 소수 3째자리 이하의 변동에 대해서는 고려하지 않도록 설계하였다.**
