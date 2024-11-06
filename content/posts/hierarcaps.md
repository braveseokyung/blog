---
title : '[논문 리뷰] Emergent Visual-Semantic Hierarchies in Image-Text Representation'
date : 2024-11-06T20:55:46+09:00
draft : false
authors : ["braveseokyung"]
description : ECCV 2024 oral paper인 Emergent Visual-Semantic Hierarchies in Image-Text Representation 논문에 대한 리뷰입니다.
categories : ["paper-review"]
toc:
    auto: true
---

<!--more-->

## 논문 정보
- ECCV 2024 Oral
- Keyword : Hierarchical representation, Multimodal learning, Vision and Language
- [paper](https://arxiv.org/abs/2407.08521)

## Abstract

### 문제 상황
CLIP과 같은 최근 VLM 연구들은 text와 image의 shared semantic space를 표현하는 효과적인 방법을 제안하고 있지만 image와 그 image를 설명하는 여러 text 간의 hierarchical한 성질을 explicit하게 반영하지 못합니다. 반면, hierarchy를 연구한 기존의 multimodal hierarchical representation learning 방법들은 scratch부터 학습해야 하기 때문에 costly한 동시에 SOTA multimodal foundation model의 knowledge를 사용하지 못한다는 문제가 있습니다.

### 방법론
이 연구에서는, 기존의 foundation model이 hierarchy를 explicit하게 학습하지 않았음에도 visual-semantic hierarchy에 대해 emergent understanding(zero-shot hierarchical understanding)을 가지고 있음을 보이고 이러한 understanding을 활용할 수 있는 Radial Embedding(RE) framework를 제안합니다. 또한 Hierarchical representation에 대한 quantitative benchmark로 삼을 수 있는 HierarCaps dataset을 제안하며, 이 dataset을 LLM을 통해 자동으로 구성한 방식을 설명합니다. 

### 결론
위의 방법론을 통해 foundation VLM이 hierarchical understanding을 잘하도록 디자인된 다른 모델에 버금가는 understanding을 가지고 있으며, text-only fine-tuning만으로 기존의 pretrained knowledge를 보존하는 동시에 hierarchical reasoning에 better align될 수 있음을 보입니다.

## Introduction

### visual-semantic hierarchy

{{<image src="스크린샷 2024-11-06 오후 9.21.23.png">}}

Text에서 개념, 문장 간 hierarchy가 존재하듯이, image와 text 간에도 hierarchy가 존재하는데, 이를 visual-semantic hierarchy라고 합니다. 한 이미지를 설명할 수 있는 text는 여러 개가 존재할 수 있습니다. 예를 들어, "mammal"이라는 description을 코끼로 사진에 대해서도, 기린 사진에 대해서도 적합한 설명이 될 수 있습니다. 하지만 "mammal"의 하위 개념인 "funny giraffe"는 코끼로 사진을 설명할 수 없으며, "funny giraffe"로 설명될 수 있는 image는 "mammal"로 설명될 수 있는 image의 부분집합이 됩니다. 이렇게 점점 더 description을 구체화할수록 그 description은 특정 이미지에 가까워집니다. 그리고 image는 어떤 구체적인 description보다도 더 많은 정보를 담고 있으므로, text와 image의 hierarchy를 생각해보면 text는 보다 general한 상위에, image는 specific한 하위에 놓이게 됩니다.

따라서 이 논문을 비롯한 Hierarchical representation을 연구하는 논문들은 VLM이 이러한 visual-semantic hierarchy를 이해할 수 있어야 한다고 이야기하고 있습니다.

### 이전 연구들의 한계

Visual-semantic hiearchy를 연구한 이전 연구들은 Hyperbolic space를 사용하고 있습니다. 하지만 Foundation model은 Euclidean space에서 학습되었기 때문에, Hyperbolic space에 embedding하기 위해서는 foundation model의 weight를 사용하지 못하고 (..정말?) scratch부터 학습해야 한다는 문제점이 있다고 지적합니다. Foundation VLM의 knowledge를 사용할 수 없어서 hierarchy에 대해서는 잘 학습될지 몰라도 다른 multimodal task에 대해 robust하지 못하다는 것입니다. 따라서 이 논문은 Foundation VLM의 knowledge는 유지하면서도 visual-semantic hierarchy를 이해하도록 하는 것을 목표로 하고 있습니다.

## Method

### Radial Embedding(RE) Framework

이 논문에서 제안하는 Radial Embedding(RE) Framework을 이해하기 위해서는 기존 방법론인 Entailment Cone(EC) Framework를 알아야 할 필요가 있어, 잠시 설명하겠습니다. 

Entailment Cone은 위에서 언급되었던 기존의 Hyperbolic 모델(Ganea et al., Hyperbolic entailment cones for learning hierarchical embeddings, 2018)이 사용하는 방법입니다. 이 방법론은 text와 image의 hierarchy 즉 entailment를 반영하는 개념인데, 더 포괄적인 개념의 노드를 origin으로 삼는 cone을 만들어서 하위 개념들이 그 cone의 범위 안에 들어오도록 학습합니다. 예를 들어 상위 개념인 animal로부터 비롯된 cone안에 cat이 들어가도록 하는 것이죠. 그리고 이것을 수식적으로 표현하기 위해, 

하지만 이러한 Entailment Cone은 hyperbolic space라는 setting에서 가능한 개념이기 때문에 이 논문이 원하는 Euclidean setting에서도 가능한 방법이 아닙니다. 이 논문에서 제안하는 Radial Embedding Framework를 EC Framework와 비교해서 살펴보겠습니다.

{{<image src="스크린샷 2024-11-06 오후 10.23.53">}}

왼쪽이 EC framework, 오른쪽이 RE framework입니다. e,e'은 embedding이고 여기서 e가 e'보다 포괄적인 개념의 embedding입니다. 예를 들어 (animal, cat)의 embedding이라고 생각해볼 수 있습니다. r은 root의 embedding으로, 일종의 origin입니다(거리를 구할 때 원점으로부터의 거리를 구하듯이 기준점이 필요합니다). 

수식을 살펴보자면, 왼쪽에서 가장 중요한 것은 cone C입니다. 이상적인 상황은 e에 entail되는 e'이 cone의 각도 $\theta$ 안에 들어오는 것입니다. 사진을 보면 e'이 cone 밖에 위치하고 있는데, 이에 대한 penality를 주기 위해 embedding e,e'이 이루는 외각(초록색부분)에서 cone 각 $\theta$ 즉 half-aperture 부분을 빼서 그 값이 클수록 loss를 크게 줍니다.
반면 RE Framework에서는 cone을 이용하지 않고 negative pair e''와의 외각을 추가적으로 수식에 포함시켜서, positive pair가 이루는 각도는 작아지고 negative pair가 이루는 각도는 커지도록 학습합니다. 즉 positive pair (e,e')를 cone에 위치시킨다기 보다 루트 r과의 일직선 상에 놓이게끔하고, negative pair와는 정반대에 위치하도록 학습합니다.

### Hierarcaps Dataset
RE framework에서는 negative pair가 필요합니다. 하지만 기존 dataset은 기본적으로 image-caption positive pair로 이루어져있기 때문에 이 논문에서는 negative pair와 단계별 description을 LLM으로 생성해서 사용했습니다.

{{<image src="스크린샷 2024-11-06 오후 10.42.20">}}

위와 같이 한 이미지에 대해 단계적으로 점점 구체적인 description을 positive, negative 모두 사용하고 있습니다. negative는 positive와 비교했을 때 한단계 상위 개념에는 포함되자만, 같은 단계의 개념에서는 Hierarcaps Dataset은 COCO caption dataset을 이용하고 있는데, positive pair의 맨 끝에 있는 가장 구체적인 description이 image와 쌍을 이루는 원래의 pair입니다. 나머지 단계의 description과 negative pair는 LLM을 통해 생성됩니다.

{{<image src="스크린샷 2024-11-06 오후 10.07.52">}}

먼저 real caption을 LLM(LLama2)에 주고 추상화 정도를 다르게 해서 요약해달라고 프롬프팅을 하여 3가지의 description을 추가로 얻습니다. 이렇게 구성된 총 4개의 description에는 순서를 부여할 수 있는데, small language model에 이 순서를 학습시켜서 generic->specific, specific->generic 양방향의 순서를 모두 뱉을 수 있게 합니다. 즉, 1번 description이 들어가면 2,3,4를 순차적으로 뱉을 수 있도록 학습시킵니다. 이를 통해 negative caption을 만들 때는 general-> specific 방향으로 hierarchy를 완성하게 하여 상위 레벨에 속하는 다른 text를 가져옵니다.

## Conclusion

정리하자면, 이 논문은 hierarchy understanding을 갖는 VLM을 학습시키는데에 있어 기존 방법들처럼 hyperbolic embedding을 scratch부터 학습하지 않고, negative pair를 갖는 비교적 적은 데이터로 foundation VLM을 fine-tuning하여 foundation model의 knowledge를 보존하면서도 적은 cost로 hierarchical alignment를 얻는 방법을 제안한 논문이라고 정리할 수 있을 것 같습니다! 
