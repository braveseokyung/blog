---
title : 'Information Theory의 관점에서 Estimator 살펴보기'
date : 2024-11-20T17:54:51+09:00
draft : false
authors : ["braveseokyung"]
description : Information theory의 관점에서 Estimator를 살펴보자. 키워드 - MSE, MVUE, Fisher Information, CRLB
categories : ["courses"]
series : ["information-theory-and-inference"]
series_weight : 3
toc:
    auto: false
---

<!--more-->

# Estimators

## Inference : Problem Setting
    
{{<image src="7%20Estimators/Untitled%203.png">}}
    
- Data (Observations) $\mathbf{x}=[x_1,...,x_n]^T$
어떤 distribution $p(x)$ 를 따를 것으로 생각되는 vector of samples
- Data model $p(x)$
distribution $p(x)$, 하지만 detailed parameter $\theta$ 는 unknown
- Parameter $\theta$
    
    
    - Deterministic : unknown **constant**
    $p(x;\theta)$
    
    - Probablistic : **random variable**, distribution $p(\theta)$
    $p(x)=p(x|\theta)$
        
        bayesian approach
        
- Estimator $\hat{\theta}$
주어진 데이터로부터 parameter $\theta$를 infer하는 deterministic function
    
    $\hat\theta=g(x)$
    

## Estimator

- 어떤 distribution인지($p(x;\theta)$)는 알지만 parameter $\theta$ 는 unknown 인 상황
- 데이터 $\{x_1,...,x_N\}$ 은 distribution $p(x;\theta)$ 로부터 independent하게 sample됨
    - 따라서 $p(\mathbf{x};\theta)$는 $p(x_i;\theta)$ 의 곱으로 나타낼 수 있다
        
        {{<image src="7%20Estimators/Untitled%204.png">}}
        
- parameter $\theta$의 estimator $\hat\theta$ 는 데이터 $\mathbf{x}=[x_1,...,x_N]^T$ 에 대한 deterministic function
    
    {{<image src="7%20Estimators/Untitled%205.png">}}
    
    - 다른 set of data $\mathbf{x}'=[x_1',...,x_N']^T$ 에 대해서는 다른 값을 가진다
        
        !{{<image src="7%20Estimators/Untitled%206.png">}}
        
- 모든 possible data set $\mathbf{x}$ 에 대해 **`평균적으로** how good is $\hat\theta$ ?`

## Mean and Variance of an Estimator

<aside>
💡 estimator $\hat\theta$ 은 $\mathbf{x}$ 에 대한 함수
$x_i$ 하나하나가 random variable

</aside>

- $\hat\theta$ 은 function of random variables
위에서 $x_i$ 가 distribution으로부터 iid하게 sample된 random variable임
notation을 random variable에 맞게 바꾸면
$\{X_i\}$ : iid random variables with
    
    {{<image src="7%20Estimators/Untitled%207.png">}}
    
- $\hat\theta$의 average behavior를 살펴보기 위해 $\hat\theta$ 의 mean, variance를 고려
    
    {{<image src="7%20Estimators/Untitled%208.png">}}
    
    $\hat\theta$ 은 $\mathbf{x}$ 에 대한 함수
    

### Gaussian Noises

- $\{x_1,...,x_N\}$ : independent sample from $\mathcal{N}(\mu,\sigma^2)$
    
    {{<image src="7%20Estimators/Untitled%209.png">}}
    
- mean estimator
data $\mathbf{x}=[x_1,...,x_N]^T$ 로부터 true mean $\mu$ 를 estimate하고 싶은 것
    - sample mean
        
        {{<image src="7%20Estimators/Untitled%2010.png">}}
        
    
- mean and variance of mean estimator
**mean estimator** 의 평균과 분산
    
    {{<image src="7%20Estimators/Untitled%2011.png">}}
    
- variance estimator
    - sample variance
        
        {{<image src="7%20Estimators/Untitled%2012.png">}}
        
