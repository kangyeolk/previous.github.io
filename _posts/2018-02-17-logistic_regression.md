---
title: Logistic Regression & Softmax Regression
category: Statistics-Regression
tag: Regression
---

## Logistic Regression

선형 회귀에서는 종속변수가 연속형(continuous) 변수였기 때문에 직선 형태의 추정이 때론 잘 맞아들어가기도 하였습니다. 하지만, 종속변수가 이산형(discrete) 변수 일때 같은 방식으로 직선을 추정하게 된다면 그러한 추정은 올바르지 않을 것 입니다.

<center><a href="https://imgur.com/gUmxjpM"><img src="https://i.imgur.com/gUmxjpM.png" width="400px" height="400px" title="source: imgur.com" /></a></center>

만약 종속변수가 이산형이라면 먼저 생각할 수 있는 것은 이것을 연속형으로 바꾸어 주자는 생각입니다. 종속변수가 binary한 상황에서 $$p(Y=0|X), p(Y=1|X)$$을 구하고 그 중 $$p(Y=1|X)$$를 종속변수로 사용할 수 있습니다. 이는 분명히 연속형 변수이긴 하지만 범위가 $$[0,1]$$이므로 범위를 바꿔줄 필요가 있습니다. 이 때 **실패확률 대비 성공확률** 인 **$$Odds=\frac{p(Y=1|X)}{1-p(Y=1|X)}$$** 를 생각해 볼 수 있습니다. 하지만 이 역시 범위가 $$[0, \infty]$$이므로 여기에 log를 씌워서 **logit** 이라는 binary한 종속변수에서 변형한 것으로 종속변수로 사용하게 됩니다. 이렇게 된다면 종속변수가 연속형 변수가 됨은 물론 범위가 $$[-\infty, \infty]$$이기 때문에 문제 없이 회귀적합을 할 수 있습니다. 이것을 **Logistic Regression** 이라고 합니다.

> $$\log \Big(\frac{p(Y=1|X)}{1-p(Y=1|X)} \Big) = X \beta$$

## $$\beta$$에 대한 추정 및 해석

한편 위의 식을 변형하면 $$p(Y=1|X)=\frac{e^{X \beta}}{1+e^{X \beta}}$$ 라는 것을 알 수 있습니다. 우변 같은 꼴을하고 있는 $$f(x)$$를 **Logistic function(Sigmoid function)** 이라고 부릅니다. 아래는 표준 logistic function (출처:wikipedia)

<center><a href="https://imgur.com/tEpmk00"><img src="https://i.imgur.com/tEpmk00.png" width="400px" height="400px" title="source: imgur.com" /></a></center>

그리고 $$\frac{e^{X_{i} \beta}}{1+e^{X_{i} \beta}} = p_{i}$$라고 한다면 데이터 하나에 대한 식은 $$p_{i}^{y_{i}}(1-p_{i})^{1-y_{i}}$$ 꼴로 나타낼 수 있습니다. $$n$$개의 데이터가 주어졌을 때, $$\beta$$에 대한 추정은 데이터 전체에 대한 likelihood $$\prod_{i=1}^{n} p_{i}^{y_{i}}(1-p_{i})^{1-y_{i}}$$를 통해 Maximum likelihood estimation을 구하는 식으로 됩니다. 하지만, 이 likelihood에서 MLE가 수식적으로 구해지지 않으므로 gradient descent 방법과 같이 점진적인 방법을 통해서 이를 구해야 합니다. 그렇기 때문에 때론 $$\beta$$값이 수렴하지 않기도 합니다. 수렴하지 않는 원인 중 하나는 예측변수들 사이의 다중공선성(multicolinearity)인데(사실 선형회귀에서도 다중공선성 문제가 있다면 회귀계수가 제대로 구해지지 않기도 합니다.), 이를 해소하기 위해서는 Ridge Regression이나 Lasso Regression처럼 regularization method를 도입해서 해결을 할 수 있습니다. 이에 대해서는 다른 포스팅에서 다루겠습니다.

또한 $$\beta$$를 해석함에 있어서 회귀분석에서 X의 변화에 따른 Y의 변화량으로 해석하는 것처럼 로지스틱 회귀분석에서는 X의 변화에 따른 Y의 logit의 변화 입니다. 정확히는 X가 한 단위 증가하면, y의 logit이 $$\beta$$만큼 증가합니다.

## Softmax Regression

