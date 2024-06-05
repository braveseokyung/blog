---
title : '[paper & 코드 리뷰] COMET : Unsupervised Learning of Compositional Energy Concepts'
date : 2024-06-05T20:18:07+09:00
draft : false
authors : ["braveseokyung"]
description : EBM 논문 Unsupervised Learning of Compositional Energy Concepts (COMET) 코드 리뷰
categories : ["Generative-Models"]
series : ["energy-based-models"]
series_weight : 1
toc:
    auto: false
---

<!--more-->
[paper & code] https://energy-based-model.github.io/comet/

## paper
### 1. Introduction
사람의 지능 - 지금 하고자 하는 task를 위해 전에 배운 것을 합치고(composition), 재사용하는 것(re-utilization)
"infinite use of finite means"
이 논문의 관심사 - unsupervised하게 compositional component를 찾고(decomposition) 합칠(composition) 수 있는 system, COMET
#### contribution
1) image를 넓은 범위의 요소(global factor)와 좁은 범위의 요소(local factor)로 쪼갤(decomposition) 수 있는 하나의 framework를 제시
2) 더 사실적인 데이터
3) generalize 능력, 다른 종류(mode) data, 다른 instance의 요소를 잘 받아들임
### 2. Related Work

| 기존 work                                                                | 기존 work 특징                           | COMET 특징                                                                                     |
| ---------------------------------------------------------------------- | ------------------------------------ | -------------------------------------------------------------------------------------------- |
| global factor disentanglement, <br>ICA(independent component analysis) | global latent space를 찾는 것 목표         | data를 compositional vector space의 집합으로 쪼갬<br>같은 factor의 여러 instance OR<br>다른 데이터셋을 합칠 수 있게 함 |
| unsupervised object discovery                                          | 각 object를 segment해서 scene을 쪼갬        | 외부적인 segmentation mask x<br>object 간 global한 관계                                              |
| EBMs                                                                   | Langevin sampling으로 sample을 generate | unsupervised 매너로 factor를 찾음                                                                  |

### 3. Composable Energy Networks
목표 : 각 이미지 x에 대해 요소들의 집합(set of components) $Z=\{z_0,z_1,...,z_k\}$ 를 얻는 것
1) Z를 어떻게 set of energy function으로 표현할지
2) data를 어떻게 unsupervised하게 Z의 요소로 쪼개는 방법을 학습할 수 있는지
#### 3.1. Composing Factors of Variation as Energy Functions

{{<image src="스크린샷 2024-03-28 오후 3.45.13.png">}}
이전 연구들 composing components -> image

|     | $\beta$-VAE                                                                                | MONET                                                    |
| --- | ------------------------------------------------------------------------------------------ | -------------------------------------------------------- |
| 특징  | decoder를 통해 set of component를 image에 mapping                                               | decoder with shared weights<br>각각의 component에 대해 process |
| 한계  | 같은 component가 있는 여러 instance를 합칠 수 없음<br>training할 때 발견된 component 이외의 component에 대해 대응 불가 | component들이 각각 decode되어 서로의 관계를 알 수 없음                   |
1) component간에 shared decoder를 갖고,
2) jointly decode components 하여 관계를 찾을 수 있어야 함
   
	 **COMET: 각 component z를 energy function으로 표현!**
##### Representing Components as Energy Functions
image x의 하나의 component z에 대해 다음의 energy function을 부여
$$E_\theta(x,z):R^{D}*R^{M} -> R$$
위의 energy function은 component z를 가지고 있는 모든 이미지들에 low energy를 부여하고, 다른 모든 이미지들에 대해서는 high energy를 부여하도록 학습됨

component z를 가지고 있는 $x'$ 을 얻기 위해서 다음의 식을 풂
$$x'=argmin_{x}E_\theta(x;z_i)$$
(component를 가지고 있으면 energy가 낮아야 하므로)
[[Compositional Visual Generation with Energy Based Models]] 와 달리, 확률적 interpretation이 없음
##### Composing Energy Functions
multiple component에 대해 set of components $\{z_i\}$ 를 각 energy function의 합으로 표현
$$\sum_{i}E_\theta(x,z_i)$$

