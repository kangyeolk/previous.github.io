---
title: Basis & Gram-Schmidt Orthogonalization Process
category: Linear Algebra
tag: Linear Algebra
---

### Linearly Independence

벡터는 $x_{1}, ..., x_{n}$이 linearly independent하다는 것은 다음과 같은 조건을 만족하는 것 입니다:

> $k_{1}x_{1}+k_{2}x_{2}+\cdots+k_{n}x_{n} \Leftrightarrow k_{1}=k_{2}=\cdots=k_{n}$

이는 다른 $n-1$개의 벡터를 통해서 나머지 벡터를 표현할 수 없는 것을 의미합니다. 즉, 어떠한 벡터도 다른 벡터들에 의존적(dependent)이지 않습니다.

### Basis

$n$ 차원 공간의 벡터는 $n$개의 linearly independent 한 $n$차원 벡터로 표현될 수 있습니다. 이러한 벡터 집합을 공간의 **basis** 라고 하고, 공간을 **span** 한다고 표현합니다. basis 집합 $\{x_{1},...,x_{n}\}$중에 다음과 같은 조건을 만족하는 basis를 **standard basis** 라고 합니다.

> $e_{1}=(1,0,0,\cdots,0), e_{2}=(0,1,0,\cdots,0), e_{3}=(0,0,1,\cdots,0), e_{n}=(0,0,0,\cdots,1)$

standard basis $\{e_{1},...,e_{n}\}$은 orthonormal basis의 하나의 예입니다. 벡터들이 orthonormal 하다는 것은 다음의 조건일 때 입니다:

> $e_{i} \cdot e_{j}=0 (i \neq j), \lvert\lvert e_{i} \lvert\lvert = 1$

### Gram-Schmidt Orthogonalization

Gram-Schmidt Orthogonalization은 주어진 basis $B = \{u_{1},u_{2},\cdots,u_{n}\}$으로부터 orthogonal한 basis $B'=\{v_{1},\cdots,v_{n}\}$를 만들어내는 과정입니다. 이 basis를 normalizing한다면 orthonormal한 basis $B''=\{w_{1},w_{2}, \cdots, w_{n}\}$를 얻을 수 있습니다.

먼저 벡터 두 개를 변형하는 경우를 생각해 보면 다음과 같은 그림으로 이해할 수 있습니다:

<center><a href="https://imgur.com/l5KpvNI"><img src="https://i.imgur.com/l5KpvNI.png" width="500px" height="350px" title="source: imgur.com" /></a></center>

* $v_{1}=u_{1}$으로 놓습니다.
* $v_{1}$과 아직 orthogonal하지 않은 벡터 $u_{2}$는 이와 수직은 벡터 $v_{2}$에 의해 나타낼 수 있습니다: $u_{2}=v_{2}+proj_{v_{1}}u_{2}$ 따라서
> $v_{2}=u_{2}-proj_{v_{1}}u_{2}=u_{2}-(\frac{u_{2}\cdot v_{1}}{v_{1} \cdot v_{1}})v_{1}$

이를 계속 확장해서 생각하면 마찬가지로 일단 $v_{1}, v_{2}$는 orthogonal하니, 아직 두 벡터에 orthogonal하지 않은 벡터 $u_{3}$를 두 벡터에 수직인 $v_{3}$로 나타낼 수 있습니다: $u_{3}=v_{3}+proj_{v_{1}}u_{3}+proj_{v_{2}}u_{3}$따라서

> $v_{3}=u_{3}-proj_{v_{1}}u_{3}-proj_{v_{2}}u_{3}$

이를 일반화 하면:

> $v_{k}=u_{k}-\sum_{j=1}^{k-1}proj_{u_{j}}(v_{k})$

로 일반화 시킬 수 있습니다. 그리고 $e_{k}=\frac{v_{k}}{\lvert\lvert v_{k} \lvert\lvert}$를 통해서 orthonormal한 standard basis를 만들 수 있습니다.
