---
title : 'Bayesian Estimator'
date : 2024-08-21T21:00:37+09:00
draft : false
authors : ["braveseokyung"]
description : MLE(Maximum Likelihood Estimator)와는 다른, Bayesian Estimator에 대해 알아보자!
categories : ["courses"]
series : ["information-theory-and-inference"]
series_weight : 1
toc:
    auto: true
---

<!--more-->

## Maximum Likelihood Estimation vs Bayesian Estimation

앞글에서 MLE(Maximum Likelihood Estimator)는, likelihood를 maximize하는 parameter $\theta$를 estimator로 선택했다. 여기서 parmaeter $\theta$는 미지이지만 고정된 값으로 생각하는데, 이것은 frequentist의 관점이다. 반면 Bayesian estimation에서는 $\theta$를 **random variable**로 보는 Bayesian 관점을 취하는데, 때문에 random varaible $\theta$의 pdf인 $p(\theta)$가 중요해진다. $p(\theta)$는 prior라고 하는데, prior라고 부르는 이유는 $p(\theta)$를 우리가 사전에 알고있는 정보로 보기 때문이다. 

즉 Bayesian Estimation에서는 Prior의 pdf가 고려되므로, 몇가지 우리가 알고 있는 pdf로 prior를 가정함으로써 bayesian estimator를 유도하게 된다.

## PDFs

Bayesian estimation을 할 때 중요한 PDF

- Gaussian $N(\mu,\sigma^2)$
- Gamma $Gamma(\alpha,\lambda)$
- Beta $Beta(a,b)$
- Inverse Gamma, $I Gamma(a,\lambda)$

### Gaussian distribution

$X\sim\mathcal{N}(\mu,\sigma^2)$

- pdf
    
    {{<image src="9%20Bayesian%20Estimator/Untitled.png" width="500" height="500">}}  
- mean
    
    $E(X)=\mu$
    
- mode
    
    $x_{mode}=\mu$
    

### Gamma distribution

$X\sim Gamma(\alpha,\lambda)$

- pdf
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%201.png" width="500" height="500">}}
    
    엥 parameter a,$\lambda$ 인거 같은디
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%202.png" width="500" height="500">}}
    
    - $\Gamma(a)=(a-1)\Gamma(a-1)$
    - Gamma(1,$\lambda)$=Expo($\lambda)$
- mean
    
    $E(X)=\frac{a}{\lambda}$
    
    $E(\frac{1}{X})=\frac{\lambda}{a-1}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%203.png" width="500" height="500">}}
    
- mode
    
    $x_{mode}=\frac{a-1}{\lambda}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%204.png" width="500" height="500">}}
    

### Beta distribution

$X\sim Beta(a,b)$

- PDF
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%205.png" width="500" height="500">}}
    
    where 
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%206.png" width="500" height="500">}}
    
- mean
    
    $E(X)=\frac{a}{a+b}$
    
- mode
    
    $x_{mode}=\frac{a-1}{a+b-2}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%207.png" width="500" height="500">}}
    

### Inverse Gamma distribution

$X\sim IGamma(a,\lambda)$

$X=\frac{1}{Y}, Y\sim Gamma(a,\lambda)$ : random variable의 역수가 Gamma distribution을 따르는경우

- PDF
    
    transformation of variables
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%208.png" width="500" height="500">}}
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%209.png" width="300" height="300">}}
    
- mean
    
    $E(X)=\frac{\lambda}{a-1}$
    
    $E(\frac{1}{X})=\frac{a}{\lambda}$
    
    Gamma와 반대!
    
- mode
    
    $x_{mode}=\frac{\lambda}{a+1}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2010.png" width="500" height="500">}}
    

## Prior Distribution

- parameter  $\theta$ 에 대해 힌트가 있다고 생각해보자
    - 예를들어 가능한 range가 있거나
    - 다른 값보다 더 likely한 값이 있다는 prior information이나
- 이러한 힌트가 $\theta$의 distribution으로 주어지고, prior distribution이라고 한다
    
    $p(\theta)$ : prior distribution on $\theta$
    

### Bayesian inference

- 우리가 알고있는 것
    
    이제 우리가 알고있는 것은
    
    - prior distribution : $\theta$ 에 대한 prior distribution $p(\theta)$
    - data model : likelihood function $p(x;\theta)$
        
        우리가 관찰한 data들의 probability - unknown $\theta$가 given
        
    - actual data : $\mathbf{x}=[x_1,…,x_N]$ (evidence)
- 알고자 하는 것: posterior probability
    
    data $\mathbf{x}$를 관찰하는 것은  $\theta$의 probability에 영향을 준다(posterior)
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2011.png" width="500" height="500">}}
    

### Example : biased coin(bernoulli)

x=1 : H, x=0 : T

- 알고싶은 것
    
    $\theta=P(x=1)$ (0<$\theta$<1)
    
- prior를 uniform으로 가정
    
    $p(\theta)=Unif(0,1)$
    
- data model
    
    likelihood function $p(x;\theta)=\theta^x(1-\theta)^{1-x}$
    
- actual data
    
    $\mathbf{x}=[1,1,0]$
    
    $p(\mathbf{x};\theta)=\theta^2(1-\theta)$
    
- knowing the data(evidence) $\mathbf{x}$, $\theta$의 updated probability given [1,1,0]은
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2012.png" >}}
    
    posterior가 Beta(3,2)
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2013.png" >}}
    
    해석 : $\theta$에 대한 probability가 data를 관찰함으로써 update된 것
    

