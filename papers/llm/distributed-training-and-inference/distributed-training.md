
## Distributed training and inference

- Data parallel
    - **[DDP PyTorch](https://pytorch.org/tutorials/intermediate/ddp_tutorial.html)**: 
        - In one word: all-reduce after back-propagation
        ```
        forward pass :
        for layer_i in layers: 
            forward pass for layer_i
        
        backward pass :
        for layer_i in layers:
            backward pass for layer_i
            full: all-reduce gradients for layer_i
            full: update momentum & variance
            full: update weights
        ```å
        
    - **ZeRO: Memory Optimizations Toward Training Trillion Parameter Models, 2019**
        - shards optimizer states, gradients, parameters to data parallel workers
        - each worker updates the local parameters by forward/backward
        - global parameter and gradient synced by all-gather and reduce-scatter in each layer, RAM released for next layer
    - **[FSDP: PyTorch offical version of ZeRO](https://pytorch.org/blog/introducing-pytorch-fully-sharded-data-parallel-api/)**
        - In one word: replacement for DDP, pesudo-code in nutshell:
        ```
        forward pass :
        for layer_i in layers:
            all-gather full weights for layer_i
            forward pass for layer_i
            discard full weights for layer_i
        backward pass:
        for layer_i in layers:
            all-gather full weights for layer_i
            backward pass for layer_i
            discard full weights for layer_i
            part: reduce-scatter gradients for layer_i
            part: update momentum & variance
            part: update weights
        ```
- Model parallel
    - Tensor Parallel: horizontal partition
        - **[Mesh TensorFlow - Model Parallelism Made Easier](https://github.com/tensorflow/mesh)** 
        - In one word: for TensorFlow
        - **Megatron-LM: Training Multi-Billion Parameter Language Models Using Model Parallelism, 2019**
        - In one word: for PyTorch
    - Pipeline Parallel: vertical partition
        - **GPipe: Easy Scaling with Micro-Batch Pipeline Parallelism, 2018**
        - **[PipeDream](https://github.com/PipedreamHQ/pipedream)** 
        - In one word: improve pipeline efficiency by storing multiple copies of weights
        - **TeraPipe: Token-Level Pipeline Parallelism for Training Large-Scale Language Models, 2021**: 
        - In one word: designed for Transformer, pipeline occurs across tokens instead of mini-batch
- All in one
    - **Colossal-AI: A Unified Deep Learning System For Large-Scale Parallel Training, 2021**
    - **[Megatron-Deepspeed](https://github.com/microsoft/Megatron-DeepSpeed)** 
- Quantization for training & inference
    - **[Large Transformer Model Inference Optimization, 2023, Lil's Log](https://lilianweng.github.io/posts/2023-01-10-inference-optimization/)** 
    - **LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale, 2022, Tim**
    - In one word: vector-wise quantization for row and column in matrix multiplication, with a new mix-precision decomposition scheme; applied in head projection and FFN layers in transformer
    - Methods:
        - Observations:
          - outlier features definition(each column of $[B \times T, D]$ as a feature): 
            - magnitude larger than 6
            - affect more than 25% layers 
            - affect at least 6% sequence(batch) dimensions
          - outlier features emergent with model scales, 6.7B is critical point
          - outlier features highly systematic: 6.7B scale, 150K outliers occur in 6 feature dimensions(columns)
        - Quantization in vector wise $XW$: 
          - row int8 quantization for $X$: $B \times T$ constants
          - column int8 quantization for $W$: $D$ constants
        - Columns(X) and related rows(W) with outlier features extracted and do FP16 multiplication(< 0.1%)
    - Opensource implementations: [bitsandbytes](https://github.com/TimDettmers/bitsandbytes)
  - **QLoRA: Efficient Finetuning of Quantized LLMs, 2023, Tim**
    - In one word: a quantization method to finetune 65B model on single 48GB GPU, preserve 16bit finetuning performance
    - Methods:
      - 4-bit NormalFloat quantization data type
        - obs: neural network weights in zero-centered normal distribution with std $\sigma$
        - precompute information theoretically optimal data type for zero-mean and std $\sigma$ in range [-1, 1] 
          - $q_i = \frac{1}{2} (Q_X(\frac{i}{2^k+1}) + Q_X(\frac{i+1}{2^k+1}))$
        - zero included by two data types for positive and negative part both with zero
      - Double quantization
        - problem: 32 bit const for 64 blocksize quantization: 32/64=0.5 more bit for each weight
        - quantization the ```absmax ``` const for each block with a 8-bit Float in blocksize 256
          - reduce to: 8/64(first quant size) + 32/(64*256)(second quant size) = 0.127 bit per weight
      - Paged Optimzer from GPU to CPU memory
      - QLoRA: 
        - store weights in 4-bit NormalFloat
        - dequantization to 16-bit BF for forward/backward pass
        - only compute weight gradients for LoRA weights in 16-bit BF 
    - Results:
      - 4 bit NormalFloat outperform 4-bit Float Point
      - k-bit QLoRA match 16bit finetuing and 16-bit LoRA performance
      - Guanaco Chatbot:
        - Finetuning from LLaMA 7B, 33B, 65B with OASST1 dataset
        - Elo Rating ranked #2 (only after GPT-4) when judged by GPT-4 on Vicuna benchmark
      - edge device estimation:
        - iPhone 12 Plus, finetune 3 million tokens per night
  - **The case for 4-bit precision: k-bit Inference Scaling Laws, 2022, Tim**
  - **8-BIT OPTIMIZERS VIA BLOCK-WISE QUANTIZATION, 2022, Tim**
- Others
    - **1-bit Adam: Communication Efficient Large-Scale Training with Adam’s Convergence Speed, 2021, Microsoft**
    - In one word: reduce the communication bandwidth(from FP32 to binary) with error-compensated compression, leverage the findings that variance of Adam becomes stable during training
    - Two-stages method in details: 
        - Use vanilla Adam for the warm-up stage (~20% of the training steps)
        - In compression stage, fix the variance updates in Adam, communicate the momentum with error compensated 1-bit compression
        - In parameter server settings, communication of worker local momentum and server global momentum get compressed before sending
    - Theoretical results
        - Prove of convergence
        - Communication bandwith reduce to 1/32 for FP32 and 1/16 for FP 16 
- Scaling law
  - **Scaling Laws for Neural Language Models, 2021, OpenAI**