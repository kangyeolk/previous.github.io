---
title: 네트워크 기본 분석
category: Network
tag: Network
---

이번 글에서는 현대 사회의 여러 현상에서 등장하는 **네트워크** 에 대해서 다루겠습니다. 여담으로 Barabási Albert-László가 쓴 저서 Linked: the New Science of Networks은 네트워크 이론의 시초가 된 책입니다. Barabási는 1999년 Scale-free network(척도 없는 네트워크)에 대해서 소개하면서 Barabási-Albert model을 제시하게 됩니다. 이 모델에 대해서는 다른 글에서 다루기로 하고 여기서는 네트워크의 개념과 그에 대한 몇 가지 측정 개념에 대해서 다루겠습니다. 참고자료는 wikipedia와 고려대학교 허명회 교수님의  '탐색적 데이터 분석'  수업 시간의 강의 자료입니다.

## 네트워크의 정의와 표현

### 정의

네트워크를 자료 구조로 치면 directed graph 혹은 undirected graph 입니다. 즉, node들과 edge들의 집합으로 구성된 하나의 구조입니다. 이를 조금 풀어써보면 **네트워크는 다수의 연결된 또는 연결되지 않은 node로 구성된 구조입니다.** 그리고 이를 표현 하기 위한 방법은 두 가지가 있습니다.

### adjacency matrix(인접 행렬)

**adjacency matrix** 은 $$n$$개의 node로 구성된 네트워크에서 노드 간 연결 상태를 나타내주는 $$n \times n$$행렬이다. 그 행렬 $$A=(a_{ij}); \ i,j=1,\cdots,n)$$에서 node $$i$$에서 노드 j가 연결되면 $$a_{ij}=1$$이고 그렇지 않으면 $$a_{ij}=0$$이다.

### edge list(변 목록)

위에서 언급된 연결선은 edge입니다. 네트워크는 이러한 edge들의 목록으로 표현 할 수 있습니다. node $$i$$에서 $$j$$로 가는 변은 $$(i,j)$$로 표기된다.

<center><a href="https://imgur.com/mR3WBdF"><img src="https://i.imgur.com/mR3WBdF.png" width="300px" height="300px" title="source: imgur.com" /></a></center>

이와 같은 네트워크 구조를 위에서 정의된 두 가지 표현 방식으로 나타낼 수 있습니다. adjacency matrix 형식으로 나타낸다면:

$$
A=
  \begin{bmatrix}
    0 & 1 & 1 & 0 & 1 \\
    0 & 0 & 1 & 0 & 0 \\
    1 & 0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 & 0 \\
    1 & 0 & 0 & 0 & 0
  \end{bmatrix}
$$

그리고 edge list 꼴로 나타내면 아래와 같이 나타낼 수도 있습니다:

$$
  E=
    \begin{bmatrix}
      1 & 2 \\
      1 & 3 \\
      1 & 5 \\
      2 & 3 \\
      3 & 1 \\
      3 & 4 \\
      5 & 1
    \end{bmatrix}

$$

## 네트워크의 특징

### 최단거리 행렬

node간 가장 짧은 경로의 길이는 두 node 사이의 거리를 잴 때 필요한 행렬 입니다. 예시로 나온 네트워크에서 $$M2 \to M5$$의 최단경로는 $$M2 \to M3 \to M1 \to M5$$로 최단거리는 3입니다. 둘 사이의 경로가 존재하지 않는 경우에는 $$\infty$$로 거리를 표기합니다. 위의 예시에서 최단 거리 행렬:

$$
D=
  \begin{bmatrix}
    0 & 1 & 1 & 2 & 1 \\
    2 & 0 & 1 & 2 & 3 \\
    1 & 2 & 0 & 1 & 2 \\
    \infty & \infty & \infty & 0 & \infty \\
    1 & 2 & 2 & 3 & 0
  \end{bmatrix}
$$

### 네트워크의 방향성

네트워크를 나타내는 행렬이 대칭적이면 무방향 네트워크(undirected network)이고 비대칭적이면 방향 네트워크(directed network)이다. 즉, 무방향 네트워크에서는 $$i \to j \Leftrightarrow j \to i$$이다. 하지만 방향 네트워크에서는 그렇지 않은 예가 있다.

### 네트워크의 밀도

**네트워크의 밀도는 가능한 총 연결선 수 대비 총 연결선의 수이다.** 가능한 총 연결선 수는 방향성 네트워크의 경우 $$_n P_2 $$ 이고, 무방향성 네트워크의 경우 $$ _n C_2 $$이다. 그리고 총 연결선 수는 중복되지 않게 세주어야 한다. 즉, in-degree 혹은 out-degree 둘 중 하나만 선택해서 사용하면된다. 예시의 경우에는 총 연결선 수가 7개이고, 가능한 총 수가 20개(방향성 존재) 이므로 밀도=7/20이다.

