---
title: "20210209 TIE Tensorflow Model"
date: 2021-02-09T20:12:34+09:00
categories: ["TIE"]
tags: ["tensorflow", "numpy"]
---

# [Tensorflow model.evaluate()] Numpy class type value error

```bash
---------------------------------------------------------------------------
ValueError                                Traceback (most recent call last)
<ipython-input-99-7a9586bd062a> in <module>()
----> 1 model.evaluate(x_test, y_test, verbose=2)
/usr/local/lib/python3.6/dist-packages/tensorflow/python/keras/engine/data_adapter.py in select_data_adapter(x, y)
    962         "Failed to find data adapter that can handle "
    963         "input: {}, {}".format(
--> 964             _type_name(x), _type_name(y)))
    965   elif len(adapter_cls) > 1:
    966     raise RuntimeError(
ValueError: Failed to find data adapter that can handle input: <class 'numpy.ndarray'>, (<class 'list'> containing values of types {'(<class \'list\'> containing values of types {"<class \'float\'>"})'})
```

### 오류 개요

1. 오류 종류: ValueError
2. 언어: Python
3. 라이브러리: Tensorflow, Numpy
4. 오류 발생 라인: `model.evaluate(x_test, y_test, verbose=2)`
5. 오류 원문: 입력값을 처리할 수 있는 데이터 어댑터를 찾지 못함:
6. 오류 이유: `numpy array`클래스와 `numpy list` 클래스의 혼용

### 오류 해결

colab에서 이것저것 만지면서 array를 담은 변수가 list로 바뀌었던 것 같다. 다시 처음부터 실행하니 오류 없음.