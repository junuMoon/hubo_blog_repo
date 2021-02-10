---
title: "[20210209_TIL] Tensorflow Tutorial"
date: 2021-02-09T20:12:34+09:00
categories: ["TIL"]
tags: ["Tensorflow", "DeepLearning"]
---

## Layers

심층신경망 모델은 여러 개의 층(layer)로 이뤄진다.

### Flatten

다차원 벡터를 일차원으로 쭉 펴준다. 이 레이어에는 학습시킬 파라미터가 없다. 단지 데이터를 반영할 뿐이다.

### Dense

Input과 Output 레이어를 연결한다. densely connected(fully connected)는 깊게 얽힌, 다른 단어로 '심층'을 뜻한다. keras.layers.Dense 메소드는 노드 개수를 파라미터로 갖는다.

### Dropout

모델이 훈련 데이터에 지나치게 맞춰져 오히려 테스트 데이터에서 정확도가 떨어지는 현상인 과적합(overfitting)을 막기 위한 옵션. 파라미터로 받은 rate 만큼 무작위로 뉴론을 드랍하여 모델의 overfitting을 막는다.

- [Dropout: A Simple Way to Prevent Neural Networks from Overfitting](http://www.jmlr.org/papers/volume15/srivastava14a/srivastava14a.pdf)

## Compile the model

모델을 훈련시키기 전에 다음과 같은 세팅이 필요하다.

```python
model.compile(optimizer='adam',
			 loss=tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True),
			 metrics=['accuracy']
)
```

### Optimizer

모델의 손실 함수값을 최소할 수 있도록 만드는 학습 전략. adam, SDG 등이 있다.

### Loss function

모델의 예측값과 실제 답의 차이를 계산하여 모델이 잘 훈련되고 있는지를 판단한다. 이 값을 최소화하여 모델을 올바른 방향으로 지도할 수 있다. Sparse Categorical Crossentropy 등이 있다.

### Metrics

모델의 성능을 평가하는 척도. 정확도(Accuracy), 정밀도(Precision), F1-score 등이 있다.

### Logit

log+odds. 로짓(logit)은 0~1의 확률값(odds)에 로그를 씌워 -∞~∞의 값으로 만든다. 이를 sigmoid나 softmax 등의 활성화 함수에 적용하면 0~1사이의 확률값을 얻을 수 있다.

모델의 raw output에 활성화함수를 적용하면 확률값을 얻을 수 있다.

```python
model.predict(test_images)[0][0]` = -17.750181
tf.keras.Sequential([model, tf.keras.layers.Softmax()]).predict(test_images)[0][0] = 0.000015
```

> Q.  Sparse Categrocial Cross Entropy 손실 함수에서 logits=True가 의미하는 바는?
> ```python
> tf.keras.losses.SparseCategoricalCrossentropy(from_logits=True)
> ```
>
> A. from_logits = True 속성은 모델에 의해 생성된 출력 값이 정규화(normalization)되지 않았음을 손실 함수에 알려줍니다. 확률 분포를 생성하기 위해 softmax 함수가 모델에 적용되지 않았다는 것입니다. 
>
> 따라서 이 경우 softmax 함수는 손실 함수에 의해 출력 값에 자동으로 적용됩니다. 이는 손실함수에 from_logits = False 속성을 주고 마지막 층(layer)에 softmax 활성화 함수를 사용하는 것과 차이가 없습니다. 그러나 어떤 경우에는 모델 학습 중 수치 안정성에 도움이 될 수 있습니다.
>
> ref: https://datascience.stackexchange.com/questions/73093/what-does-from-logits-true-do-in-sparsecategoricalcrossentropy-loss-function

### model.compile / model.fit / model.evaluate / model.predict

- compile 메소드: 손실함수, 옵티마이저, 측도 등의 모델 세팅
- fit 메소드: 손실함수의 최소값을 목표로 모델 훈련
- evalute 메소드: 모델의 성능을 측정
- predict 메소드: 모델의 예측값 출력

> ```bash
> Epoch 1/10
> 1875/1875 [==============================] - 5s 2ms/step - loss: 0.6305 - accuracy: 0.7775
> ```
>
> Q. MNIST 패션 데이터의 훈련 세트에 60000개의 이미지가 있는데 모델이 1875개씩 학습하는 이유는 무엇입니까?
>
> A. 모델의 batch 크기가 32이고, 1875번 배치 학습됐다는 뜻입니다. 1875*32 = 60000
>
> ---
> model.fit 메소드의 파라미터인 batch_size의 default 값은 32이다.
>
> ref: https://stackoverflow.com/questions/62186784/why-the-model-is-training-on-only-1875-training-set-images-if-there-are-60000-im
> https://keras.rstudio.com/reference/fit.html

