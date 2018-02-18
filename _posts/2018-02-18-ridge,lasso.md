---
title: Ridge & Lasso Regression
category: Statistics-Regression
tag: Regression
---

선형회귀에서 OLS로 추정한 추정값 $$\hat{\beta}$$의 분산은 $$Var(\hat{\beta})=\sigma^{2}(X^{t}X)^{-1}$$과 같이 구해집니다.(<a href="https://stats.stackexchange.com/questions/68151/how-to-derive-variance-covariance-matrix-of-coefficients-in-linear-regression">전개 과정)</a> 당연히 이와 같은 분산은 작을 수록 좋습니다. 하지만 분산이 data dependence 하기 때문에 특정 데이터에서는 분산이 매우 커지거나 역행렬 추정 자체가 안되는 경우가 생깁니다. 만약 설명변수 사이의 다중공선성(multicolinearity)이 심하게 존재한다면 분산이 매우 커지게 되며 (추정값 역시 엄청 커질 가능성이 존재합니다.), 데이터의 수($$n$$)보다 변수 개수($$p$$)가 많은 경우 역행렬 자체를 구할 수 없게 되는 문제가 생깁니다. 이를 해결하기 위해 Ridge 혹은 Lasso Regression을 하게 됩니다.

## Ridge Regression

$$\beta$$ 값이 일정 수준 이상으로 팽창하는 것(음 혹은 양의 방향)을 막기 위해서 $$\beta$$ 값에 대한 제약 조건(constraint)을 생각해 볼 수 있습니다. 기존의 OLS에서는 추정값과 주어진 $$y$$의 차이를 최소화하려고 했습니다. 즉, $$minimize \ \sum_{i=1}^{n} (y_{i} - \beta z_{i})^{2}$$(보통 데이터는 stadardization 됩니다.)하고자 하죠. Ridge Regression에서는 제약 조건으로 $$\sum_{j=1}^{p} \beta_{j}^{2} \leq t$$을 줍니다. 그러면  제약 조건 아래서 최적화 문제를 푸는 일반적인 방법인 라그랑주 승수법(Lagrange multiplier method)에 의해서 $$minimize \ \sum_{i=1}^{n} (y_{i} - \beta z_{i})^{2} + \lambda \sum_{j=1}^{p} \beta_{j}^{2}$$로 바꿀 수 있습니다. 이 문제를 풀면:

> $$\hat{\beta}_{\lambda}^{ridge} = (Z^{T}Z + \lambda I_{p})^{-1} Z^{T} y$$

이 때 $$\lambda$$를 **shrinkage parameter** 라고 합니다. $$\lambda$$는 regularization 정도를 조정합니다. $$\lambda$$의 변화를 살펴보면:

* $$\lambda \to 0$$일떄, OLS의 해와 같게 됩니다.
* $$\lambda \to \infty$$일떄, $$\hat{\beta}_{\lambda}^{ridge}$$=0 입니다.

두 번째 사실은 $$(Z^{T}Z + \lambda I_{p})^{-1}$$ 부분의 안쪽 행렬의 대각성분이 $$\infty$$로 가는 것을 보면 알 수 있습니다. 그래서 우리는 최적의 $$\lambda$$를 찾아야 하는데, 이는 보통 Cross Validation으로 찾을 수 있습니다.

## Lasso Regression

Lasso Regression은 Ridge Regression과 비슷합니다. 같은 문제인 $$minimize \ \sum_{i=1}^{n} (y_{i} - \beta z_{i})^{2}$$(보통 데이터는 stadardization 됩니다.)에서 Lasso Regression에서는 제약 조건으로 $$\sum_{j=1}^{p} \lvert \beta_{j} \lvert \leq t$$을 줍니다. 그러면  제약 조건 아래서 최적화 문제를 푸는 일반적인 방법인 라그랑주 승수법(Lagrange multiplier method)에 의해서 $$minimize \ \sum_{i=1}^{n} (y_{i} - \beta z_{i})^{2} + \lambda \sum_{j=1}^{p} \lvert \beta_{j} \lvert$$로 바꿀 수 있습니다. 이 문제는 Ridge Regression 처럼 딱 떨어지는 해를 찾을 수가 없습니다. 그래서 iterative한 과정을 통해서 구하게 됩니다. 한편 위와 같이 $$\lambda$$를 **shrinkage parameter** 라고 합니다. $$\lambda$$는 regularization 정도를 조정합니다. 마찬가지로 $$\lambda \to 0$$일떄, OLS의 해와 같게 됩니다. Lasso Regression에서는 많은 Ridge Regression에 비해서 많은 $$\beta_{j}$$들이 0이 될 수 있습니다.

## Python code

$$Z^{T}Z$$가 full rank가 아니더라도 ridge와 lasso는 해를 구할 수 있습니다. 또한, $$p >> n$$인 상황에서 $$\beta$$값을 축소하므로써 변수 선택을 가능하게 해줍니다. 두 가지 Regression을 python으로 해 볼 수 있습니다. 이는 <a href="https://www.analyticsvidhya.com/blog/2016/01/complete-tutorial-ridge-lasso-regression-python/"> 포스팅</a>내용을 참고해서 구현했습니다.

