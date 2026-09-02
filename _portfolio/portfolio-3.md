---
title: "Mixture of Attention"
excerpt: "This project is a very basic trial on transplanting the mean idea of MoE into attention mechanism. Because this idea was accomplish by others before my research group, I only carried out a preliminary attempt. However, this research experience had greately elicited my passion on LLMs and really helped me on understanding attention mechanism. <br/><img src='/images/quadtree_partition_result.png' width='500'> "
collection: portfolio
---
<script>
  window.MathJax = { tex: { inlineMath: [['$', '$'], ['\\(', '\\)']] } };
</script>
<script async src="https://cdn.jsdelivr.net/npm/mathjax@3/es5/tex-chtml.js"></script>

# Transformer structure of ViT
First we need to be aware that as a simple transformer-based model, ViT has 12 layers of encoder without any decoder. Plus, for each encoder, it has 12 heads. 

Thus, for one inference, there will be 144 attention matrices in total. 

Plus, for an attention matrix, it's size is sequence length $\times$ sequence length. So, generally, the size of attention score always varies with the sequence length. However, for ViT, it always splits images into $n$ patches (in our specific case, $n=196$), thus, the attention matrix here is fixed to $196\times 196$. 

# 144 attention matrices in one inference
We use one example in CIFAR-10 to do the inference on ViT and visualized its attention matrices to have a basic observation. 
<figure>
  <img src="/images/attention_layer_0_head_0_3d.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples.">
  <figcaption>Figure 1: An example of visualized attention matrix. Here we take the attention matrix of the first head of the first layer as an example.  
  </figcaption>
</figure>

# General rule of the distribution of attention
However, we cannot tell anything from results given by only one time of inference. So, I was very curious that what will happen if I let the model perform inference on all samples and directly take the average of all attention score for every head in every layer. At first, I assumed that each position will be noticed because there are various samples. Thus, the attention matrix should be flatten. 
However, when I saw the experiment results, I got very shocked because even after being averaged, those attention matrices were still displaying distinct distribution patterns. 
<figure>
  <img src="/images/attention_layer_0_head_0_3dwhole.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples.">
  <figcaption>Figure 2: Still, we take the attention matrix of the first head of the first layer as an example. 
  </figcaption>
</figure>
As figure 2 illustrated above, we could tell that the attention matrix of the first head of the first layer still shows a very obvious distribution rule. Results also show that other attention matrices are all displaying specific features. 

# Investigation of other research to validate our observation
To validate my observations, I investigated researches of others like [Big Bird](https://arxiv.org/abs/2007.14062), which also indicates that the distribution of attention scores obey several specific modes.
<figure>
  <img src="/images/BigBird.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples.">
  <figcaption>Figure 3: Attention score distribution modes proposed by Big Bird.
  </figcaption>
</figure>

# Blockwise attention
Based on our initial findings, we tried to first realize the blockwise attention to see whether it could make a difference. 
So, I directly take the average of all attention matrices and normalized the result. 
Second, I take a threshold and if the attention score surpasses this threshold, it will be marked as $1$; and if it does not, it will be marked as $0$. Result is visualized below:
<figure>
  <img src="/images/blockwise_attention.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples."
       style="display: block; margin-left: auto; margin-right: auto; max-width: 60%; height: auto;">
  <figcaption>Figure 4: Attention score distribution modes proposed by Big Bird.
  </figcaption>
</figure>

My original idea is to partition this attention matrix into blocks based on the importance map. And for blocks that are marked as $0$, they will be roughly computed, for example, straight take the average and regard elementes in those blocks as one element; for those are marked as $1$, elements in them will be calculated precisely. The result of partitioning is shown as:
<figure>
  <img src="/images/quadtree_partition_result.png" 
       alt="A flowchart showing a process where a model's thinking steps are guided by high-quality examples."
       style="display: block; margin-left: auto; margin-right: auto; max-width: 60%; height: auto;">
  <figcaption>Figure 4: Attention score distribution modes proposed by Big Bird.
  </figcaption>
</figure>

# To be continue
When I advanced our research progress to this point, other research groups like Kimi and DeepSeek have already accomplished the whole idea. So, I had to seek for other ideas to research.
However, this research experimence really elicited my interest in LLMs, especially the explainable artificial intelligence of LLMs. I am very curious about why the distribution of attention scores appear in those specific modes; will those modes vary with different model and what roles do the indices of layers and heads play in the determination of those modes...