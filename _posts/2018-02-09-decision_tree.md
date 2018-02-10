---
title: Decision Tree
category: Basic Machine Learning Algorithm
tag: DT
---
이번 글에서는 **Decision tree** 에 대해서 다루겠습니다. tree라는 말은 이 구조가 하나의 root를 시작으로 branch를 뻗어 가는 구조라는 뜻에서 나왔습니다. 우리가
타겟 변수로 설정한 변수가 이산형(Discrete)이라면 **Classification tree** 라고 하며, 연속형(Continuous)이라면 **Regression tree** 라고 부릅니다.

## 용어 정의
<a href="https://imgur.com/sZgYK1B"><img src="https://i.imgur.com/sZgYK1B.png" width="600px" height="400px" title="source: imgur.com" /></a>

데이터가 k개의 변수와 하나의 결과인 $$D=(x_{1},...,x_{k},Y)$$와 같이 주어진다고 했을 때,
먼저 tree 구조에서 사용되는 용어를 살펴보면:
1. 뿌리마디(root node): 시작되는 마디(위 그림에서 성별)
2. 부모마디(parent node): 주어진 마디의 상위마디(나이의 부모마디는 성별이다.)
3. 자식마디(child node): 주어진 마디의 하위마디(성별의 자식마디는 나이이다.)
4. 끝 마디(terminal node): 자식마디가 없는 마디, 즉 타겟 변수의 결과이다.(위 그림에서 생존 여부)
5. 가지(branch): root-terminal 연결된 마디
6. 깊이(depth): root-terminal 중간마디들의 수

## Decision Tree의 형성

tree의 형성과정은 가지의 분리(Split Rule)-분리 정지(Stop Rule)-가지치기(Pruning)인 크게 세 부분으로 나누어 집니다. 어떠한 기준을 가지고 가지를 정지 조건에 도달할 때까지 분리한 후에 overfitting을 방지하기 위해서 필요없는 가지를 처내는 것 입니다. 이러한 일련의 과정을 해주는 다양한 알고리즘들이 있습니다. 그 중 몇 가지만 살펴보겠습니다.

* CART(Classification And Regression Tree) Algorithm
* ID3(Iterative Dichotomiser 3)
* C4.5 & C5.0

사실 C4.5, C5.0은 ID3의 단점을 수정,보완한 알고리즘이기 때문에 자세하게는 CART와 C4.5,C5.0에 대해 살펴 보겠습니다. 하지만 그 전에 먼저 서로 다른 알고리즘에서 분리 기준으로 어떤 지표를 사용하는지 다루겠습니다.

### Gini impurity(지니 불순도)

**Gini impurity** 는 CART알고리즘에서 사용되는 기준입니다. Gini impurity는 각 자식노드에서 계산된 후에 가중 평균 합을 해서 최종적으로 구해집니다. 기본적으로 Decision tree의 가지 분리 목적이 각 자식노드에 최대한 타겟 변수가 구분되게 분리시키는 것 입니다. 타겟 변수가 <1>, <2>로 두 가지 범주가 있다고 합시다. <1> 기준에서는 노드에 <1>만 있었으면 좋겠는데, <2>라는 불순도가 낀 것 입니다. 즉, $$p_{<2>}$$가 불순도인 셈이죠. 마찬가지로 <2>기준으로는 $$p_{<1>}$$이 불순도가 됩니다. 자기 빼고는 전부 불순도가 되는 것입니다. 그런데, 누구의 불순도가 더 신빙성있냐고 묻는 다면, <1>과 <2>의 비율을 고려해야 합니다. 왜냐하면 비율이 높은 쪽에 가중치를 더 높이주는 것이라고 생각하시면 됩니다. 만약 $$J$$개의 타겟 변수 범주가 있다면, 각 노드의 불순도는 그래서 아래와 같이 구해집니다.

