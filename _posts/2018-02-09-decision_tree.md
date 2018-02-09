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

위의 그림에서 $$R1$$의 지니 불순도는 $$I(R1)=1-\{(4/6)^{2}+(2/6)^{2}\}=0.444$$이고, $$R2$$의 지니 불순도는 $$I(R2)=1-\{(3/4)^{2}+(1/4)^{2}\}=0.375$$이다.
따라서 이 분리 기준의 최종적인 지니 불순도를 구하면, $$(6/10) \times I(R1) + (4/10) \times I(R2) = 0.4164$$로 계산됩니다.

### Information gain

**Information gain** 은 C4.5, C5.0에서 사용되는 분리 기준으로 정보 이론에서의 **entropy** 를 기반으로 하는 분리 기준입니다. 먼저, $p_{1},...,p_{J}$를 각 클래스의
확률이라고 한다면 이 때의 entropy는 아래와 같습니다:

$$H(T)=I_{E}(p_{1},...,p_{J})=-\sum_{i=1}^{J}p_{i}log_{2}p_{i}}$$
여기서 Information gain은 원래의 부모 노드와 자식 노드들의 가중합 사이의 엔트로피의 차로써 구해진다, a를 분리 기준이라고 한다면:
$$IG(T,a)[Information Gain] = H(T)[Entropy(parent)] - H(T|a)[Entropy(Children)]$$
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

### C4.5 & C5.0

**C4.5** 알고리즘의 목적은 위에서 언급한 Information gain을 최대화 시키는 Decision tree를 찾는 것 입니다. 또한 이 알고리즘은 tree를 만드는 과정을 종료한 후에
가지치기(pruning)를 하는 특징을 가집니다.

```
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
