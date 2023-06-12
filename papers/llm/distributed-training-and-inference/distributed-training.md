
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
    - **LLM.int8(): 8-bit Matrix Multiplication for Transformers at Scale, 2022, Huggingface**
    - In one word: vector-wise quantization for row and column in matrix multiplication, with a new mix-precision decomposition scheme; applied in head projection and FFN layers in transformer
    - Methods:
        - Observations:
        - outlier features emergent with model scales, 6.7B is critical point
        - outlier features highly systematic: 6.7B scale, 150K outliers occur in 6 feature dimensions(columns)
        - Quantization in vector wise $XW$: 
        - row int8 quantization for $X$
        - column int8 quantization for $W$ 
        - Column or row with outlier features ($\ge 6.0$) extracted using FP16 multiplication(< 0.1%)
    - Opensource implementations: [bitsandbytes](https://github.com/TimDettmers/bitsandbytes)
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