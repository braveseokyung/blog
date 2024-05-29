---
title : 'Convex sets (1)'
date : 2024-05-29T21:42:55+09:00
draft : false
authors : ["braveseokyung"]
description : convex sets
categories : ["courses"]
series : ["Convex-Optimization"]
series_weight : 1
toc:
    auto: false
---

## 2.1 Affine and convex sets
### 2.1.1 Lines and line segments
#### line 
두 point를 연결하는 set of points


[전제] $x_1 \neq x_2$ , $x_{1},x_{2}\in \mathbb{R}^n$ , $\theta\in R$ (x1,x2가 1차원 vector) 일 때 집합
$$\{\theta x_1+(1-\theta)x_2\}$$


$y=\theta x_1+(1-\theta x_2)$ 로 두면 다음과 같이 $x_2$를 기준으로 표현할 수 있다
$$y=x_2+\theta(x_1-x_2)$$
{{<image src="Pasted image 20240405222644.png" width="400">}}
[해석]

$\theta=0$일때의 reference point $x_2$와 $x_1-x_2$의 방향의 sum

$\theta$는 scale parameter로, y가 $x_2$로부터 $x_1$을 향해 얼마나 갈 것인지의 fraction

#### line segment
두 point 를 잇는 선분
line의 coefficient가 0-1 사이인 경우
$$\{\theta x_1+(1-\theta)x_2|\theta\in [0,1]\}$$

$0 \leq \theta \leq 1$ : $\theta$가 커질수록 x1을 향해 가까워지고,

$\theta>1$ : x1의 beyond에 존재

### 2.1.2 Affine sets
#### Affine sets
쉽게 생각해서 두 점을 찍고 그 두 점을 잇는 직선을 쭉 그었을 때 set을 벗어나지 않으면 affine

set A의 임의의 두 점 $x_1,x_2$가 만드는 직선의 모든 점이 A 안에 들어있으면 Affine set이다

다른말로는, C가 C 안의 임의의 두 point에 대해 그 두 point의 linear combination-coefficient의 합이 1이되는-도 포함하고 있으면 affine

[전제] 모든(for any) $x_1,x_{2}\in C$, 모든 $\theta \in \mathbb{R}$  에 대해
$$\theta x_1+(1-\theta)x_{2}\in C$$
이면 C가 affine set

* example
	* 직선, 평면은 다 affine set
	* 어떤 linear equation의 ==solution set== $S=\{x|Ax=b\}$
	* half plane은 affine set이 아니다

2개 이상의 점으로 generalization

affine set은 자신한테 속하는 모든 점들의 모든 affine combination도 포함한다
즉, $x_1,...,x_{k}\in C$, $\theta_{1}+...+\theta_{k}=1$ 일때
point $\theta_{1}x_1+...+\theta_{k}x_k$ 또한 C에 포함된다

또 다른 expression : subspace + offset으로 표현될 수 있다

C의 subspace V와 C에 속하는 임의의 점 $x_0$에 대해,
$$C=V+x_0=\{v+x_{0}|v \in V\}$$
affine set C의 dimension은 subspace $V=C-x_0$의 dimension이다

##### affine hull
어떤 집합 $C\subset \mathbb{R}^n$ 의 점들의 모든 affine combination의 set을 affine hull이라고 한다

$aff C={\theta_{1}x_1+...+\theta_{k}x_k|x_1,...,x_{k}\in C, \theta_1+...+\theta_k=1}$

> affine hull은 C를 포함하는 가장 작은 affine set이다!

### 2.1.4 Convex sets

#### Convex Set
쉽게 생각해서 어떤 집합 안에 두 점을 찍고 두 점을 잇는 선분을 쭉 그었을 때, 집합에 포함되면 convex set
임의의 $x_1,x_2$ 에 대해 두 점을 잇는 선분을 모두 포함하는 집합

[전제] for any $x_1,x_{2}\in C$, for all $\theta \in [0,1]$  
$$\theta x_1+(1-\theta)x_{2}\in C$$
* examples
{{<image src="스크린샷 2024-04-06 오후 5.19.21.png">}}
    convex, not convex, not convex
  
  boundary만 있고 안이 비어있는 경우도 다 convex
  
  Discrete set: 점이 1개면 convex set, 2개 이상~countable many이면 not convex

모든 affine set은 convex 하다

if affine then convex

affine이 convex보다 strict한 개념

affine set $\subset$ convex set

#### Linear and Convex combinations
convex combination은 coefficient가 모두 0보나 크거나 같고 그 합이 1인 linear combination

##### linear combination(affine combination)
for some $\theta_{i}\in \mathbb{R}$,
$$x=\theta_{1}x_1+\theta_{2}x_2+...+\theta_{n}x_n$$
##### convex combination
for some 1) $\theta_{i}\geq 0$, 2) $\sum \theta_i=1$
$$x=\theta_{1}x_1+\theta_{2}x_2+...+\theta_{n}x_n$$
point들의 mixture, weighted average로 볼 수 있다

{{< admonition note "linear(affine) combination" >}}
affine set-convex set의 관계와 비슷한 것 같다

그래서 linear combination을 affine combination이라고도 부르는구나!

affine set의 확장처럼, convex set도 set에 포함되는 point들의 모든 convex combination을 포함하고 있는 set으로 볼 수 있다!
{{< /admonition >}}

#### convex hull
어떤 set의 convex hull이란,
그 set의 점들의 모든 convex combination을 포함하고 있는 set

> 어떤 set을 포함하는 smallest convex set!
만약 그 set이 이미 convex set이면, conv(C)=C 즉 자기자신!

conv $C=\{\theta_{1}x_1+...+\theta_{k}x_{k}|x_{i}\in C, \theta_{i}\geq 0, i=1,...,k, \theta_1+...+\theta_k=1\}$
{{<image src="스크린샷 2024-04-06 오후 5.38.43.png">}}

[further 해석]
convex combination의 아이디어는 infinite sum, integral로 generalized 될 수 있고,

가장 general하게는 **probability distribution**으로 볼 수 있다

[전제] $\theta_{i}\geq 0$, $i=1,2,...,$ $\sum\limits_{i=1}^{\infty}\theta_i=1$, $x_1,x_{2},... \in C$, C is convex

* infinite sum  $$\sum\limits_{i=1}^{\infty}\theta_{i}x_{i}\in C$$
  if C converges
* integral, probability distribution

  $p:\mathbb{R}^{n}\rightarrow \mathbb{R}$, $p(x)\geq 0$ for all $x\in C$, $\int_{C}p(x)dx=1$,
  $$\int_{C}p(x)x dx \in C$$
  이 뜻은, C가 convex이고 x가 C에 속하는 random vector이면, $\mathbb{E}(x)\in C$

  간단하게 생각하면, random variable x가 $x_1,x_2$ 의 값만 가질 수 있고

  $p(x=x_1)=\theta,p(x=x_2)=1-\theta$ 이면 $\mathbb{E}x=\theta x_1+(1-\theta)x_2$