> $$density = \frac{\sum_{i=1}^{n} \#(number \ of \ indegrees)}{n(n-1)} = \frac{\sum_{i=1}^{n} \#(number \ of \ outdegrees)}{n(n-1)} $$

## node의 중심성

한 node가 네트워크에서 얼마나 중요한지를 측정하는 방식에는 여러 개가 존재한다.

### Closeness Centrality(근접 중심성)

위의 예시에서 구한 최단 거리 행렬을 다시 가져오자. 이를 이용해서 node $$i$$의 근접 중심성을 구할 수 있다.
$$
D=
  \begin{bmatrix}
    0 & 1 & 1 & 2 & 1 \\
    2 & 0 & 1 & 2 & 3 \\
    1 & 2 & 0 & 1 & 2 \\
    \infty & \infty & \infty & 0 & \infty \\
    1 & 2 & 2 & 3 & 0
  \end{bmatrix}
$$

$$d_{ij}$$을 두 node $$i$$ 와 $$j$$ 사이의 최단거리라고 하자.

> **(DEFINITION I)** node $$i$$의 근접 중심성 : $$C(i) = \frac{n-1}{\sum_{j \neq i} d_{ij}}$$

이는 node $$i$$로부터 다른 node 사이의 최단거리의 산술평균의 역수 개념이다. 즉, 평균 거리가 짧을수록 근접 중심성은 커진다. 하지만 이러한 정의는 하나라도 두 node 사이의 경로가 존재하지 않는다면 0이 되어 버린다.($$d_{ij}=\infty$$이므로) 따라서 이를 보완한 두 번째 정의로 보통 많이 사용된다.

> **(DEFINITION II)** node $$i$$의 근접 중심성 : $$C(i) = \frac{1}{n-1}\sum_{j \neq i} 1/d_{ij}}$$

이는 node $$i$$로부터 다른 node 사이의 최단거리의 조화평균의 역수 개념이다. 이를 사용하면 $$d_{ij}=\infty$$이여도 하나의 요소만 0으로 되는 것으로 끝난다. $$1/d_{ij}$$의 의미는 둘 사이의 유사도를 의미하므로 이는 다른 node들에 대한 평균적인 유사도이다. 예를 들어 두 번째 정의로 $$M1$$의 근접 중심성을 구해보자.

> $$C(M1)=\frac{1}{5-1} (\frac{1}{1}+\frac{1}{1}+\frac{1}{2}+\frac{1}{1})=\frac{3.5}{4} = 0.875 $$

### Betweenness Centrality(중개 중심성)

중개 중심성은 말 그대로 두 node 사이의 경로 중에 해당 node를 지나가는지 여부 즉, **해당 node가 두 node를 중개하는지 여부** 로 구한다. $$g_{ij}$$를 $$i$$에서 $$j$$로 가는 최단경로의 수, $$g_{ivj}$$는 그 중에서 $$v$$를 거치는 경로의 수라고 하자.

> **(DEFINITION)** node v의 중개 중심성 : C_{B}(v)= \sum_{i \neq v} \sum_{j \neq v,i} \frac{g_{ivj}}{g_{ij}}

<center><a href="https://imgur.com/r61HEKU"><img src="https://i.imgur.com/r61HEKU.png" width="300px" height="300px" title="source: imgur.com" /></a></center>

위의 네트워크에서 node 1의 중개 중심성을 구해 봅시다. 일단, node 2,3,4,5에 대해 어느 node로 최단거리로 가려고 해도 반드시 node 1를 지나게 됩니다. 그러므로 node 본인과 node 1를 제외한 나머지 node 11개에 대해서 모두 $$\frac{1}{1}$$입니다. 총 44개. 그리고 나머지 node 6-13이 node 2-5로 가기 최단거리로 가기 위해서는 반드시 node 1을 지나야 됩니다. 따라서 $$8(node \ 6-13) \times 4(node \ 2-5) = 32$$ 입니다. 결론적으로 node 1의 중개 중심성은 $$44+32=76$$이 되겠습니다.

### Eigenvector Centrality(고유벡터 중심성)

벡터 $$c$$ 를 node의 중요도라고 한다면, node i의 중요도는 그와 연결되어 있는 모든 node들의 중요도 합에 비례합니다. 수식으로 표현하면

> $$c_{i} \propto \sum_{j=1}^{n} a_{ij}c_{j}$$이며 $$Ac = \lambda c$$

