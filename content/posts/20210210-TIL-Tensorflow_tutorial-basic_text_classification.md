---
title: "[20210210_TIL] Tensorflow tutorial: Basic text classification"
date: 2021-02-10T16:04:19+09:00
categories: ["TIL"]
tags: ["Tensorflow", "DeepLearning"]
draft: true
---

### seed

난수 생성을 위한 기본값. 시드가 바뀌면 새로운 난수 셋이 만들어진다. 시드가 같다면 동일한 난수 셋이 만들어진다.

### 데이터셋 종류

1. train set: 모델을 학습하기 위한 데이터셋
2. validation set: 학습이 완료된 모델을 검증하기 위한 데이터셋
3. test set: 학습과 검증이 완료된 모델의 성능을 평가하기 위한 데이터셋

#### validation set과 test set의 차이

test set은 학습이 완료된 모델의 성능을 평가하기 위하여 사용한다. validation set은 학습된 여러 모델 중 가장 좋은 모델을 선택하기 위해 사용한다.

### Standardization

이모티콘, 태그 등의 불필요한 문자열을 제거한다.

### Tokenization

문자열을 문장 또는 단어 등 필요한 단위의 토큰으로 나눈다.

### Vectorizaion

토큰을 신경망이 이해할 수 있는 수집합으로 만든다.

### tf.keras.layers.experimental.preprocessing.TextVectorization

1. 각 샘플 표준화(lowercase + 구두점 제거)
2. 각 샘플을 부분으로 나눈다(문장, 단어 등)
3. 나눈 부분을 조합하여 토큰으로 만든다(ngram 등)
4. 토큰 인덱스를 만든다(각 토큰에 유일한 정수값을 매긴다)
5. 벡터와 같은 수집합으로 만든다.

#### attributes

- max_token: 토큰 모음집인 vocabulary의 사이즈. 코퍼스의 단어가 max_token 상수보다 높을 시 1번 index의 UNK(unknown)로 등록된다.
- output_sequence_length: 출력되는 시퀀스(리스트, 배열 등)의 길이. 샘플의 문장의 길이가 output_sequence_length 상수보다 클 시 여기서 짤린다.

