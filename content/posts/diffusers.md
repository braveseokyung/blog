---
title : 'Diffusers 라이브러리(DDPM, DDIM)'
date : 2025-02-05T22:08:05+09:00
draft : false
authors : ["braveseokyung"]
description : DDPM과 DDIM을 쓸 수 있는 Diffusers 라이브러리에 대해 알아보자!
categories : ["딥러닝"]
toc:
    auto: false
---
<!--more-->

## Overview

Diffusers 라이브러리는 DDPM, DDIM 등의 diffusion model을 쉽게 적용해볼 수 있는 오픈소스입니다. 이 글에서는 Diffusers 라이브러리의 사용법과 구성을 알아보도록 하겠습니다!

### Training

- diffusion model은 스텝바이스텝으로 gaussian noise를 denoise해서 image가 나오도록 학습됨
- neural network는 각 스텝에서 slightly denoise한 다음 스텝을 predict하도록 학습됨
- certain number of step 후에 sample이 얻어짐

{{<image src="img-diffusers/Untitled.png">}}

### Architecture

{{<image src="img-diffusers/Untitled%201.png">}}

- U-Net을 통해 denosing : input과 같은 size인 image를 predict
- downsample: input을 받아서 매 layer마다 이미지 사이즈를 절반으로 줄이는 resnet layer를 통과시컴
- upsample: 같은 수의 block을 통과시켜 upsample
- skip connection: 대응하는 downsample path와 upsample path를 link feature

## Core API

1. Model 
    
    U-Net architecture
    
2. Scheduler
    
    noise scheduling algorithm이 필요
    
    - inference를 위해 몇번의 diffusion step이 필요한지
    - model output에서 어떻게 less noisy한 image를 compute할지
3. Pipeline
    - pipeline이 model과 scheduler를 그룹으로 묶음
    -> end-user가 full denoising loop process를 돌리기 쉬움

## Pipelines

### Install & Import

{{<image src="img-diffusers/Untitled%202.png">}}

- from_pretrained() method
    
    hugging face에서 google/ddpm-celebhq-256 model을 가져옴
    
    - model : 2D UNet
    - Scheduler : DDPM scheduler

### Generate Images

{{<image src="img-diffusers/Untitled%203.png">}}

- run pipeline
- random initial noise sample을 생성하고
- iterate diffusion process

### Models

{{<image src="img-diffusers/Untitled%204.png">}}

- Load another model
    - google/ddpm-church-256 model
    - `unconditional` image generation
    - model configuration
        - sample_size: input sample의 h,w
        - in_channels
        - down_block_types, up_block_types: UNet architecture
        - block_out_channels : downsampling block에서 output channel size
        - layers_per_block : 각 UNet block에서 ResNet block 수
- Inference trained model
    
    {{<image src="img-diffusers/Untitled%205.png">}}
    
    - image와 같은 shape의 random Gaussian sample (batchsize*in_channels*sample_size*sample_size)
    - noisy sample을 model에 통과시켜서 timestep
    - 같은 size의 noisy residual로부터 slightly less noised image를 얻는다
- Train your own model
    
    {{<image src="img-diffusers/Untitled%206.png">}}
    
    - 같은 config의 randomly initialized model을 만듦
    - model weight(`diffusion_pytorch_model.bin`)와 config(`config.json`)를 저장
    - model을 재사용한다면 use from_pretrained() method

## Schedulers

### Scheduler algorithms

{{<image src="img-diffusers/Untitled%207.png">}}

- training 과정에서 noise를 더할 때 쓰는 noise schedule을 정의
- given model output(noisy_residual), slightly less noisy sample을 얻을 수 있는 algorithm을 정의
- DDPM paper에서 제안하는 DDPM scheduler를 사용
- config
    - num_train_timestep : denoising process의 length
    - beta_schedule : type of noise schedule
    - beta_start, beta_end : smallest noise value, highest noise value of schedule

### Define denoising loop

{{<image src="img-diffusers/Untitled%208.png">}}

denoising process는 timestep에서 보통 decreasing order로 간다 즉 1000→0 처럼

1. model을 가지고 less noisy sample의 residual을 predict
2. scheduler를 가지고 compute less noisy sampling

### Define denoising loop with DDPM scheduler

{{<image src="img-diffusers/Untitled%209.png">}}

### Define denoising loop with DDIM scheduler

{{<image src="img-diffusers/Untitled%2010.png">}}

## Experiments guideline

- step 수가 적을 때 : DDIM(deterministic)이 DDPM(stochastic)보다 더 좋은 generation
- diffusion model로 semantic interpolation을 하고자 할 때 : DDIM