$$ I_{G}(p)=\sum_{i=1}^{J}p_{i}\sum_{k \neq i}p_{k}=\sum_{i=1}^{J} p_{i}(1-p_{i})=\sum_{i=1}^{J}(p_{i}-p_{i}^{2})=
  \sum_{i=1}^{J}p_{i}-\sum_{i=1}^{J} p_{i}^{2}=1-\sum_{i=1}^{J} p_{i}^{2}$$

따라서 실제로 계산할 때는 마지막처럼 $$1-\sum_{i=1}^{J} p_{i}^{2}$$로 간단하게 구할 수 있습니다. 예를 통해 계산해 봅시다.

<a href="https://imgur.com/dJ3g1te"><img src="https://i.imgur.com/dJ3g1te.png" width="600px" height="200px" title="source: imgur.com" /></a>

위의 그림에서 $$R1$$의 지니 불순도는 $$I(R1)=1-\{(\frac{4}{6})^{2}+(\frac{2}{6})^{2}\}=0.444$$이고, $$R2$$의 지니 불순도는 $$I(R2)=1-\{(\frac{3}{4})^{2}+(\frac{1}{4})^{2}\}=0.375$$이다.
따라서 이 분리 기준의 최종적인 지니 불순도를 구하면, $$(6/10) \times I(R1) + (4/10) \times I(R2) = 0.4164$$로 계산됩니다.

### Information gain

**Information gain** 은 C4.5, C5.0에서 사용되는 분리 기준으로 정보 이론에서의 **entropy** 를 기반으로 하는 분리 기준입니다. 먼저, $p_{1},...,p_{J}$를 각 클래스의
확률이라고 한다면 이 때의 entropy는 아래와 같습니다:

$$H(T)=I_{E}(p_{1},...,p_{J}) = - \sum_{i=1}^{J} p_{i} \log_{2} p_{i}$$

여기서 Information gain은 원래의 부모 노드와 자식 노드들의 가중합 사이의 엔트로피의 차로써 구해진다, a를 분리 기준이라고 한다면,

$$IG(T,a)[Information \ Gain] = H(T)[Entropy(parent)] - H(T|a)[Entropy(Children)]$$

사실 부모 노드의 entropy는 같으므로 H(T|a)가 작은 쪽을 선택하면 됩니다. 다시 말해 entropy를 최소화하는 분리 기준을 선택하면 된다는 말이죠. 위와 같은 예시로 entropy 계산을 해봅시다.

<a href="https://imgur.com/dJ3g1te"><img src="https://i.imgur.com/dJ3g1te.png" width="600px" height="200px" title="source: imgur.com" /></a>

위의 그림에서 $$R1$$의 entropy는 $$I(R1)=-\Big\{\frac{4}{6}
\log_{2}(\frac{4}{6})+\frac{2}{6}
\log_{2}(\frac{2}{6})\Big\}=0.9182$$이고, $$R2$$의 entropy는 $$I(R2)-\Big\{\frac{3}{4}
\log_{2}(\frac{3}{4})+\frac{1}{4}
\log_{2}(\frac{1}{4})\Big\}=0.8113$$이다.
따라서 이 분리 기준의 최종적인 entropy를 구하면, $$(6/10) \times I(R1) + (4/10) \times I(R2) = 0.8754$$로 계산됩니다.

지니 불순도와 entropy 모두 작은 분리 기준을 선택하면 가장 낮아지는 변수-최적 분리점을 찾아서 그 기준으로 분리를 하면 됩니다. 범주형은 쉽게 계산되지만, 연속형 변수의 경우에는 몇 가지 과정이 필요합니다. 계산량이 너무 많기 때문이죠. 구체적으로 몇 가지 알고리즘을 살펴봅시다.

## Decision Tree Learning Algorithm

### CART(Classification And Regression Tree)

#### Split Rule

알고리즘 아래와 같은 순서로 진행됩니다.

1. 각 변수에 대해 최적의 분리 기준을 찾는다. 연속형 변수의 경우 먼저 작은 값 부터 큰 값으로 정렬합니다. 정렬된 변수를 통해 제일 큰 값부터 분리 기준 후보로 해서
분리 기준이 최적이 되는 지점을 찾습니다. 범주형 변수의 경우 각 범주로 분류했을 때, 가장 좋은 분리 기준을 찾습니다.
2. 노드의 최고의 분리 기준을 찾습니다. (1에서 구한 값들을 후보로 해서)
3. 정지 규칙(Stop rules)이 충족되지 않는다면 노드를 분리합니다.

