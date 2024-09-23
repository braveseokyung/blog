---
title : '[논문 리뷰] Accept the Modality Gap : An Exploration in Hyperbolic Space'
date : 2024-09-24T00:16:26+09:00
draft : true
authors : ["braveseokyung"]
description : CVPR 2024 Highlight paper인 Accept the Modality Gap An Exploration in Hyperbolic Space 논문을 읽고 정리한 내용입니다.
categories : ["paper-review"]
toc:
    auto: true
---

<!--more-->

## 논문 정보
- CVPR 2024 Highlight
- Keyword : multimodal learning, modality gap, hyperbolic representation learning
- [paper](https://www.google.com/url?sa=t&source=web&rct=j&opi=89978449&url=https://openaccess.thecvf.com/content/CVPR2024/papers/Ramasinghe_Accept_the_Modality_Gap_An_Exploration_in_the_Hyperbolic_Space_CVPR_2024_paper.pdf&ved=2ahUKEwjax__SrtmIAxXSh1YBHYrfDq4QFnoECBkQAQ&usg=AOvVaw26x5QLHl4rCwV3Fe1tYq97)

## Abstract

- 최근 연구로 hierarchical한 feature representation의 학습에 hyperbolic space의 포텐셜이 주목받고 있음
- single modality에 대해 hyperbolic space를 leveraging하는 것은 어느 정도 진전이 있었지만, multimodal settting에서는 under explored
- 최근 연구
Euclidean multimodal learning 테크닉을 hyperbolic space에 적용(geodesic distance based contrastive loss)
    
    → 이 논문은 이러한 spatial proximity based contrastive loss 가 latent space에서의 hierarchy를 방해한다는 이론적/경험적 반박 제시
    
    <aside>
    💡 geodesic distance based contrastive loss  = = spatial proximity based contrastive loss
    
    </aside>
    
- 이 연구
    
    (latent space 상에서의 hierarchy를 방해한다는) 문제를 해결하기 위해, 
    
    - **cross-modal representation이 text와 image 사이의 inherent modality gap을 accept해야 함을 주장**
    - **spatial proximity를 enforce하지 않는 novel한 cross-modal similarity를 제안**
- 이 연구의 의의
    - unimodal hierarchy를 보존하면서 두 모달리티를 잘 align함
    - downstream task에서 더 좋은 latent structure가 나타남과 동시에, text-to-image, image-to-text retrieval task에서 성능이 superior

## Introduction

- natural world에서 hierarchical한 structure는 fundamental한 요소
- image-text representation으로 natural world를 총체적으로 보고자 하는 Multimodal foundation model CLIP은, **Euclidean and spherical geometry**를 사용
    - 하지만 이런 방법은 hierarchical한 information을 capture하는데에 있어 intrinsic geometric constraint가 존재
- hierarchical structure에서 continuous approximation이 가능한 hyperbolic space가 등장
     
    > - MERU(2023)
    > - Hyperbolic self-paced learning for self-supervised skeleton-based action representations(2023)
    > - Hyperbolic contrastive learning for visual representations beyond objects(2023)
    > - Hyperbolic image embeddings(2020)
    > - Hypliloc: Towards effective lidar pose regression with hyperbolic fusion (2023)
- hyperbolic space에서의 hyperbolic embedding은 unimodal setting에서는 많이 탐구되었지만, multimodal에서는 under explored
- 지금의 multimodal model들은 cross-modal embedding의 similarity를 shared embedding space에서의 spatial proximity로 계산 → spatial proximity based contrastive loss
    - modality 간 matching concept을 cluster, non-matching을 밀어내는 방식
    - 널리 사용되지만, image-text modality 사이의 alignment가 ill-posed problem
    - **modality gap**이라는 문제가 발생(modality 간의 misalignment)

**modality gap**

주장: modality gap의 원인은 visual/linguistic data의 1) **representational nature**의 intrinsic한 차이와 2) information content의 차이라고 생각함

Text

- 구조적 syntax, semantically rich → abstract concept과 relationship을 **explicit**하게 담고있음

Image

