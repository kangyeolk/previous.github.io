---
title: Chain Rule
category: Some Concepts
tag: Mathmatics
---

**Chain Rule** gradient descent 알고리즘의 학습 방식인 Backpropagation에 기본이 되는 수학적 내용입니다. 이 글에서는 Chain Rule에 대해서 소개하고 더 나아가서 이를 증명하겠습니다.

**THEOREM (chain rule)**

함수 $$y=f(u)$$와 $$u=g(x)$$가 미분가능하면 합성함수 $$f \circ g$$도 미분가능하고 $$(f \circ g)^{'}(x)=f^{'}(g(x))g^{'}(x)$$이다.


**PROOF**

$$u=g(x)$$가 미분 가능하므로 $$\lim_{\Delta x \to 0} \frac{\Delta u}{\Delta x}=g'(x)$$ 이다. <br />
여기서 $$\epsilon_{1}=\frac{\Delta u}{\Delta x}-g'(x)$$라 두면 $$\Delta u = (\epsilon_{1}+g'(x))\Delta x$$이다. 여기서 $$\lim_{\Delta x \to 0} \epsilon_{1}=0$$이고 $$\Delta x \to 0$$이면 $$\Delta u \to 0$$임을 알 수 있다. <br />
그리고 $$y=f(u)$$도 미분 가능하므로 $$\lim_{\Delta u \to 0} \frac{\Delta y}{\Delta u}=f'(u)$$ 이다. <br />
여기서 $$\epsilon_{2}=\frac{\Delta y}{\Delta u}-f'(u)$$라고 한다면 $$\Delta y = (\epsilon_{2}+f'(u))\Delta u =\{\epsilon_{2}+f'(g(x))\}\{\epsilon_{1}+g'(x)\}\Delta x$$ $$\cdots (1)$$이고 $$\lim_{\Delta u \to 0} \epsilon_{2}=0$$이다. <br />
이제 $$y=f \circ g(x)=f(g(x))$$에서 $$\frac{dy}{dx}=\lim_{\Delta x \to 0} \frac{\Delta y}{\Delta x}=\{\epsilon_{2}+f'(g(x))\}\{\epsilon_{1}+g'(x)\}$$이다.($$(1)$$에 의해) 한편, $$\Delta x \to 0(\equiv \Delta u \to 0)$$일 때 $$\epsilon_{1}, \epsilon_{1} \to 0 $$이므로 위의 식은 곧 $$f'(g(x))g'(x)$$이다.

**EXAMPLE**

Q. $$y=(x^{3}-1)^{100}$$을 Chain Rule을 사용해 미분하여라.

A. $$f(x)=x^{100}, g(x)=x^{3}-1, f'(x)=100x^{99}, g'(x)=3x^{2}$$이므로 $$f'(g(x))g'(x)=100(x^{3}-1)^{99} \times 3x^{2}$$