Classification tree에서 분리가 되는 기준은 Gini impurity 기준으로 한다면 (부모의 불순도) - (자식들 불순도의 가중합) > 0 이라면 즉, 분리를 했을 때, 불순도가 내려가는 여부에 따라서 분리하면 됩니다. 한편, Regression tree에서는 하나의 노드에서 불순도를 $$e(t)=\sum_{n \in t}(y_{n}-\bar{y}(t))^{2}$$ 와 같이 구하고, 마찬가지로 (부모의 불순도), (자식들 불순도의 가중합) 을 비교해서 불순도가 낮아지면 분리해 낸다.

#### Stop rules

CART 알고리즘은 몇 가지 Stopping Rules들을 가지고 가지 분리를 멈춤니다:

* 노드의 불순도가 0이 될 때 즉, 하나의 노드에 모두 같은 범주에 속하거나 같은 값을 가지고 있을 때,
* 깊이가 사용자가 지정한 깊이 한계에 도달했을 때.
* 노드의 사이즈가 사용자가 지정한 최소 사이즈에 도달했을 때.
* 노드의 분리로 인한 불순도 감소량이 사용자 지정 값보다 작을 때

#### Pruning

CART는 Cost-Complexity Pruning이라는 방법으로 가지 치기를 합니다. 이 방법은 Cost-Complexity function을 최적화 시키는 나무를 찾아가는 것 입니다.

$$Cost-Complexity \ function: \ R_{\alpha}(T)=R(T)+\alpha|f(T)|$$

로 정의 되는데, 여기서 각각의 term의 의미는 아래와 같습니다.

* $$R(T)=\sum_{t \in f(T)} r(t)p(t) = \sum_{t \in f(T)}R(t)$$으로 각 노드의 misclassification errors의 합을 의미합니다. 즉, training error를 의미하죠.
  - $$r(t)=1-max_{k}p(C_{k})$$
  - $$p(t)=\frac{n(t)}{n}$$
* f(T)는 tree T의 terminal 노드들의 집합을 구하는 funtion입니다.
* $$\alpha$$는 Regularization parameter 입니다.
* (부가 정의) $$T_{t}$$를 t가 root인 sub-tree라고 하고, $$T-T_{t}$$를 tree(T)에서 subtree($$T_{t}$$)를 제거한거라고 정의합니다(노드 t는 제외).
* (부가 정의) $$R(t)$$= node t에서의 training error

<a href="https://imgur.com/4zGJBOt"><img src="https://i.imgur.com/4zGJBOt.png" title="source: imgur.com" /></a>

여기서 T에서 subtree $$T_{t}$$를 가지치기하는 경우를 생각해 봅시다. pruning하기 전후의 Cost-Complexity function을 비교해야 합니다: $$R_{\alpha}(T-T_{t})-R_{\alpha}(T)$$
pruning 한 후와 그 전의 Cost-Complexity의 차이를 계산해 보면-그림을 보시면서 계산하면 식 이해가 편하십니다:)

$$R_{\alpha}(T-T_{t})-R_{\alpha}(T)=R(T-T_{t})-R(T)+\alpha(|f(T-T_{t})|-f(T))=R(t)-R(T_{t})+\alpha(1-|f(T_{t})|)$$

$$R(t)-R(T_{t})+\alpha(1-|f(T_{t})|)>0$$이라면 즉, $$\alpha < \frac{R(t)-R(T_{t})}{|f(T_{t})|-1}$$이라면 pruning 후의 Cost-Complexity가 더 높으므로 좋지 않고,
$$R(t)-R(T_{t})+\alpha(1-|f(T_{t})|)<0$$이라면 즉, $$\alpha > \frac{R(t)-R(T_{t})}{|f(T_{t})|-1}$$이라면 pruning 후의 Cost-Complexity가 낮으므로 좋습니다.
그리고 구체적인 알고리즘은 Cost-Complexity를 계속 줄여가는 방향으로 진행됩니다.