set of components Z를 가지고 있는 image $x'$를 얻기 위해 다음의 식을 풂
$$x'=argmin_{x}\sum_{i}E_\theta(x;z_i)$$
MONET도 component를 합치는(composing) 하는데에 summation을 사용하지만, 
COMET은 component의 sum이 아닌 각 component에 대응하는 **cost function의 sum**

* 결과
즉, generation $x'$는 각각의 energy function을 jointly minimize한 결과물
각 component $z_i$는 **같은 energy function** $E_\theta$로 parameterized 되어 여러 component를 모델링할 수 있다
#### 3.2 Unsupervised Decomposition of Composable Energy Functions
input image $x_i$로부터 어떻게 set of composable energy function을 찾아내는지 설명!

앞에서 set of components $Z=z_i$가 set of composable energy function으로 위의 식 $x'=argmin_{x}\sum_{i}E_\theta(x;z_i)$ 를 통해 encode 될 수 있음을 보였음

unsupervised하게 set of component Z를 찾기 위해서는, ==input image $x_i$를 recompose하여 train
set of components $z_k$를 infer하기 위해 learned neural network encoder $Enc_\theta(x_i)$를 사용==
$$\mathcal{L}_{MSE}(\theta)=||argmin_x(\sum\limits_{k}E_\theta(x;Enc_\theta(x_i)[k]))-x_i||^2$$

실제로는 위 식의 argmin을 구하는 것이 computationally intractable하기 때문에, ==x에 대한 N step gradient optimization을 통해 argmin operation을 approximate==
approximate optimum $x_i^N$ 은 다음과 같이 구해짐
$$x_i^N=x_i^{N−1}−λ∇_x \sum\limits_kE_\theta(x_i^{N−1};Enc(x)[k])$$
$\lambda$ : gradient step size
$x_i^n$: n step of gradient descent 후 결과

이렇게 얻은 $x_i^n$을 넣은 modified objective인 $\mathcal{L}_{MSE}=\sum\limits_{n=1}^N||x_i^n-x_i||^2$ 을 $x_i^n$에 대해 minimize함으로써 energy function $E_\theta$를 train

## 코드
### train.py
#### parser로 argument 받기
받은 argument는 이후에 나오는 `FLAGS`에 저장됨
```python
"""Parse input arguments"""
parser = argparse.ArgumentParser(description='Train EBM model')

parser.add_argument('--train', action='store_true', help='whether or not to train')
parser.add_argument('--optimize_test', action='store_true', help='whether or not to train')
parser.add_argument('--cuda', action='store_true', help='whether to use cuda or not')
parser.add_argument('--single', action='store_true', help='test overfitting of the dataset')
# continued
```
중요 argument

#### `gen_image()` 함수
parameters: `latents`, `FLAGS`, `models`, `im_neg`, `im`, `num_steps`, `sample=False`, `create_graph=True`, `idx=None`, `weights=None`

`latents` : 원본 이미지로부터 얻은 latent

`im_neg` : random initialize된 negative image

`idx` : generate하고자 하는 idx

`num_steps` : langevin dynamics step 수

return : `im_neg`, `im_negs`, `im_grad`, `masks`

`im_neg` : 업데이트가 끝난 최종 im_neg

`im_negs` : 그동안의 im_neg가 저장된 리스트

`im_grad` : 최종 gradient

```python
im_noise = torch.randn_like(im_neg).detach()
```