- mean and variance of variance estimator
    
    **variance estimator**의 평균과 분산
    
    Mean : $E(\theta^2)=\theta^2$
    
    Variance: $Var(\theta^2)=\frac{2\theta^4}{N-1}$
    유도:
    
    {{<image src="7%20Estimators/Untitled%2013.png">}}
    

### Exponential distribution

- $\mathbf{x}=\{x_1,...,x_N\}^T$ : independent samples from $Expo(\lambda)$
    
    {{<image src="7%20Estimators/Untitled%2014.png">}}
    
- estimator of $\lambda$
exponential의 mean이 $\lambda$ 의 inverse이므로
    
    {{<image src="7%20Estimators/Untitled%2015.png">}}
    
    Expo($\lambda$)=Gamma(1,$\lambda$) 이고 $Y=\sum_{i=1}^{N}X_i\sim Gamma(N,\lambda)$
    
    따라서,
    
    {{<image src="7%20Estimators/Untitled%2016.png">}}
    

## Desired Estimator

- 좋은 estimator란?
    - unbiased estimator: estimator의 mean이 true mean
    $E(\hat\theta)=\theta$
    - estimator의 variance가 최대한 작도록
    $Var(\hat\theta)$
- 정의 : Bias
어떤 estimator의 bias는 estimator의 평균과 실제값의 차이의 제곱
    
    $(E(\hat\theta)-\theta)^2$
    

## Evaluating Estimator : Mean Squared Error

- 정의 : estimator $\hat\theta$ 에 대한 mean square error(MSE)는 다음과 같다
    
    $MSE(\hat\theta)=E((\hat\theta-\theta)^2)$ 
    
- 성질 : MSE의 decomposition
    
    estimator의 MSE는 variance와 bias의 합으로 표현할 수 있다
    
    {{<image src="7%20Estimators/Untitled%2017.png">}}
    
    - derivation
        
        {{<image src="7%20Estimators/Untitled%2018.png">}}
        
        $E(\hat\theta)-\theta$ 즉 bias가 constant
        

## Minimum Variance Unbiased(MVU) Estimator

### Unbiased estimator

bias=0

{{<image src="7%20Estimators/Untitled%2019.png">}}

### MVU estimator

- 정의: unbiased estimator 중에 lowest variance를 갖는 놈 즉, the best unbiased estimator
- 하지만 MVU estimator를 찾는 것은 보통 어렵다

### MVU is not always optimal in MSE

MSE를 계산해봤을 때, MVU가 가장 optimal하지 않을 수도 있다

- example

## Fisher Information

- 정의
    
    parameter를 모르는 pdf $p(\mathbf{x};\theta)$ 에 대해 `fisher information`은
    
    {{<image src="7%20Estimators/Untitled%2020.png">}}
    
- 성질
    - 두번 미분 가능한 $\ln p(\mathbf{x};\theta)$ 에 대해
    - fisher information이 non-negative
        
        {{<image src="7%20Estimators/Untitled%2021.png">}}
        

## Cramer-Rao Lower Bound(CRLB)

- Theorem
    
    x iid sampled from $p(x;\theta)$
    
    $\hat\theta(x)$ estimator for  $\theta$
    
    1. unbiased estimator라면 다음을 만족
        
        {{<image src="7%20Estimators/Untitled%2022.png">}}
        
        - $I(\theta)$는 Fisher information이고,
        - $\frac{1}{I(\theta)}$를 Cramer-Rao Lower Bound(CRLB)라고 한다
    2. CRLB를 variance로 갖는 unbiased estimator는 found iff
        
        {{<image src="7%20Estimators/Untitled%2023.png">}}
        
        for some g
        
        - 이때 $\hat\theta=g(x)$ 가 MVU estimator가 되고 이 estimator의 variance가 CRLB인 $\frac{1}{I(\theta)}$

## Examples

* Example : sample mean estimator가 Gaussian noise의 MVU

* Example : sample variance estimator for Gaussian data는 MVU가 아님

* Example: estimator for exponential data - MVU를 찾을 수 없음
