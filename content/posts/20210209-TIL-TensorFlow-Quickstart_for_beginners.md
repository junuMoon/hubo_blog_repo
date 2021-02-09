---
title: "20210209 TIE Tensorflow Model"
date: 2021-02-09T20:12:34+09:00
categories: ["TIL"]
tags: ["Tensorflow", "DeepLearning"]
---

# TensorFlow: Quickstart for beginners

### Flatten

다차원 벡터를 일차원으로 쭉 펴준다.

### Dense

Input과 Ouput 레이어를 연결한다.

### Dropout

무작위로 뉴론을 드랍하여 모델의 overfitting을 막는다.

### Softmax

입력 벡터의 원소를 0과 1사이의 값으로 변환하여 리턴한다.

### Loss

모델의 예측값과 실제 답의 차이. 

### Sparse Categorical Crossentropy

Loss function 중 하나

### model.compile / model.fit / model.evaluate

compile 메소드: 손실함수, 옵티마이저, 측정지표 등의 모델 구성
fit 메소드: 손실함수의 값을 최소로 하는 모델 훈련
evalute 메소드: 모델의 성능을 측정