1. Initialization $$T^{1}$$를 $$\alpha^{1}=0$$일때 얻어질 tree라 하자. i=1부터 해서
2. $$g_{i}(t)=\frac{R(t)-R(T_{t}^{i})}{|f(T_{t}^{i})|-1}$$를 최소화하는 노드 $$t \in T^{i}$$를 선택한 후에
$$\alpha^{i+1}=g_{i}(t_{i})$$라 하고 $$T^{i+1}=T^{i}-T_{t_{i}}^{i}$$로 새로 구합니다.
3. Output으로 sequence of trees $$T^{1} \supset T^{2} \supset ... \supset \{root\}$$와 sequence of parameters $$\alpha^{1} \leq \alpha^{2} \leq ... \leq $$가
나옵니다.
4. $$\alpha$$의 값을 sequence 중에서 cross validation으로 구해줍니다.

$$g_{i}(t)$$를 최소화하는 노드 t를 고르는 것은 곧, Cost-Complexity가 가장 많이 줄어들게 하는 노드 t를 고르는 것과 같습니다. 아래에서 예시를 하나 보시죠.

<a href="https://imgur.com/zZv6yDr"><img src="https://i.imgur.com/zZv6yDr.png" width="600px" height="300px" title="source: imgur.com" /></a>

세 개의 노드 $$t_{1}, t_{2}, t_{3}$$에 대해서 값을 계산해 봅시다.

#### Iteration 1
$$\alpha^{(1)}=0$$이라 하자.

|   t    |   $$R(t)$$    | $$R(T_{t})$$  | $$g(t)$$ 8
| :-----:  | :-----: | :-----: | :-----: |
| $$t_{1}$$  |  $$\frac{8}{16} \cdot \frac{16}{16}$$ |  0   | $$\frac{8/16-0}{4-1}=\frac{1}{6}$$ |
| $$t_{2}$$  |  $$\frac{4}{12} \cdot \frac{12}{16}$$ |  0   | $$\frac{4/16-0}{3-1}=\frac{1}{8}$$ |
| $$t_{3}$$  |  $$\frac{2}{6} \cdot \frac{6}{16}$$   |  0   | $$\frac{2/16-0}{2-1}=\frac{1}{8}$$ |

* $$ g(t_{2})=g(t_{3}) $$이고 이렇게 같을 때는 더 깊이 있는 노드를 선택하게 됩니다.(여기선 $$g(t_{3})$$)
* $$t_{3}$$에서 가지치기 하고, $$\alpha^{(2)}=1/8$$로 둡니다.

<a href="https://imgur.com/22YnoE2"><img src="https://i.imgur.com/22YnoE2.png" width="600px" height="200px" title="source: imgur.com" /></a>


#### Iteration 2
|   t    |   $$R(t)$$    | $$R(T_{t})$$  | $$g(t)$$ 8
| :-----:  | :-----: | :-----: | :-----: |
| $$t_{1}$$  |  $$\frac{8}{16} \cdot \frac{16}{16}$$ |  $$\frac{2}{16}$$   | $$\frac{8/16-2/16}{3-1}=\frac{6}{32}$$ |
| $$t_{2}$$  |  $$\frac{4}{12} \cdot \frac{12}{16}$$ |  $$\frac{2}{16}$$   | $$\frac{4/16-2/16}{2-1}=\frac{1}{8}$$ |

* $$t_{2}$$에서 $$g(t_{2})$$가 최소가 된다.
* $$t_{2}$$에서 가지치기합니다. $$\alpha^{(3)}=1/8$$로 둡니다.

<a href="https://imgur.com/NpbYLTb"><img src="https://i.imgur.com/NpbYLTb.png" width="600px" height="200px" title="source: imgur.com" /></a>

#### Iteration 3

* $$\alpha^{(4)}=g(t_{1})=\frac{8/16-4/16}{2-1}=\frac{1}{4}$$


