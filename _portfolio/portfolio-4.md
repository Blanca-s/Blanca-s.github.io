---
title: "G^2RPO-A: Guided Group Relative Policy Optimization with Adaptive Guidance"
excerpt: "This paper investigates the training instability of Generative Reinforcement Preference Optimization (GRPO) when applied to smaller-scale Large Language Models (LLMs). We identify that the inherent difficulty for these models to generate high-quality completions leads to a sparse reward problem, thereby impeding the training process. To address this, we introduce a guidance mechanism into the GRPO framework. While this guidance effectively mitigates the sparse reward issue, it concurrently introduces the problem of advantage vanishing. Consequently, this work proposes novel methods to counteract this effect, ultimately achieving superior performance over the vanilla GRPO baseline. <br/><img src='/images/guided-overview.png'> "
collection: portfolio
---
# Guidance mechanism in GRPO
The guidance mechanism can be easily illustrated as:
<figure>
  <img src="/images/guided-overview.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples.">
  <figcaption>Figure 2: Illustration of roll-outs with guidance. This process uses high-quality thinking trajectories to improve model performance.
  </figcaption>
</figure>

To demonstrate the efficacy of our approach, we analyzed the rewards assigned to an initial batch of 280 completions from both vanilla GRPO and Guided GRPO. A heatmap visualization of these reward distributions clearly shows that the guidance mechanism effectively mitigates the sparse reward problem, yielding a significantly denser reward signal.

<figure>
  <img src="/images/comparison.png" 
       alt="A heatmap or grid visualization showing the reward distribution for Guided GRPO, where Qwen3-1.7B was fine-tuned on coding tasks. The chart displays a 10x7 matrix, representing the average pooled rewards from 10 roll-outs and 280 candidates per batch.">
  <figcaption>Figure 2: Reward distribution for Guided GRPO using Qwen3-1.7B. This chart visualizes the average pooled rewards from 10 roll-outs and 280 candidates per batch, fine-tuned on coding tasks.
  </figcaption>
</figure>

# The limitation of simple guidance
However, simply injecting guidance into the prompt during GRPO trainig does not really help to elicit models with better performance. As a result, we analyze the reasons behind this ineffectiveness, deducing two main points: 1) Completions for one query cannot be all guided. 2) Guidance length matters in the training process. 

To figure out why simply injecting guidance in the prompt cannot effectively elicit the reasoning ability of small-scale LLMs, we analyze the advantage standard deviation during Guided GRPO training and find that when all completions for one query are guided, the advantage $\sigma$ are extremely low, meaning that the driver of strategy updates has almost vanished. 

<figure>
  <img src="/images/advantage σ.png" 
       alt="">
  <figcaption>Figure 2: An analyze of advantage standard deviation.  
  </figcaption>
</figure>

Thus, we propose that completions for one query should be partially guided, and the guiding ratio depends on the types of tasks and the size of models. 

# Other components assist guidance mechanism
## Hard queries
In many data filtration work, hard queries are removed from the trainig dataset and queries of medium difficulty are recommended. However, because the introduction of guidance, hard queries now can be more easily solved by models. Thus, the re-introducing of hard queries are considerable. 

## Adaptive guidance length
Because the real-time performance and the difficulty level of the problem are uncertain at each training step, it is hard to make a pre-defined rule to decide how much guidance to provide at a certain step. Thus, we proposed that maybe an adaptive guidance selection mechanism is the key to the solution. The core of this idea is to decide the guidance length by the variance of success rate.