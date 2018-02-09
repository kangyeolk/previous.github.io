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

# Gini impurity(지니 불순도)
Gini impurity는 CART알고리즘에서 사용되는 기준입니다. Gini impurity는 각 자식노드에서 계산된 후에 가중 평균 합을 해서 최종적으로 구해집니다. 기본적으로 Decision tree의 가지 분리 목적이 각 자식노드에 최대한 타겟 변수가 구분되게 분리시키는 것 입니다. 타겟 변수가 <1>, <2>로 두 가지 범주가 있다고 합시다. <1> 기준에서는 노드에 <1>만 있었으면 좋겠는데, <2>라는 불순도가 낀 것 입니다. 즉, $$p_{<2>}$$가 불순도인 셈이죠. 마찬가지로 <2>기준으로는 $$p_{<1>}$$이 불순도가 됩니다. 자기 빼고는 전부 불순도가 되는 것입니다. 그런데, 누구의 불순도가 더 신빙성있냐고 묻는 다면, <1>과 <2>의 비율을 고려해야 합니다. 왜냐하면 비율이 높은 쪽에 가중치를 더 높이주는 것이라고 생각하시면 됩니다.$$J$$개의 타겟 변수 범주가 있을 때, 각 노드의 불순도는 그래서 아래와 같이 구해집니다.

$$ I_{G}(p)=\sum_{i=1}^{J}p_{i}\sum_{k \neq i}p_{k}=\sum_{i=1}^{J} p_{i}(1-p_{i})=\sum_{i=1}^{J}(p_{i}-p_{i}^{2})=
  \sum_{i=1}^{J}p_{i}-\sum_{i=1}^{J} p_{i}^{2}=1-\sum_{i=1}^{J} p_{i}^{2}$$

따라서 실제로 계산할 때는 마지막처럼 $$1-\sum_{i=1}^{J} p_{i}^{2}$$로 간단하게 구할 수 있습니다. 이러한 지니 불순도가 가장 낮아지는 변수-최적 분리점을 찾아서 그 기준으로 분리를 하면 됩니다. 범주형은 쉽게 계산되지만, 연속형 변수의 경우에는 분리점 몇개를 지정해서


## 복수의 Tree를 이용한 테크닉


## Structured Journalism ==> topic
<a href="http://imgur.com/HqWB22Q"><img src="http://i.imgur.com/HqWB22Q.png" width="600px" title="source: imgur.com" /></a> 이미지 넣기

강조
**맥락(context)**을 알 수 있게 하면 좋겠다는 생각을 했습니다. 이와 관련된 개념이 바로 **[스트럭처 저널리즘(Structured Journalism)](https://www.kpf.or.kr/site/kpf/research/selectMediaPdsView.do?seq=7562)**입니다.

옆에 사이드
> 언론은 특종 경쟁에 매몰돼 있다. 같은 사실을 보도하더라도 어제와 '다른' 뉴스, 남들과는 '차별화한' 시각을 추구한다. 그런데 많은 언론사들이 유사한 제목의 비슷한 기사를 보도한다면 해당 사건이나 이슈는 사회에 파급력이 클 가능성이 높다. 바꿔 말해 해당 기사는 차별성을 다소 포기하더라도 보도할 만한 가치가 있는 중요한 아티클이라는 이야기다.

# latex
$$\begin{pmatrix} { A }_{ 11 } & { A }_{ 12 } & ... & { A }_{ 1n } \\ { A }_{ 21 } & { A }_{ 22 } & ... & { A }_{ 2n } \\ ... & ... & ... & ... \\ { A }_{ n1 } & { A }_{ n2 } & ... & { A }_{ nn } \end{pmatrix}\begin{pmatrix} { x }_{ 1 } \\ { x }_{ 2 } \\ ... \\ { x }_{ n } \end{pmatrix}=\begin{pmatrix} { A }_{ 11 }{ x }_{ 1 }+{ A }_{ 12 }{ x }_{ 2 }+...+{ A }_{ 1n }{ x }_{ n } \\ { A }_{ 21 }{ x }_{ 1 }+{ A }_{ 22 }{ x }_{ 2 }+...+{ A }_{ 2n }{ x }_{ n } \\ ... \\ { A }_{ n1 }{ x }_{ 1 }+{ A }_{ n2 }{ x }_{ 2 }+...+{ A }_{ nn }{ x }_{ n } \end{pmatrix}=\begin{pmatrix} { \lambda x }_{ 1 } \\ \lambda { x }_{ 2 } \\ ... \\ \lambda { x }_{ n } \end{pmatrix}$$