- concrete instances of the world
- 복잡한 scene과 hierarchical한 relationship을 visual cue를 통해 **implict**하게 표현
- distance metric을 minimize하려는 모델들은 text-image사이의 nuanced association을 capture하기 어려워 할 수 있음
- 그 결과 rich, one-to-many correspondence from text-to-image를 간과하는 oversimplified alignment를 초래할 수 있음

**meru (최근 연구)**

- hyperbolic space에서 unified image-text representation을 배우기 위해 entailment loss를 사용
- entailment loss는 cross-modal hierarchy(text description이 image보다 더 generic하다)를 이용
- 하지만 여전히 spatial proximity based contrastive loss의 비중이 크고, Euclidean/spherical과 같은 pitfall이 발생
- 이 논문에서 modality 간의 spatial proximity를 minimize하는 strategy는 text/image embedding 모두의 hierarchical representation에 나쁘게 영향을 미침을 이론적/실험적으로 보일 것임

**이 논문에서는~**

- image와 text embedding사이의 modality gap을 accept하는 **novel loss function**을 소개
- pairwise similiarity를 평가하는데 기존의 latent space에서의 spatial proximity → hyperbolic angle-based metric
- 이 논문의 loss function은 hierarchy를 더 잘 보일 뿐만 아니라 hyperbolic space의 expanse를 better utilize

**논문 contribution**

- hyperbolic space에서 image/text를 align하는데 쓰이는 spatial proximity based contrastive loss가 hierarchy를 보존하는데 detrimental함을 보임 + meru의 contrastive loss 와 entailment loss combine 한 게 fundamental mismatch가 있음을 보임
- 위의 문제를 modality gap을 accept함으로써 해결하는 novel objective function을 제안
    - hierarchy 보존 + 두 모달리티를 align할 때 hyperbolic space를 better utilize
- CLIP에서 hyperbolic space로의 최근 extension이 near-Euclidan geometry with low curvature에서만 잘 작동하는 것임을 보임, 우리껀 high curvature space에서도 잘 작동함!
- text-to-image, image-to-text retrieval task에서 superior!

## Preliminaries

### Hyperbolic Spaces

- Hyperbolic space는 constance negative curvature을 가지는 Riemannian manifold( Euclidean, spherical space는 0 또는 constant positive curvature)을 가짐
- 그에 따르는 unique property : 평행선의 divergence(발산), boundary로 갈수록 exponential volume growth
- volume growth property → hyperbolic space를 hierarchical/graph structured data를 embedding할 수 있는 ideal한 후보로 만듦

#### Lorentz Model(Minkowski model)

- hyperbolic space를 표현하는 방법
- curvature c를 가지는 hyperbolic space $\mathbb{H}^d$ within (d+1)dimensional Euclidean space $\mathbb{R}^{d+1}$
    
    $$
    \mathbb{H}^d=\{x\in\mathbb{d+1}| ⟨x, x⟩_\mathbb{H}=-\frac{1}{c},x_0>0\}
    $$
    
     Lorentzian inner product :
    
    $$
    \langle{\mathbf{x},\mathbf{y}}\rangle_\mathbb{H}=-x_0y_0+\sum^d_{i=1}x_iy_i
    $$
    
    vector의 0번째 component는 time으로, 나머지는 space component로 취급됨
    
    time component를 space component로부터 계산
    
    $$
    x_{\text{time}}=x_0=\sqrt{\frac{1}{c}+\lVert\mathbf{x}_{\text{space}}\rVert}
    $$
    
    - Euclidean norm, $\mathbf{x}_{\text{space}}=\mathbf{x}_{1:d}$

**Geodesics**

hyperbolic space에서 point 간 가장 짧은 path(Euclidean geometry에서 straight lines(직선)에 대응)

- Lorentz model에서, geodesic은 intersection of planes through origin with the hyperboloid

- 두 point x,y 사이의 geodesic distance(가장 짧은 거리)는
    
    $$
    d_{\mathbb{H}}(\mathbf{x,y})=\sqrt{\frac{1}{c}} \cosh^{-1}(-c\langle{\mathbf{x,y}\rangle}_{\mathbb{H}})
    $$
    

**Tangent Spaces**

hyperbolic space에서 point $\mathbf{x}\in\mathbb{H}^d$ 에서의 tangent space는 그것을 locally approximate하는 Euclidean space

