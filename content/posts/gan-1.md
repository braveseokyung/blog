---
title : 'GAN (1)'
date : 2024-05-29T21:42:55+09:00
draft : false
authors : ["braveseokyung"]
description : generative model 중 generator와 discriminator의 2 player game으로 학습되는 GAN을 알아보자!
categories : ["Generative-Models"]
series : ["generative-adversarial-network"]
series_weight : 1
toc:
    auto: false
---

<!--more-->

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled.png">}}

## Recap : Likelihood Training

- Maximal likelihood training models
    
    $$
    \hat{\theta}=\argmax_{\theta}\sum_{i=1}^{M}\log p_\theta(x_i), x_1,x_2,...,x_m \sim p_{data}(x)
    $$
    
    likelihood를 가장 크게 하는 $\theta$ MLE를 찾는 것은 data distrubtion과 generated distribution 사이의 KL divergence  $d_{KL}(p_{data}||p_{\theta})$ 를 최소화하는 것과 같다
    
    Variational Autoencoder, Normalizing Flows가 여기에 해당
    
    keyword : [data distribution](https://www.notion.so/data-distribution-6c76d92307b749198b93db38ef33145c?pvs=21) 
    
- 높은 likelihood가 high-quality generated sample을 뜻하는가?
optimal generative model이라면 highest likelihood, best sample quality를 보장하겠지만, imperfect model에 대해서는 high likelihood가 good sample quality를 보장하지 못할 수 있다

## GAN ( Generative Adversarial Models )

### Transformation (Generator)

- Problem
high-dimensional complex data distribution으로부터 sample하고 싶은데, direct한 방법이 없음
- Solution
simple distribution으로부터 sample하고 ( ex. random noise ), 이 distribution을 복잡한 data distribution으로 transform
- Question
이 complex transformation을 어떻게 얻을 것인가?
→ Neural Network!

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%201.png">}}

---

### Classify (discriminator)

- Problem
output distribution이 training data distribution과 비슷한지 알고 싶음
- Solution
생성된 sample과 training sample이 비슷한지 비교
- question
이 classfier를 어떻게 표현할 것인가?
→ Neural Network!

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%202.png">}}

---

## ⭐ Training GANs: Two-player game

Generator는 real-looking sample을 만들어 discriminator를 속일 수 있도록 학습하고, discriminator는 real image와 fake image를 구분할 수 있도록 학습

minimax game

- GAN Training objective
    
    $$
    \min_G\max_D V(D,G)=\mathbb{E}_{x\sim p_{data}(x)}[\log D(x)]+\mathbb{E}_{z\sim p_z(z)}[log(1-D(G(z)))]
    $$
    
    $D(x)$  : **real data** x 에 대한 discriminator output
    
    $1-D(G(z))$ : **fake data** G(z)에 대한 discriminator output
    
    $D(.)$: discriminator가 real이라 생각하면 1, fake라 생각하면 0
    위의 두 값 모두 discriminator가 답을 맞추면 1, 틀리면 0이 나옴
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%203.png">}}
    
    - binary cross entropy
        
        {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/derivation.png">}}
        
    
    따라서,
    
    Discrminator D는 objective를 maximize하려고 하고 (D(x)는 1, D(G(z))는 0이 되도록)
    
    Generator G는 objective를 minimize하려고 한다 (D(G(z))가 1이 되도록)
    
    - optimality
        
        global minimum은 $p_{data}=p_G$ 일때라는 것을 보일 수 있음
        
    
    →
    
    Alternate between
    
    1. Gradient ascent on Discriminator
        
        $$
        \max_D \mathbb{E}_{x\sim p_{data}(x)}[\log D(x)]+\mathbb{E}_{z\sim p_z(z)}[log(1-D(G(z)))]
        $$
        
    2. Gradient descent on Generator
        
        $$
        \min_G\mathbb{E}_{z\sim p_z(z)}[log(1-D(G(z)))]
        $$
        
        in practice, discriminator가 맞는 likelihood를 minimize하지 말고 discriminator가 틀리는 likelihood를 maximize
        
        {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%204.png">}}
        
        sample이 fake일 때 low signal, gradient vanishing
        sample이 이미 좋은데 gradient signal이 그쪽에서 dominated
        
        {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%205.png">}}
    
    참고자료 : https://youtu.be/igP03FXZqgo?t=2538&si=vb62yzuDcdm6lQly
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/justin-johnson-1.png">}}

    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/justin-johnson-2.png">}}

    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/justin-johnson-3.png">}}
    
    - discriminator gradient ascent: k steps

### ⭐ Gan Training Algorithm

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%206.png">}}

- training이 끝나면 generator를 가지고 generate new images
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%207.png">}}
    

## GAN’s architecture

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%208.png">}}

### training discriminator

generator를 freeze하고, backprop error to update discriminator weights

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%209.png">}}

### training generator

discriminator를 freeze하고, backprop to update generator weights

- generator를 학습할 때는 real example이 필요없다
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%204.png">}}
    

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2010.png">}}

## Samples from GAN

generated sample

- nearest neighbor from training set

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2011.png">}}

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2012.png">}}

## Deep Convolutional Generative Adversarial Networks(DCGAN)

- generator가 fractionally-strided convolution으로 구성된 upsampling network
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2013.png">}}
    
- discriminator도 convolutional network

Architecture guidelines

- pooling layer
    - discriminator : strided convolution으로 교체
    - generator: fractional-strided convolution으로 교체
- batch norm : generator/discriminator 모두에 batch norm 적용
- fc hidden layer 삭제
- generator의 모든 layer에서 ReLU 사용, 단 ouput에서만 Tanh사용
- discriminator의 모든 layer에서 LeakyReLU 사용

## Samples from DCGAN

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2014.png">}}

## Interpolating between random points in latent space

- generator가 training dataset을 memorize하면 interpolation 동안 sharp transition
    
    $z_t=tz_0+(1-t)z_1$
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2015.png">}}
    

## Interpretable Vector Arithmetic

- {smiling woman}-{neutral woman}+{neutral man}={smiling man}

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2016.png">}}

- {Glasses man}-{No glasses man}+{No glasses woman}={Woman with glasses}

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2017.png">}}

## CycleGAN

어떤 domain에서 다른 domain으로 translate image

ex) monet↔photos, zebras↔horses, summer↔winter

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2018.png">}}

### ⭐CycleGAN with unpaired data

- 일반적인 regression과 CycleGAN이 다른 부분은 unpaired setting이라는 것.
- 어떻게 X와 Y 사이의 transformation을 unsupervised manner로 배울 수 있을까?

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2019.png">}}

### CycleGAN setting

- Data
    
    다른 domain의 두 set
    
    $X=\{x_i\}_{i=1}^{N_x}, Y=\{y_i\}_{i=1}^{N_y}$
    
- Generator 2개
    
    $F:\mathcal{X}\rightarrow\mathcal{Y}, G:\mathcal{Y}\rightarrow\mathcal{X}$ 
    
- Discriminator 2개
    
    $D_X,D_Y$
    

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2020.png">}}
- GAN loss objective 2개
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2021.png">}}
    

### Cycle Consistency Loss

- $F(G(X))=X, G(F(Y))=Y$
- cycle
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2022.png">}}
    
    {{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2023.png">}}
    

### Overall loss for CycleGAN

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2024.png">}}

### CycleGAN samples

{{<image src="11%20Generative%20Adversarial%20Networks%209bd77cd02a7b4271b4925238db5d5b0b/Untitled%2025.png">}}

## Reference

Generative Adversarial Networks : https://arxiv.org/abs/1406.2661

Radford et al, “Unsupervised Representation Learning with Deep Convolutional Generative Adversarial Networks, ICLR 2016 : http://arxiv.org/pdf/1511.06434

Zhu et al. Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks, ICCV 2017 : https://arxiv.org/abs/1703.10593