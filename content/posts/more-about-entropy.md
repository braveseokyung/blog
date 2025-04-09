---
title : 'Entropy 총정리(joint, conditional, mutual information)'
date : 2025-04-09T22:20:21+09:00
draft : false
authors : ["braveseokyung"]
description : Joint entropy, conditional entropy, chain rule of entropy, mutual information - entropy 총정리!
categories : ["courses"]
series : ["information-theory-and-inference"]
series_weight : 2
toc:
    auto: false
---
<!--more-->

## joint entropy of X,Y
$$H(X,Y)=\sum_{xy\in\mathcal{A}_X\mathcal{A}_Y}P(x,y)\log\frac{1}{P(x,y)}$$
만약 random variable이 independent하면 entropy는 additive하다
$$H(X,Y)=H(X)+H(Y) \iff P(x,y)=P(x)P(y)$$
## conditional entropy
conditional entropy of X given $y=b_k$는 
probability distribution $P(x|y=b_k)$ 의 entropy와 같다
$$H(X|y=b_k)\equiv\sum\limits_{x\in\mathcal{A}_x}P(x|y=b_k)\log\frac{1}{P(x|y=b_k)}$$
특정 y의 value가 아닌 random variable Y 전체에 대한 X의 conditional probability, 
즉 conditional entropy of X given Y는
**conditional entropy of X given y의 average와 같다**
$$H(X|Y)\equiv \sum_{y\in\mathcal{A}_Y}P(y)H(X|Y=y)=\sum_{y\in\mathcal{A}_Y}P(y)\sum_{x\in\mathcal{A}_{x}}P(x|y)\log\frac{1}{P(x|y)}$$
$$=\sum_{y\in\mathcal{A}_Y}\sum_{x\in\mathcal{A_x}}P(y)P(x|y)\log\frac{1}{P(x|y)}=\sum_{y\in\mathcal{A}_Y}\sum_{x\in\mathcal{A_x}}P(x,y)\log\frac{1}{P(x|y)}$$
$$=\sum_{xy\in\mathcal{A}_X\mathcal{A}_Y}P(x,y)\log\frac{1}{P(x|y)}$$
y를 알 때 x에 남아있는 average uncertainty를 measure한다고 해석할 수 있다
## marginal entropy of X
conditional entropy와 구분하기 위해 쓰는 용어로 그냥 X의 entropy와 같다
## Chain rule for information content
information content에 대해 conditional probability와 유사한 chain rule이 존재
$$h(x,y)=h(x)+h(y|x)$$
b/c product rule for probabilities $P(x,y)=P(x)P(y|x)$
$$\log\frac{1}{P(x,y)}=\log\frac{1}{P(x)}+\log\frac{1}{P(x|y)}$$
**x and y의 information content는 x의 information content + x가 given일때 y의 information content로 해석할 수 있다**
## Chain rule for entropy
joint entropy에 대해서도 chain rule이 성립
$$H(X,Y)=H(X)+H(Y|X)=H(Y)+H(X|Y)$$
**X,Y의 uncertainty는 X의 uncertainty + X가 given일 때 Y의 uncertainty로 해석할 수 있다**
## Mutual information
mutual information between X and Y는
$$I(X;Y)\equiv H(X)-H(X|Y)$$
**y의 값을 알았을 때 x의 average reduction in uncertainty를 measure**
그냥 H(X)를 구해도 Y가 상관없다고는 말 할 수 없음.
given Y라는 건 Y가 deterministic하다는 것이고 이건 X랑만 관련있다는(X에 대해서만 random하다는?) 뜻
**the average amount of information that y conveys about x**, 를 의미(vice versa도 같음)
### properties
1) $I(X;Y)=I(Y;X)$ commutative
2) $I(X;Y)\geq0$
3) $H(X,Y)=I(X;Y)+H(X|Y)+H(Y|X)$
4) $I(X;Y)=H(X)+H(Y)-H(X,Y)$

{{< admonition type=note title="Relationship between joint information, marginal entropy, conditional entropy and mutual entropy" >}}
{{< /admonition >}}
{{<image src="entropy-mutual-information.png">}}


## conditional mutual information
conditional mutual information between X and Y given $z=c_k$
$$I(X;Y |z=c_k)=H(X|z=c_k)−H(X|Y,z=c_k)$$

conditional mutual information between X and Y given Z
average over z of above conditional mutual information
$$I(X;Y |Z)=H(X|Z)−H(X|Y,Z)$$

## Information conveyed by a channel
consider how much information can be communicated through channel
* operation 관점: communicated한 모든 bit이 negligible probability of error 로 recover할 수 있는 방법 찾기
* mathematical 관점: measure how much information the output conveys about the input by **mutual information**
  $$I(X;Y)\equiv H(X)-H(X|Y)=H(Y)-H(Y|X)$$
  
