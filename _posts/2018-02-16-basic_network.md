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

<center><a href="https://imgur.com/mR3WBdF"><img src="https://i.imgur.com/mR3WBdF.png" width="400px" height="400px" title="source: imgur.com" /></a></center>

이와 같은 네트워크 구조를 위에서 정의된 두 가지 표현 방식으로 나타낼 수 있습니다. adjacency matrix 형식으로 나태낸다면:

$$
\[
A=
  \begin{bmatrix}
    0 & 1 & 1 & 0 & 1 \\
    0 & 0 & 1 & 0 & 0 \\
    1 & 0 & 0 & 1 & 0 \\
    0 & 0 & 0 & 0 & 0 \\
    1 & 0 & 0 & 0 & 0
  \end{bmatrix}
\]
$$

그리고 edge list 꼴로 나타낸다면:

$$
\[
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
\]
$$

## 네트워크의 특징

### 최단거리 행렬

node간 가장 짧은 경로의 길이는 두 node 사이의 거리를 잴 때 필요한 행렬 입니다. 예시로 나온 네트워크에서 $$M2 \to M5$$의 최단경로는 $$M2 \to M3 \to M1 \to M5$$로 최단거리는 3입니다. 둘 사이의 경로가 존재하지 않는 경우에는 $$\infty$$로 거리를 표기합니다. 위의 예시에서 최단 거리 행렬:

$$
\[
D=
  \begin{bmatrix}
    0 & 1 & 1 & 2 & 1 \\
    2 & 0 & 1 & 2 & 3 \\
    1 & 2 & 0 & 1 & 2 \\
    \infty & \infty & \infty & 0 & \infty \\
    1 & 2 & 2 & 3 & 0
  \end{bmatrix}
\]
$$

### 네트워크의 방향성

네트워크를 나타내는 행렬이 대칭적이면 무방향 네트워크(undirected network)이고 비대칭적이면 방향 네트워크(directed network)이다. 즉, 무방향 네트워크에서는 $$i \to j \equiv j \to i$$이다. 하지만 방향 네트워크에서는 그렇지 않은 예가 있다.

### 네트워크의 밀도

**네트워크의 밀도는 가능한 총 연결선 수 대비 총 연결선의 수이다.** 가능한 총 연결선 수는 방향성 네트워크의 경우 $$_n P_2 $$ 이고, 무방향성 네트워크의 경우 $$ _n C_2 $$이다. 그리고 총 연결선 수는 중복되지 않게 세주어야 한다. 즉, in-degree 혹은 out-degree 둘 중 하나만 선택해서 사용하면된다. 예시의 경우에는 총 연결선 수가 7개이고, 가능한 총 수가 20개(방향성 존재) 이므로 밀도=7/20이다.

> $$density = \frac{\sum_{i=1}^{n} \#(number \ of \ in-degrees)}{n(n-1)} = \frac{\sum_{i=1}^{n} \#(number \ of \ out-degrees)}{n(n-1)} $$

## node의 중심성

한 node가 네트워크에서 얼마나 중요한지를 측정하는 방식에는 여러 개가 존재한다.

### Closeness Centrality(근접 중심성)

위의 예시에서 구한 최단 거리 행렬을 다시 가져오자 

### Betweenness Centrality(중개 중심성)

### Eigenvector Centrality(고유벡터 중심성)
