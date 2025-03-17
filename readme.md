## 🏆 SmartFarmKorea Data Analysis Competition - 1st Place Solution
![Python](https://img.shields.io/badge/Python-3.8-blue.svg)
![Machine Learning](https://img.shields.io/badge/Machine%20Learning-Success-green)

## Introduction
- 스마트팜코리아 데이터마트 내 데이터를 활용하여 농업 분야에 활용할 수 있는 분석 인사이트 제안 혹은 AI 기반 솔루션 개발
- 분석 인사이트 주제: 양돈농가 번식 데이터
- AI 기반 솔루션 주제: 양돈기침 음성 데이터

## Dataset
- 양돈농가 번식 데이터
- 양돈기침 음성 데이터

## CV Strategy
- 기침유형 별 층화추출 샘플링
- CV 데이터셋 비율: Train(81%) / Valid(9%) / Public(5%) / Private(5%)

## Preprocessing & Feature Engineering
- Extract Audio Features
 1. ZCR (Zero Crossing Rate)
 2. MFCC (dim : 32)
 3. Chroma Frequencies (dim : 16)
 4. RMS (Root Mean Square)
- Feature Engineering
 1. Transform ZCR vector to the scalar mean of the number of ‘True’ in ZCR vector
 2. Add the mean & standard deviation on MFCC
 3. Add the mean & standard deviation Chroma Frequencies
 4. Add the mean, standard deviation, max, min, min-max range, min-max pct range on RMS

## Modeling
- ElasticNet, RandomFrorest, XGBoost, KNN, MLP 모델들의 추론값을 가중평균하여 앙상블
![image](https://github.com/user-attachments/assets/15bc1db0-fa99-4248-b78b-94b3963c7dc0)

## Insights
- MFCC feature는 두 기침 유형을 분류하는데 유의미한 feature임을 보임
![Untitled](https://github.com/user-attachments/assets/92a96703-cbfc-472d-9fc2-57781bd52d87)