formula reference [ Learning continuous
hierarchies in the lorentz model of hyperbolic geometry](https://www.notion.so/Learning-continuous-hierarchies-in-the-lorentz-model-of-hyperbolic-geometry-3fa173b760f64b7bb7836cdac45fcc92?pvs=21) 

**Centroid of Points**

hyperbolic space에서 set of point의 centroid를 찾는 것은 Euclidean에서처럼 straightforward하지 않다→ Einstein midpoint    

- Eistein midpoint
    - Klein coordinate으로 변환해서 얻는 것이 더 쉬움
        
        $\mathbf{x}=(x_0,\mathbf{x}_{1:d})\in\mathbb{H}^d$ 가 hyperboloid model의 point라면, 다음 projection을 통해 Klein coordinate $\mathbf{k}\in\mathbb{K}^d$로 변환 가능
        
        ![스크린샷 2024-08-26 오후 3.12.08.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.12.08.png)
        
    
    - 그러면 centroid를 다음과 같이 나타낼 수 있다
        
        ![스크린샷 2024-08-26 오후 3.13.56.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.13.56.png)
        
        $\gamma_j$는 Lorentz factor
        
        ![스크린샷 2024-08-26 오후 3.14.15.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.14.15.png)
        

### Spatial Proximity based Contrastive Loss

contrastive loss의 핵심은 matching datapoint(positive pair)간의 거리를 최소화하고 non-matching datapoint(negative pair) 간의 거리를 최대화하는 것

Formally,

$\mathcal{B}$ : N image-text pair batch

$\mathbf{x}_i,\mathbf{y}_i\in\mathbb{R}^d$ : data sample $i$ 에 대응되는 text, image embedding

$\mathcal{K}:\mathbb{R}^{d\times d} \rightarrow\mathbb{R}$ : similarity function

$\tau>0$ : temperature parameter

![스크린샷 2024-08-26 오후 3.16.40.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.16.40.png)

- CLIP
    
    symmetric contrastive loss 사용
    
    ![스크린샷 2024-08-26 오후 4.05.26.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.05.26.png)
    
    - embedding unit으로 normalize
    - $\mathcal{K}$ : cosine similarity
    - CLIP embedding은 (d-1)-dimensional hypersphere에 있게 된다
    
    cosine similarity가 hypersphere에서 geodesic distance에 대해 inversely proportional하므로, CLIP은 **spatial proximity를 이용해서 cross-modal similarity를 구한다고 볼 수 있다**
    
- Hyperbolic space에서
    
    image와 text가 hyperbolic space에 있다고 ensure하고 geodesic distance로 proximity를 구하면 위의 contrastive loss를 hyperbolic space로 확장될 수 있다
    
    by setting $\mathcal{K}=-d_{\mathbb{H}}$
    
    MERU에서는 위의 loss와 entailment loss(**impose hierarchy that text entails images**)를 함께 사용 
    

## Interplay between Losses

- 앞서 설명했듯이, contrastive loss는 spatial proximity를 기반으로 text와 image embedding 사이의  modality gap을 bridge해주려고 함(spherical or hyperbolic)
    
    → 그래서 objective가 **modality에 관계없이** matching concept을 가까이 당기고, non-matching을 멀리 미는 것이 됨
    
    → 그래서 **cross-modal hierarchy를 encourage하기 위해 entailment loss**를 사용하는 것
    
    [Hyperbolic entailment cones for learning hierarchical embeddings](https://www.notion.so/Hyperbolic-entailment-cones-for-learning-hierarchical-embeddings-2e7150fdfb8c46749c2a9c9cc8f0862d?pvs=21) 에서 제안했다고 함
    
    목적 : text embedding이 그에 대응하는 image embedding에 비하면 더 abstract한 concept이라는 전제를 반영하기 위함
    
    이걸 위해 entailment loss는 text embedding에 대응되는 모든 image embedding을 text embedding으로부터 비롯된 cone 안으로 넣음
    
    Formally, text embedding($\mathbf{x}$)와 image embedding($\mathbf{y}$) 사이의 entailment loss는 
    
    $$
    L_{\text{entail}}(\mathbf{x},\mathbf{y})=\max(0,\text{ext}(\mathbf{x},\mathbf{y})-\text{aper}(\mathbf{x}))
    $$
    
    ext(,) : **text와 image embedding 사이의 exterior angle** → 이후 등장하는 이 논문의 loss에서도 사용됨($\alpha$)
    
    origin을 지나는 geodesic 위의 angle의 합은 $\pi$ 이므로
    
    ![스크린샷 2024-08-26 오후 4.44.55.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-26_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.44.55.png)
    
    aper() : cone의 aperture angle, space component의 norm이 커지면 aper()은 작아짐
    
    K : constant hyperparameter로, origin 근처에서 discontinuity of cones를 mitigate(완화)하기 위함
    
    ![스크린샷 2024-08-26 오후 4.47.53.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/92fc74fa-461e-480c-9f40-a79b3f0e5004.png)
    
- 이론적으로, contrastive loss는 모든 image embedding을 matching text embedding에 가깝게 push → **image embedding의 diversity를 reduce** → discourage hierarchies
    
    entailment loss는 이 collapse를 방지 못함 → contrastive & entailment loss combination은 hierarchy를 discourage(특히 image 도메인에서)
    
- in practice,
    
    text와 image의 natural variation과 stochasticity 때문에 contrastive loss로 train하면 latent space에서 diversity가 존재(이론적으로는 diversity reduce 였지만)
    
    nevertheless, image embedding에서 diversity가 존재하더라도  entailment objective가 violate될 가능성이 매우 높음을 보임 → 2차원 케이스에서 수식적으로 보이겠음
    

### Proposition 1.

- consider set of points $\{\mathbf{y}_i\}^{n}_{i=1}\in\mathbb{H}^2$, 한 점 $\mathbf{x}\in{\mathbb{H}^2}$
- 모든 i에 대해 hyperbolic distance $d_\mathbb{H}(\mathbf{x},\mathbf{y}_i)=r$이라고 가정

→ 그러면 to maximize sum $\sum^{n}_{i=1}\sum^{n}_{j=1} d_{\mathbb{H}}(\mathbf{y}_i,\mathbf{y}_j)$ (for n>1),

적어도 하나의 $\mathbf{y}_i$는 $\mathbf{x}$로부터 originate되는 entailment cone의 바깥에 있어야 한다.

직관적으로 생각해서, cat의 text embedding과 visually distinct한 cat image의 집합을 생각해보자.

contrastive loss에 의하면, image embedding은 전부 cat을 depict하기 때문에 text embedding으로부터 consistent(일관된) distance에 있어야만한다

하지만 image의 visual diversity 때문에 image embedding들이 maximally separated되는 것을 고려하지 않을 수 없다(만약 얘네가 떨어져있지 않으면 model은 forced to only learn abstract features-hierarcy를 보존하기 위해 꼭 필요한 visual cue를 놓칠 수 있는-)

→ 적어도 하나의 image embedding은 어쩔 수 없이 entailment cone 바깥에 떨어질 것 → contrastive loss와 entailment loss 사이의 fundamental discrepancy

MERU paper와 이 논문의 실험에서 실험적으로도 보임

다음 분석에서는 두 loss를 동시에 minimize하는 것이 hyperbolic space에서 image embedding의 구역을 rapid decrease할 필요가 있게 만듦(i.e. **collapse**)을 보일 것

이러한 constraint는 hyperbolic space를 full extent로 사용하는 것을 크게 limit, hierarchical structure establishment를 방해한다

### Proposition 2.

- consider set of points $\{\mathbf{y}_i\}^{n}_{i=1}\in\mathbb{H}^2$, 한 점 $\mathbf{x}\in{\mathbb{H}^2}$
- Let $d_{\max}=\max_i(d_{\mathbb{H}}(\mathbf{x},\mathbf{y}_i))$
- Let $\{\mathbf{y}_i\}^{n}_{i=1}$ : x로부터 originate되는 entailment cone 안에 있음

→ 그러면 $d_{\max}$가 감소하면, image embedding의 전체 구역이 exponentially 감소

직관적으로, cat 예시를 다시 생각해보자.

두 loss가 모두 converge하면, (entailment loss)모든 Image embedding이 entailment cone 안에 들어가고 contrastive loss가 이 embedding들로부터 text embedding까지의 maximum geodesic distance를 reduce

→ 이렇게 되면, image embedding이 차지하는 spatial domain이 축소됨

→ 줄어드는 공간에서 embedding끼리의 타이트한 clustering을 요구하므로 spatial proximity에 기반한 모든 형태의 image hierarchy를 유지하기 어렵다

## Our Approach : Accept the Modality Gap

위의 discussion에서 geodesic-based contrastive loss(entailment cone loss within hyperbolic spaces)의 inherent한 한계를 조명했다

여기서부터 얻을 수 있는 key insight는,

이러한 한계가 두 fundamentally distinct한 **modality의 gap을 spatially bridge하려는 과정**에서 일어난다는 점이다.

이걸 극복하기 위해, 이 논문의 novel한 가설은,

cross-modal concept의 similarity를 측정할 대안으로써 **modality gap을 acknowledge하는 objective function**을 사용한다면, 이러한 챌린지를 해결하는데 효과적일 것이다

가설을 기반으로

- 모든 matching concept가(unimodal/cross-modal) hyperbolic space의 origin으로부터 특정한 geodesic으로 align되는 unique constraint를 제안
- 위 constraint 하에 image가 text와 비교했을 때 더 specific하다는 것을 반영하여 image embedding을 textual counterpart보다 더 멀리 위치
- 그 결과, **geodesic 상에서 두 컨셉 간의 deviation angle**이 conceptual distance를 측정하는 새로운 metric이 됨.

**Figure 3**

![스크린샷 2024-08-27 오후 3.15.16.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.15.16.png)

예를 들자면, “cat”의 image embedding과 그에 대응되는 textual descriptor를 align한다고 생각해보자

objective : angle $\alpha$ 를 최소화하는 동시에 angle $\beta$ 를 최대화함으로써, 두 modality 사이의 optimal alignment를 얻는 것

non-matching concept에 대해서는, 반대로 $\alpha$ 최대화/ $\beta$ 최소화

- hyperbolic에서 angle $\alpha$, angle $\beta$ 수식:
    
    $$
    \alpha(\mathbf{x},\mathbf{y})=\text{ext}(\mathbf{x},\mathbf{y}), \beta(\mathbf{x},\mathbf{y})=\pi-\alpha(\mathbf{x},\mathbf{y})
    $$
    
- angle-based contrastive loss($L_{\text{angle}}$)
    
    ![스크린샷 2024-08-27 오후 3.37.07.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_3.37.07.png)
    
    - $L^{T\rightarrow I}$ : text to image contrastive loss
        
        위의 식에서 similarity function $\mathcal{K}$ 가 $\alpha,\beta$로 대체되고 $\mathcal{B}$ 의 matching pair에 대해 $\alpha$ 최소화, $\beta$ 최대화하고 나머지에 대해서는 반대로
        
    - asymmetric :  text가 image보다 generic하다는 걸 반영했기 때문
        
        entailment loss의 smoothed, contrastive version이라고 볼 수 있음 (비슷한 concept을 cone의 axis에 align하기 때문에)
        
        $L_{\text{angle}}$의 두 term이 mutual하게 satisfied되므로 objective간 mismatch가 없다($L_{\text{entail}}$ 은 혼자서는 유의미한 결과를 내지못함을 보였다 - 그래서 실험결과 이 논문 loss가 짱이래 ~)
        
- regularizer($L_{\text{centroid}}$)
    
    hyperbolic manifold에서 embedding의 distribution을 개선하기 위해 distribution level에서 regularizer를 사용
    
    text embedding의 centroid가 image embedding의 centroid보다 origin에서 가깝다고 impose
    
    Formally,
    
    $\mathbf{x}_e,\mathbf{y}_e$가 각각 text, image embedding의 Einstein midpoint(centroid)(Eq [6](https://www.notion.so/Accept-the-Modality-Gap-An-Exploration-in-Hyperbolic-Space-f2eafedf8884417e95d0bf5a0a4303e0?pvs=21))
    
    ![스크린샷 2024-08-27 오후 4.19.38.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.19.38.png)
    
    - norm의 뒤 term은 x,y 사이의 geodesic distance
    - Euclidean norm
    - p>q : image가 text보다 centroid가 origin으로부터 멂

- Final loss
    
    $$
    L_{\text{final}}=L_{\text{angle}}+\lambda L_{\text{centroid}}
    $$
    
    $\lambda>0$ 는 trade-off hyperparameter
    
- MERU의 parametrization을 따름
    - loss를 계산할 때 $\mathbf{x}=(x_{\text{time}},\mathbf{x}_{\text{space}})$
    - neural network를 사용해서 tangent space에서 Lorentz 모델의 space component만을 encode
    - hyperboloid를 project하는 exponential mapping은
        
        ![스크린샷 2024-08-27 오후 4.44.55.png](Accept%20the%20Modality%20Gap%20An%20Exploration%20in%20Hyperbol%20f2eafedf8884417e95d0bf5a0a4303e0/%25E1%2584%2589%25E1%2585%25B3%25E1%2584%258F%25E1%2585%25B3%25E1%2584%2585%25E1%2585%25B5%25E1%2586%25AB%25E1%2584%2589%25E1%2585%25A3%25E1%2586%25BA_2024-08-27_%25E1%2584%258B%25E1%2585%25A9%25E1%2584%2592%25E1%2585%25AE_4.44.55.png)
        
    - Eq [3](https://www.notion.so/Accept-the-Modality-Gap-An-Exploration-in-Hyperbolic-Space-f2eafedf8884417e95d0bf5a0a4303e0?pvs=21) 에 의해 time component를 얻을 수 있음

## Related Work

### Embeddings in Hyperbolic Spaces

- Hyperbolic geometry는 boundary로 향할 때 exponential volume expansion을 가능하게 하고 → hierarchical structure embedding을 가능하게 한다
    
    hierarchical relationship가 있는 곳에서 활용됨 : molecular structure/action recognition/3D data/text data/image
    
- hyperbolic embedding 학습 방법
    - standard deep learning layer(ResNet, Transformer)
    - hyperbolic projection[**Poincaré Embeddings for Learning Hierarchical Representations**](https://www.notion.so/Poincar-Embeddings-for-Learning-Hierarchical-Representations-69bcc72619d64be2854f602ab5725d46?pvs=21)
    - hyperbolic neural network[Hyperbolic neural networks](https://www.notion.so/Hyperbolic-neural-networks-c56b2e81b017499d979c36e4e8858fcf?pvs=21)
- tree-like inductive bias incorporate
    - **entailment loss**를 통해 child node가 parent node embedding으로부터 비롯된 cone에 들어가도록 force[Hyperbolic entailment cones for learning hierarchical embeddings](https://www.notion.so/Hyperbolic-entailment-cones-for-learning-hierarchical-embeddings-2e7150fdfb8c46749c2a9c9cc8f0862d?pvs=21)
- Hyperbolic contrastive learning
    - hyperbolic space에서 geodesic distance objective를 최소화하도록 **contrastive learning**을 사용
        
        > - Hyperbolic Contrastive Learning for Visual Representations beyond Objects
        > - Hyperbolic Contrastive Learning
        > 
- Hyperbolic embedding in cross-modality setting → MERU
    - **contrastive loss + entailment loss** 사용됨

이 논문에서 마지막 MERU의 loss 간 interplay를 다뤘음

### Joint Multimodal Learning

- early pretraining work : unimodal pretraining
    
    pretraining task로 인해 structured embedding space가 생긴다면, downstream task에 유용할 것이라는 생각
    
- multimodal setting으로 확장 : image-text pair에 대한 weak supervision을 추가로 assume
    
    > - Unicoder-VL: A Universal Encoder for Vision and Language by Cross-modal Pre-training
    > - Large-scale multi-modal pre-trained models: A comprehensive survey
     
    - 가장 유명 : contrastive objective를 train
        
        > CLIP
        ALIGN
        > 
- encoder only에서 text decoder를 포함하거나(BLIP) LLM과 합치는 쪽(BLIP-2,LLaVA)으로 발전하는 중