eigenvector, eigenvalue의 개념을 아시는 분들은 쉽게 $$c$$가 adjacency matrix A의 eigenvector이고, $$\lambda$$가 eigenvalue임을 알 수 있습니다. 이러한 수식은 아래와 같은 사실을 내포하고 있습니다.

* 한 node의 중요도는 연결되어 있는 다른 node로 이전된다.
* 중요한 node에 연결된 node가 중요하다.
* Google의 PageRank 알고리즘이 이러한 사실과 관련됩니다.(<a href="https://kangyeol-kim.github.io/network/2018/02/13/pagerank/">PageRank에 관한 포스팅</a>)

## 가중 네트워크(weighted network)와 2부 네트워크(bipartite network)

### 가중 네트워크(weighted network)

연결선의 강도가 모두 동일하면 상관없지만 많은 경우 연결성의 강도는 다릅니다. 가중 네트워크는 연결선에 가중치가 붙은 네트워크입니다. 학자들 사이의 논문 인용 수(빈도), 학교 내 친구들 사이의 친근함의 정도(강도)가 바로 가중치가 됩니다. 가중 네트워크를 행렬 $$W=(w_{ij})$$로 나타낼 때, node $$i$$와 $$j$$ 간 거리 $$g_{ij}$$는 $$w_{ij}$$의 역수로 간주할 수 있습니다. 즉, 가중치가 클수록 두 점이 가깝다고 생각합니다.

<center><a href="https://imgur.com/ndvzMYk"><img src="https://i.imgur.com/ndvzMYk.png" width="300px" height="300px" title="source: imgur.com" /></a></center>

```r
library(igraph)
A <- matrix(c(0,8,1,0,3,
              8,0,4,0,0,
              1,4,0,2,0,
              0,0,2,0,18,
              3,0,0,18,0),byrow=T,nrow=5)
NAMES <- c("M1","M2","M3","M4","M5")

net=graph.adjacency(A,mode="undirected",weighted=TRUE,diag=FALSE)
plot.igraph(net,vertex.label=NAMES, layout=layout.fruchterman.reingold,
            edge.color="black",edge.width=E(net)$weight, vertex.size=25)
```
### 2부 네트워크(bipartite network)

2부 네트워크는 node들 사이의 관계가 직접적으로 연결되기 보다는 다른 층(layer)에 있는 node를 통해 연결되는 구조의 네트워크이다. 예컨대, $$n$$명의 개인에 대하여 그들이 가입한 $$m$$개의 동아리를 조사한 경우 입니다. 어느 두 명이 같은 동아리에 속하여 있다면, 연결성이 있다고 할 수 있습니다. 2부 네트워크는 $$n \times m$$꼴 행렬로 나타낼 수 있습니다. 6명의 학생이 있고, 3개의 동아리가 있다고 했을 때 이 행렬($$A$$)은 아래와 같이 나타낼 수 있습니다. (가입=1, 미가입=0)

|학생| 동아리-1| 동아리-2| 동아리-3|
| 1 | 1 | 0 | 0 |
| 2 | 1 | 1 | 0 |
| 3 | 1 | 0 | 0 |
| 4 | 0 | 1 | 0 |
| 5 | 1 | 1 | 1 |
| 6 | 0 | 0 | 1 |

만약 $$A A^{t}$$를 계산하고 대각요소는 모두 0으로 치환하여 $$R_{(n \times n)}$$를 얻는다면, 그 성분 $$r_{ij}$$는 구성원 $$i$$와 $$j$$가 동시에 가입한 조직의 수를 의미합니다. 위의 예에서 곱하고 0으로 치환하면

$$
  \begin{bmatrix}
    1 & 0 & 0 \\
    1 & 1 & 0 \\
    1 & 0 & 0 \\
    0 & 1 & 0 \\
    1 & 1 & 1 \\
    0 & 0 & 1
  \end{bmatrix}
  \times
  \begin{bmatrix}
    1 & 1 & 1 & 0 & 1 & 0 \\
    0 & 1 & 0 & 1 & 1 & 0 \\
    0 & 0 & 0 & 0 & 1 & 1
  \end{bmatrix}
  \overset{0}{\sim}
  \begin{bmatrix}
    0 & 1 & 1 & 0 & 1 & 0 \\
    1 & 0 & 1 & 1 & 2 & 0 \\
    1 & 1 & 0 & 0 & 1 & 0 \\
    0 & 1 & 0 & 0 & 1 & 0 \\
    1 & 2 & 1 & 1 & 0 & 1 \\
    0 & 0 & 0 & 0 & 1 & 0
  \end{bmatrix}
$$

학생 2와 5가 동시에 가입된 동아리가 2개임을 알 수 있습니다. 이는 A 그리고 R 모두에서 확인할 수 있습니다.