`im_noise`: `im_neg` 와 같은 사이즈의 random noise `torch.detach()`는 gradient가 흐르는 것을 끊어준다고 한다 [설명](https://bo-10000.tistory.com/181)

```python
latents = torch.stack(latents, dim=0)
```

분리되었던 latent를 다시 합침

```python
if FLAGS.decoder:
    ###
else:
```

decoder 없는 경우로

```python
im_neg.requires_grad_(requires_grad=True)
s = im.size()
masks = torch.zeros(s[0], FLAGS.components, s[-2], s[-1]).to(im_neg.device)
masks.requires_grad_(requires_grad=True)
```

mask를 `torch.zeros()`로 0으로 채워진 tensor로 만들어주고 device에 올림
`im_neg`, `masks` 모두 gradient를 켜줌

```python
for i in range(num_steps):
    im_noise.normal_()

    energy = 0
    for j in range(len(latents)):
        if idx is not None and idx != j:
            pass
        else:
            ix = j % FLAGS.components
            energy = models[j % FLAGS.components].forward(im_neg, latents[j]) + energy

        im_grad, = torch.autograd.grad([energy.sum()], [im_neg], create_graph=create_graph)

        im_neg = im_neg - FLAGS.step_lr * im_grad

        latents = latents

        im_neg = torch.clamp(im_neg, 0, 1)
        im_negs.append(im_neg)
        im_neg = im_neg.detach()
        im_neg.requires_grad_()
```

`im_noise`를 normal distribution에서 sample하여 채움 [torch.normal_() 설명](https://pytorch.org/docs/stable/generated/torch.Tensor.normal_.html)

기존 `energy`에 latent에 해당하는 model output을 `energy`를 더해주면서 update
해당 energy에서 `im_neg`가 갖는 gradient `im_grad`를 `torch.autograd.grad()` 로 직접구함 [torch.autograd.grad documentation](https://pytorch.org/docs/stable/generated/torch.autograd.grad.html)

`im_neg`를 위에서 구한 `im_grad`로 update, `torch.clamp`로 min-max standardization

위 과정을 argument로 받은 num_step만큼, 한 step당 latent 수만큼 반복해서 `im_negs` 리스트에 저




#### 함수 `train()`
parameters: `train_dataloader`, `test_dataloader`, `logger`, `models`, `optimizers`, `FLAGS`, `logdir`, `rank_idx`

`models` : energy model이 여러 개

image, image에 대한 idx로 구성된 train_dataloader를 iteration하며 im_orig, latent, im_neg, energy_pos, energy_neg, im_loss를 구함

```python
im = im.to(dev)
idx = idx.to(dev)
im_orig = im
```
`im_orig` : 원본 이미지

```python
latent = models[0].embed_latent(im)
latents = torch.chunk(latent, FLAGS.components, dim=1)
```
`latent` : 첫번째 모델의 embed_latent한 결과

`latents` : component 개수만큼 latent를 쪼갬 [torch.chunk 설명](https://sanghyu.tistory.com/6)

```python
im_neg = torch.rand_like(im)
im_neg_init = im_neg

im_neg, im_negs, im_grad, _ = gen_image(latents, FLAGS, models, im_neg, im, FLAGS.num_steps, FLAGS.sample)

im_negs = torch.stack(im_negs, dim=1)
```
`im_neg`를 원본 이미지와 같은 크기의 tensor로 random initialize

`gen_image` 함수로 `im_neg`, `im_negs`, `im_grad` update
`im_negs` 리스트 안에 있는 `im_neg`를 합침

```python
energy_pos = 0
energy_neg = 0

energy_poss = []
energy_negs = []
for i in range(FLAGS.components):
    energy_poss.append(models[i].forward(im, latents[i]))
    energy_negs.append(models[i].forward(im_neg.detach(), latents[i]))

energy_pos = torch.stack(energy_poss, dim=1)
energy_neg = torch.stack(energy_negs, dim=1)
```
`energy_poss`에는 원본 이미지와 latent를 model에 통과시킨 값을, `energy_negs`에는 `im_neg`와 latent를 model에 통과시킨 값을 넣는다
이때 argument로 받은 component 개수만큼의 model을 iteration하며 각 model에 맞는 latent를 넣는다

최종 energy인 `energy_pos`와 `energy_neg`는 리스트에 들어있는 값들을 새로운 차원으로 stack해서 얻어짐

```python
ml_loss = (energy_pos - energy_neg).mean()

im_loss = torch.pow(im_negs[:, -1:] - im[:, None], 2).mean()

if it < 10000 or FLAGS.dataset != "celebahq_128":
    loss = im_loss
else:
    vgg_loss = loss_fn_vgg(im_negs[:, -1], im).mean()
    loss = vgg_loss  + 0.1 * im_loss

loss.backward()
```

`ml_loss`는 `energy_pos`와 `energy_neg`의 차이, 즉 original image와 negative image로 얻은 energy가 비슷해지도록 학습이 이루어진다

`im_loss`는 `im_neg`와 `im`의 직접적인 비교(MSE loss)

`loss.backward()` 로 backpropogate

#### main() 함수 
이후 main함수에서 train 함수 실행
gpu 개수에 따라 if로 갈라지지만 두 함수 모두
```python
if FLAGS.train:
        train(train_dataloader, test_dataloader, logger, models, optimizers, FLAGS, logdir, rank_idx)
```
### models.py
#### class `CondResBlock()`
`__init__` parameter : `downsample=True`, `rescale=True`, `filters=64`, `latent_dim=64`, `im_size=64`, `latent_grid=False`

논문의 다음파트 참고
> **Architecture Details.** In COMET we utilize a residual network to parameterize an underlying
energy function. We illustrate the underlying architecture of the energy function in Figure 4. The
energy function takes as input an image at 64 × 64 resolution and processes the image through a
series of residual blocks combined with average pooling to obtain a final output energy. To condition
the energy function on an input latent z, we linearly map z to a separate per channel gain and bias in
each residual block of the energy network, and use the resultant gain and bias vectors to modulate
input features [44]. We remove normalization layers from our residual network.

```python
# Upscale to an mask of image
self.latent_fc1 = nn.Linear(latent_dim, 2*filters)
self.latent_fc2 = nn.Linear(latent_dim, 2*filters)
```
filter 2배 사이즈로 upscale한 다음

```python
def forward(self, x, latent):
    x_orig = x

    latent_1 = self.latent_fc1(latent)
    latent_2 = self.latent_fc2(latent)

    gain = latent_1[:, :self.filters, None, None]
    bias = latent_1[:, self.filters:, None, None]

    gain2 = latent_2[:, :self.filters, None, None]
    bias2 = latent_2[:, self.filters:, None, None]

    x = self.conv1(x)
    x = gain * x + bias
    x = swish(x)

    x = self.conv2(x)
    x = gain2 * x + bias2
    x = swish(x)

    x_out = x_orig + x

    if self.downsample:
        x_out = swish(self.conv_downsample(x_out))
        x_out = self.avg_pool(x_out)

    return x_out
```
forward에서 gain, bias로 channel을 나눠가짐

#### class `CondResBlockNoLatent()`
위의 class와 bn, conv, activation은 같은데 gain, bias로 쪼개주는 과정이 없음

#### class `ToyEBM()`
`__init__()` parameters : `self`, `args`, `dataset`

```python
filter_dim = args.filter_dim
latent_dim_expand = args.latent_dim * args.components
latent_dim = args.latent_dim
im_size = args.im_size
```
args에 저장되어있는 다음 argument들을 가져옴

layer 정의(conv, linear, 위에서 정의한 CondResBlock, CondResBlockNoLatent)
`self.energy_map()`
```python
self.energy_map = nn.Linear(filter_dim * 2, 1)
```

`embed_latent()` parameters : `self`, `im`
```python
def embed_latent(self, im):
    x = self.embed_conv1(im)
    x = F.relu(x)
    x = self.embed_layer1(x)
    x = self.embed_layer2(x)
    x = x.mean(dim=2).mean(dim=2)
    x = x.view(x.size(0), -1)
    x = F.relu(self.embed_fc1(x))
    output = self.embed_fc2(x)

    return output
```
image 받아 latent로 embed

`forward()` parameter : `self`, `x`, `latent`
```python
def forward(self, x, latent):
    x = swish(self.conv1(x))
    x = self.avg_pool(x)
    x = self.layer_encode(x, latent)
    x = self.layer1(x, latent)
    x = self.layer2(x, latent)
    x = x.mean(dim=2).mean(dim=2)
    x = x.view(x.size(0), -1)

    x = swish(self.fc1(x))
    energy = self.energy_map(x)

    return energy
```
x와 latent를 받아 energy를 return








