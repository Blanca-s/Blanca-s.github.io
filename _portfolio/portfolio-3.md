---
title: "G^2RPO-A: Guided Group Relative Policy Optimization with Adaptive Guidance"
excerpt: "This paper investigates the training instability of Generative Reinforcement Preference Optimization (GRPO) when applied to smaller-scale Large Language Models (LLMs). We identify that the inherent difficulty for these models to generate high-quality completions leads to a sparse reward problem, thereby impeding the training process. To address this, we introduce a guidance mechanism into the GRPO framework. While this guidance effectively mitigates the sparse reward issue, it concurrently introduces the problem of advantage vanishing. Consequently, this work proposes novel methods to counteract this effect, ultimately achieving superior performance over the vanilla GRPO baseline. <br/><img src='/images/guided-overview.png' width='500'> "
collection: portfolio
---

Fine-tuning is a key technique within post-training. It has become a primary focus for researchers, largely due to the immense computational cost of pre-training.

This project is a summary of my own experience on locally deploying ViT and trying to make it work fine on CIFAR-10 dataset. 

The most precious part of this experience is that I did not know that the ViT-base model would perform so badly on CIFAR-10 dataset and at the beginning, I was just trying to help it operate normally. As a result, until I finished whe whole progress, I did not know that I was actually "fine-tuning". I understood the concept of "fine-tuning" better by this novel perspective rather than simply reading theory. 

I believe other begginers can also benifit from this method. 