위의 내용만 놓고 본다면 Logistic Regression이 binary한 종속변수만 다룰 수 있는 것처럼 보입니다. 하지만 **Logistic Regression을 여러 번 반복해서 한다면** Multiclass Classification 문제 역시 해결할 수 있습니다. 예컨대, 종속변수의 범주가 $$A,B,C$$라면 $$p(y=A \lvert x), p(y=B \lvert x), p(y=C \lvert x)$$ 세 가지 확률을 구하는 겁니다. 다범주 문제를 $$A$$인지 아닌지, $$B$$인지 아닌지, $$C$$인지 아닌지와 같이 binary한 문제 3개로 바꾸어서 각각의 확률을 구하는 것 입니다. 즉, Binary Classification을 종속변수의 수($$k$$)만큼 수행합니다. 그리고 확률의 합을 1로 만들어 주어야 합니다. 지금은 각 Binary Classification에서 확률값을 구했으므로 확률값이 전부 $$[0, 1]$$의 범위를 가집니다. 다 합치면 그 값이 1을 초과합니다. 그래서 Softmax function : $$f(z_{j})=\frac{e^{z_{j}}}{\sum_{i=1}^{k} e^{z_{i}}}$$ 을 사용합니다. 위에서 구한 확률값을 각각 $$y_{1}, y_{2}, y_{3}$$이라고 하면 이 확률값들은 이제

> $$y_{1} \to \frac{e^{y_{1}}}{\sum_{i=1}^{3} e^{y_{i}}}, y_{2} \to \frac{e^{y_{2}}}{\sum_{i=1}^{3} e^{y_{i}}}, y_{3} \to \frac{e^{y_{3}}}{\sum_{i=1}^{3} e^{y_{i}}}$$

와 같이 되는 것 입니다. 새로 구해진 값들의 합은 당연히 1이 되고 그 중 max 값을 해당 class로 할당하면 됩니다. Binary Classification을 할 때의 방식은 원래와 같습니다.

## Python code

softmax regression을 직접 구현해 보겠습니다. 데이터는 MNIST 데이터 입니다. 다 아시겠지만 0-9부터 손글씨 데이터입니다. 아래는 숫자 7의 예.

```python
from tensorflow.examples.tutorials.mnist import input_data
import pandas as pd
import numpy as np

# Download data
MNIST = input_data.read_data_sets("./")

# Split data
X_train = MNIST.train.images
X_test = MNIST.test.images
y_train = MNIST.train.labels.astype("int")
y_test = MNIST.test.labels.astype("int")

print(X_train.shape, X_test.shape, y_train.shape, y_test.shape)

from matplotlib import pyplot as plt

X0 = X_train[0].reshape(28, 28)# from 1*784 to 28*28
plt.rc('image', cmap='binary') # set runtime configuration
plt.matshow(X0)                # plot 28*28 matrix
plt.show(); y_train[0]         # show plot on screen
```
<center><a href="https://imgur.com/mxq6zHK"><img src="https://i.imgur.com/mxq6zHK.png" width="300px" height="300px" title="source: imgur.com" /></a></center>

구현코드는 아래와 같습니다. 구현된 softmax로의 예측과 내장함수로의 예측의 결과가 서로 같은 것을 알 수 있습니다.

```python
from sklearn import cross_validation
from sklearn.linear_model import LogisticRegression
from sklearn import metrics

# Part of data for time-saving
X_train90, X_train10, y_train90, y_train10 = cross_validation.train_test_split(X_train, y_train, test_size=0.1, random_state=123)

# Softmax Regression implementation

def softmax(X_train, y_train, X_test):
    nClass = len(set(y_train))
    probPerClass = np.zeros((nClass, X_test.shape[0]))
    for k in range(0, nClass):
        LR_model = LogisticRegression(random_state=123)
        LR_model.fit(X_train, y_train == k)
        probPerClass[k] = LR_model.predict_proba(X_test)[:,1]
    expProb = np.exp(probPerClass)
    expSum = np.sum(expProb, axis=0)

    prob = expProb[:, ]/expSum
    pred = np.argmax(prob, axis=0)
    return pred    

# implementation softmax result
predictions = softmax(X_train10, y_train10, X_test)
print(metrics.classification_report(predictions, y_test))

# Built-in softmax regression result
LR_model2 = LogisticRegression(random_state=123)
LR_model2.fit(X_train10, y_train10)
pred = LR_model2.predict(X_test)
print(metrics.classification_report(pred, y_test))
```

<center><a href="https://imgur.com/zBZ8mG1"><img src="https://i.imgur.com/zBZ8mG1.png" width="500px" height="500px" title="source: imgur.com" /></a></center>
