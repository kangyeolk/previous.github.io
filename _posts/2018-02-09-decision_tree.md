---
title: Decision Tree
category: Basic Machine Learning Algorithm
tag: DT
---
이번 글에서는 **Decision tree** 에 대해서 다루겠습니다. tree라는 말은 이 구조가 하나의 root를 시작으로 branch를 뻗어 가는 구조라는 뜻에서 나왔습니다. 우리가
타겟 변수로 설정한 변수가 이산형(Discrete)이라면 **Classification tree** 라고 하며, 연속형(Continuous)이라면 **Regression tree** 라고 부릅니다.

## 용어 정의
<a href="https://imgur.com/sZgYK1B"><img src="https://i.imgur.com/sZgYK1B.png" width="600px" title="source: imgur.com" /></a>

데이터가 k개의 변수와 하나의 결과인 $$D=(x_{1},...,x_{k},Y)$$와 같이 주어진다고 했을 때,
먼저 tree 구조에서 사용되는 용어를 살펴보면:
1. 뿌리마디(root node): 시작되는 마디(위 그림에서 성별)
2. 부모마디(parent node): 주어진 마디의 상위마디(나이의 부모마디는 성별이다.)
3. 자식마디(child node): 주어진 마디의 하위마디(성별의 자식마디는 나이이다.)
4. 끝 마디(terminal node): 자식마디가 없는 마디, 즉 타겟 변수의 결과이다.(위 그림에서 생존 여부)
5. 가지(branch): root-terminal 연결된 마디
6. 깊이(depth): root-terminal 중간마디들의 수

## Decision Tree의 종류

위에서 언급했듯이 tree는 **Classification tree** / **Regression tree** 가 있으며, 두 개를 합쳐서 **CART(Classification And Regression Tree)**
라는 말로 부른다. 

## Structured Journalism ==> topic
<a href="http://imgur.com/HqWB22Q"><img src="http://i.imgur.com/HqWB22Q.png" width="600px" title="source: imgur.com" /></a> 이미지 넣기

강조
**맥락(context)**을 알 수 있게 하면 좋겠다는 생각을 했습니다. 이와 관련된 개념이 바로 **[스트럭처 저널리즘(Structured Journalism)](https://www.kpf.or.kr/site/kpf/research/selectMediaPdsView.do?seq=7562)**입니다.

옆에 사이드
> 언론은 특종 경쟁에 매몰돼 있다. 같은 사실을 보도하더라도 어제와 '다른' 뉴스, 남들과는 '차별화한' 시각을 추구한다. 그런데 많은 언론사들이 유사한 제목의 비슷한 기사를 보도한다면 해당 사건이나 이슈는 사회에 파급력이 클 가능성이 높다. 바꿔 말해 해당 기사는 차별성을 다소 포기하더라도 보도할 만한 가치가 있는 중요한 아티클이라는 이야기다.

# latex
$$\begin{pmatrix} { A }_{ 11 } & { A }_{ 12 } & ... & { A }_{ 1n } \\ { A }_{ 21 } & { A }_{ 22 } & ... & { A }_{ 2n } \\ ... & ... & ... & ... \\ { A }_{ n1 } & { A }_{ n2 } & ... & { A }_{ nn } \end{pmatrix}\begin{pmatrix} { x }_{ 1 } \\ { x }_{ 2 } \\ ... \\ { x }_{ n } \end{pmatrix}=\begin{pmatrix} { A }_{ 11 }{ x }_{ 1 }+{ A }_{ 12 }{ x }_{ 2 }+...+{ A }_{ 1n }{ x }_{ n } \\ { A }_{ 21 }{ x }_{ 1 }+{ A }_{ 22 }{ x }_{ 2 }+...+{ A }_{ 2n }{ x }_{ n } \\ ... \\ { A }_{ n1 }{ x }_{ 1 }+{ A }_{ n2 }{ x }_{ 2 }+...+{ A }_{ nn }{ x }_{ n } \end{pmatrix}=\begin{pmatrix} { \lambda x }_{ 1 } \\ \lambda { x }_{ 2 } \\ ... \\ \lambda { x }_{ n } \end{pmatrix}$$
