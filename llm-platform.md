---
title: Platform for Application of Large Language Model
author: MEG-Feed
date: 3/13/2023
---

# Langchain
- 6 areas:
   - LLMs wrapper and Prompts templates
   - Chains:
   - Data Augumented Generation:
   - Agents: Dynamic Chains
   - Memory:
   - Evaluation:
- Open source
   - [Langchain github](https://github.com/hwchase17/langchain)

# Fixie
- [Github](https://github.com/fixie-ai/fixie-sdk)
- [Fixie Official Site](https://www.fixie.ai/)

# Open AI LLM
- 4 areas:
   - Completion
   - Embedding
   - Fine-tune
   - GPT-index (3'rd) 
      - Similar to `Data Augumented Generation' in langchain

# GPT-index
- [LLamaIndex](https://github.com/jerryjliu/llama_index)

# Hugging face transformer related model
- [PEFT](https://github.com/huggingface/peft)
- [TRLX](https://github.com/CarperAI/trlx)
- [trl with peft demo](https://huggingface.co/blog/trl-peft)

# Deepspeed
- [Deepspeed Github](https://github.com/microsoft/DeepSpeed)

# ColossalAI
- [Colossal Homepage](https://colossalai.org/)
- [Colossal Github](https://github.com/hpcaitech/ColossalAI)
- In one word: an array of acceleration techniques constructed in a modular way for transformer
- Parallel paradigm covered
   - Data parallel: all-reduce, all-gather, reduce-gather
      - Input split along batch
      - DDP/FSDP
      - Deepspeed
   - Model parallel
      - Tensor parallel
         - matrix multiply in 1D, 2D, 2.5D, 3D
         - Megatron-LM
         - Mesh Tensorflow
      - Pipeline parallel
         - Gpipe
         - DreamPipe
   - Sequence parallel: for long sequence in AlphaFold and long text application
      - Input split along sequence, full model replica in each device
      - Ring Self-Attention
- Design and implementation
   - Multi-dimensional model parallelism
      - support 1D, 2D, 2.5D and 3D tensor parallel, lower communication overhead for high-D parallel
      - support sequence and pipeline parallel
   - Enhanced sharding and offloading
      - re-design the sharding and offloading module of Deepspeed/ZeRO for better performance
      - support customizable sharding strategies
      - support user defined life-cycle hooks for training workflow
      - further optimizations
         - re-use the fp16 storage space of parameters for gradients in backward pass
         - adaptive hybrid Adam optimizier in offloading mode 
# Microsoft's Azure

# AI21