### Example : Gaussian

- 알고싶은 것
    
    parameter $\theta=\mu$ 의 posterior probability
    
- prior distribution $p(\theta)$가 gaussian임이 알려져있음
    
    $\theta\sim\mathcal{N}(0,\gamma^2)$
    
- actual data
    
    single data point x=m을 observe
    
- data(evidence) x=m을 알게됨으로써, $\theta$의 posterior probability given m은
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2014.png" width="500" height="500">}}
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2015.png" width="500" height="500">}}
    

## Bayesian Estimator

- prior $p(\theta)$와 $p(x|\theta)$가 주어졌을 때 estimator $\hat\theta(x)$를 어떻게 구할 수 있을까?
    - p(x;\theta) 만 알았을 때는 모든 가능한 x에 대해 squared error를 최소화하는 \hat\theta(x)를 찾고자 했음
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2016.png" width="300" height="300">}}
        
    - 이제 모든 가능한 x말고 $\theta$도 고려해야함
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2017.png" width="300" height="300">}}
        
- Bayesian estimation에서는 squared error를 `generalize`해서 error function $L(\hat\theta(x)-\theta)$로 표시하고, error function을 모든 $\mathbf{x},\theta$에 대해 expectation을 구한 것을 risk function이라고 함
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2018.png" width="500" height="500">}}
    
    - R : `Bayes risk`
    - L : `risk function`
    
- `Bayesian estimator`는 Bayes risk를 minimize 하는 $\theta$
    - estimator는 data에 대한 함수이다 $\hat\theta_{Bayesian}(\mathbf{x})$
    
    $$
    ⁍
    $$
    

### Bayes risk

- bayesian estimator w.r.t to risk function L
    
    bayesian estimator는 bayes risk를 최소화하는 $\theta$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2019.png" width="500" height="500">}}
    
- risk function
    - squared error
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2020.png" width="500" height="500">}}
        
    - absolute error
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2021.png" width="500" height="500">}}
        
    - hit-or-miss error
        - $\delta(x)$ : Dirac delta function
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2022.png" width="500" height="500">}}
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2023.png">}}
        

### Bayesian Estimators

- $L\geq 0$일 때
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2024.png" width="500" height="500">}}
    
    - 두번째 줄에서 세번째 줄 넘어갈 때 expectation에서 x하나에 대한 함수로 바꾼 것
- **Bayesian MMSE**(Bayesian Minimum mean squared error) estimator
    - 정의: mean squared risk를 최소화(risk function이 squared)
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2025.png" width="500" height="500">}}
        
- **MAP estimator**
    - 정의: hit-or-miss error risk를 최소화
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2026.png" width="500" height="500">}}
        

### Bayesian MMSE estimator

- mean squared risk를 최소화
- 최소화 하는 estimator를 구하면
    
    Bayesian MMSE estimator는 $\theta$의 mean w.r.t to posterior pdf given data $\mathbf{x}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2027.png" width="500" height="500">}}
    
- 증명
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2028.png" width="500" height="500">}}
    

### Bayesian MAP estimator

- hit-or-miss error risk를 최소화
- 최소화 하는 estimator를 구하면
    
    MAP estimator는 **mode** of posterior pdf given data x
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2029.png" width="500" height="500">}}
    
- 증명
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2030.png" width="500" height="500">}}
    

### Bayesian estimator and posterior pdf

<aside>
💡 Bayesian MMSE estimator → mean of posterior
MAP estimator → mode of posterior

</aside>

- posterior pdf에 의해 determined
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2031.png" width="500" height="500">}}
    
    - 예를 들어 posterior가 Gamma라면,
        
        {{<image src="9%20Bayesian%20Estimator/Untitled%2032.png" width="500" height="500">}}
        
        mean>mode이므로 MMSE estimator가 MAP estimator보다 큼
        

### Example: Bernoulli distribution with uniform prior

- $\mathbf{x}=[x_1,...,x_N]$  : iid sample from Bernulli distribution
- parameter : $\theta$
- $p(x|\theta)$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2033.png" width="300" height="300">}}
    
- assume prior $p(\theta)$ uniform
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2034.png" width="300" height="300">}}
    
- posterior distribution은 $Beta(n_1+1,n_0+1)$이 된다
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2035.png" >}}
    
- Bayesian MMSE estimator
    
    posterior의 평균이므로 $\frac{n_1+1}{n_1+n_0+2}=\frac{\sum x_i+1}{N+2}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2036.png">}}
    
- MAP estimator
    
    posterior의 mode이므로 $\frac{n_1}{n_1+n_0}=\frac{\sum x_i}{N}$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2037.png">}}
    

### Example: Bernoulli distribution with linear prior

- linear prior
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2038.png" width="300" height="300">}}
    
- posterior beta distribution $Beta(n_1+2,n_0+1)$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2039.png">}}
    
- Bayesian MMSE estimator
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2040.png" width="500" height="500">}}
    
- MAP estimator
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2041.png" width="500" height="500">}}
    

### Example: Bernoulli distribution with beta prior

- beta prior : $Beta(a,b)$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2042.png" width="500" height="500">}}
    
- posterior : $Beta(n_1+a,n_0+b)$
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2043.png">}}
    
- Bayesian MMSE estimator
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2044.png">}}
    
- MAP estimator
    
    {{<image src="9%20Bayesian%20Estimator/Untitled%2045.png">}}