```python
import numpy as np
import pandas as pd
import random
import matplotlib.pyplot as plt

# Generate data
x = np.array([i*np.pi/180 for i in range(60,300,4)])
np.random.seed(123)
y = np.sin(x) + np.random.normal(0,0.2,len(x))
data = pd.DataFrame(np.column_stack([x,y]),columns=['x','y'])
plt.plot(data['x'],data['y'],'.')
plt.show()
```

```python
# Append powers of data
for i in range(2,16):
    colname = 'x_%d'%i      
    data[colname] = data['x']**i

# Initialize predictors to be set of 15 powers of x
predictors=['x']
predictors.extend(['x_%d'%i for i in range(2,16)])

# Set the different values of alpha
alpha = [1e-15, 1e-4, 1e-2, 1, 5, 10]

# Dataframe for storing coefficients.
col = ['rss','intercept'] + ['coef_x_%d'%i for i in range(1,16)]
ind = ['alpha_%.2g'% alpha[i] for i in range(0,6)]
coef_matrix_ridge = pd.DataFrame(index=ind, columns=col)
coef_matrix_lasso = pd.DataFrame(index=ind, columns=col)

# Values for subplot
models_to_plot = {1e-15:231, 1e-4:232, 1e-2:233, 1:234, 5:235, 10:236}

```
데이터를 만들고 데이터의 제곱수들을 다른 변수로 사용합니다.

```python
from sklearn.linear_model import Ridge, Lasso

def ridge_regression(data, predictors, alpha, models_to_plot={}):
    #Fit the model
    ridge_model = Ridge(alpha=alpha,normalize=True)
    ridge_model.fit(data[predictors],data['y'])
    y_pred = ridge_model.predict(data[predictors])

    # Append plot
    if alpha in models_to_plot:
        plt.subplot(models_to_plot[alpha])
        plt.tight_layout() # preventing plot from overlapping
        plt.plot(data['x'],y_pred)
        plt.plot(data['x'],data['y'],'.')
        plt.title('Plot for alpha: %.3g'%alpha)

    # Return
    rss = sum((y_pred-data['y'])**2)
    ret = [rss]
    ret.extend([ridge_model.intercept_])
    ret.extend(ridge_model.coef_)
    return retfrom

def lasso_regression(data, predictors, alpha, models_to_plot={}):
    #Fit the model
    lasso_model = Lasso(alpha=alpha,normalize=True, max_iter=1e5)
    lasso_model.fit(data[predictors],data['y'])
    y_pred = lasso_model.predict(data[predictors])

    #Check if a plot is to be made for the entered alpha
    if alpha in models_to_plot:
        plt.subplot(models_to_plot[alpha])
        plt.tight_layout()
        plt.plot(data['x'],y_pred)
        plt.plot(data['x'],data['y'],'.')
        plt.title('Plot for alpha: %.3g'%alpha)

    # Return
    rss = sum((y_pred-data['y'])**2)
    ret = [rss]
    ret.extend([lasso_model.intercept_])
    ret.extend(lasso_model.coef_)
    return ret
```
Ridge와 Lasso Regression 할 수 있는 함수를 정의했습니다.

```python
for i in range(6):
    coef_matrix_ridge.iloc[i,] = ridge_regression(data, predictors, alpha[i], models_to_plot)
    coef_matrix_lasso.iloc[i,] = lasso_regression(data, predictors, alpha[i], models_to_plot)
```

<center><a href="https://imgur.com/itLcvUz"><img src="https://i.imgur.com/itLcvUz.png" width="500px" height="300px" title="source: imgur.com" /></a></center>

초록선이 Lasso 추정선이고, 파란선이 Ridge 추정선입니다. 확실히 $$\alpha$$가 높아질 수록 선이 단순해지는 것을 볼 수 있습니다. 특히 $$\alpha$$가 많이 높아지면, Lasso 추정선은 거의 수평선인데, 이는 Lasso에서 대부분의 $$\beta$$를 0으로 만들어서 그렇다고 할 수 있습니다.

```python
pd.options.display.float_format = '{:,.2g}'.format
coef_matrix_ridge
coef_matrix_lasso
```

<center><a href="https://imgur.com/CcOSI4J"><img src="https://i.imgur.com/CcOSI4J.png" width="300px" height="100px" title="source: imgur.com" /></a></center>
<center><a href="https://imgur.com/WlWQE0B"><img src="https://i.imgur.com/WlWQE0B.png" width="300px" height="100px" title="source: imgur.com" /></a></center>

위쪽 계수가 Ridge이고 아래쪽 계수가 Lasso입니다. 확실히 Ridge는 0이 되는 계수가 없지만, Lasso는 많은 계수를 0으로 만드는 것을 볼 수 있습니다.