#### Best $$\alpha$$값 찾기

* $$ \alpha^{(0)}=0,\alpha^{(1)}=1/8,\alpha^{(2)}=1/8,\alpha^{(3)}=1/4 $$가 우리가 구한 $$\alpha$$값들 입니다.
* 위에서 언급했듯이 목표는 Cost-Complexity function을 최소화하는 $$T$$를 찾는 겁니다.
  - $$0 \leq \alpha < 1/8$$, $$T_{1}$$이 best입니다.
  - $$\alpha = 1/8$$, $$T_{2}$$가 best입니다.
  - $$1/8 < \alpha < 1/4/$$, $$T_{3}$$가 best입니다.
  - $$1/4 \leq \alpha$$, $$T_{3}$$가 best입니다.
* 왜 위와 같은 결과가 나오냐면 $$0 \leq \alpha < 1/8$$라는 범위에서는 $$T_{t}=T_{t_{2}}$$이라고 할 때, $$\alpha \leq \frac{R(t)-R(T_{t})}{|f(T_{t})|-1}$$가
되고, $$T_{t}=T_{t_{1}}$$이라고 할 때, $$\alpha > \frac{R(t)-R(T_{t})}{|f(T_{t})|-1}$$이기 때문이다. 이 점에서 위와 같은 범위에서 $$T_{t_{1}}$$이 $$T_{t_{2}}$$보다 좋은 것은 명확하다. 마찬가지로 $$T_{t_{1}}$$이 $$T_{t_{3}}$$보다 더 좋은 것도 명확하다. 즉, 해당 T로 인한 $$g(t)$$의 최소값이 지정된 $$\alpha$$보다 커지면 좋은 가지치기라고 할 수 없다.       

### C4.5 & C5.0

**C5.0** 알고리즘은 Information gain을 분리 기준으로 사용합니다. CART와 달리 binary tree 혹은 multi branches tree를 만들어 냅니다. 다음에..

#### Pruning

Binomial Confidence Limit 사용한다. 다음에..

## Code

R로 간단하게 Algorithm을 시행해 보았습니다.
```{r}
## CART Algorithm
library(rpart)
data(iris)

#####################################################################################################
# rpart function option   
# minsplit : split하기 위한 최소 관측값 수 (defualt=20)
# minbucket : 자식마디에 있어야 하는 최소 관측값 수  (defualt= round(minsplit/3))
# cp(complexity parameter) : 위에서 언급한 알파값입니다, 높으면 간단한 tree, 낮으면 복잡한 tree가 됩니다.
# xval : cross-validation fold 갯수(default=10)
# maxdepth : 최대 깊이 (default=30)
#### rpart 함수 참고 : https://cran.r-project.org/web/packages/rpart/rpart.pdf #######################




```


```{r}
## C5.0 Algorithm
library(C50)
data(iris)

# Data Shuffle
set.seed(123)
g <- runif(nrow(iris))
irisr <- iris[order(g),]

# Classification tree with C5.0
m1 <- C5.0(irisr[1:100, -5], irisr[1:100, 5])
summary(m1)

# Predict
p1 <- predict(m1, irisr[101:150,])
table(irisr[101:150,5], p1)

#
plot(m1)
```

## 복수의 Tree를 이용한 테크닉

앙상블(Ensemble)이라는 방법은 간단하게 말해 모델 여러 개를 사용해서 최종적인 예측 결과를 내는 것을 의미합니다. (앙상블의 방법에는 배깅(Bagging)과 부스팅(Boosting)이
있는데, 이에 대해서는 다른 글에서 다루겠습니다.) Decision tree 모델을 앙상블을 통해서 더 좋은 결과를 내게 하는 방법론은 여러가지 있습니다.

* Boosted trees : Adaboost가 있습니다(다른 글에서 다루겠습니다.)
* Bagged decision trees : training 데이터에서 반복해서 표본을 추출해서 복수의 tree 모델을 만들고, 이러한 tree들로 투표 혹은 평균을 내서 결론을 내는 방법입니다.(Random forest)
