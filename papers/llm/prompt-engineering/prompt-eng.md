
## Prompt(hard/soft) engineering

- Survey:
    - **Pre-train, Prompt, and Predict: A Systematic Survey of Prompting Methods in Natural Language Processing, 2021, CMU** (OK)
    - **[Prompt Engineering](https://lilianweng.github.io/posts/2023-03-15-prompt-engineering/), 2023, OpenAI**
- Parameter efficient tuning
    - **Prefix Tuning: Optimizing Continuous Prompts for Generation, 2021, Stanford** (OK)
        - **[Code in Github](https://github.com/XiangLi1999/PrefixTuning)**
        - Autoregressive LM(GPT2) and encoder-decoder architecture(BART)
        - GPT2 in details: 
            - based on GPT2 huggingface implementation(oooold version)
            - hooked in GPT2 by the `past_key_values` arguments in `forward` method
            - `past_key_values` constructed in the following steps:
                - `pre_seq_len` hidden tokens with `n_embd`-dim embedding vector
                - embedding vector passed through an MLP with one hidden layer with default size 512, and output size `2 * n_layers * n_emb(=num_head * head_n_embd)`, here `2` for key and value vectors
                - shape of `past_key_values`: `(2, batch_size, num_head, seq_len, head_n_embd)`
    - **Prompt Tuning: The Power of Scale for Parameter-Efficient Prompt Tuning, 2021, Google** (OK)
    - **P-tuning/P-tuning V2: Prompt Tuning Can Be Comparable to Fine-tuning Universally Across Scales and Tasks, 2021, Tsinghua** (OK)
    - **Unified View: Towards a unified view of parameter-efficient transfer learning, 2022, CMU** (OK)
        - In one word: Unify Adpater/LoRA/Prefix-tuning into the modification to specific hidden states(heads) in pretrained model
        - Aspects of modification
        - target: head-attention(Prefix Tuning), attention(Adapter/LoRA), ffn(Adapter), key/value tranform matrix(LoRA)
        - composition: $h \leftarrow h + s \Delta h$ or $h \leftarrow (1-\lambda(x)) h + \lambda(x) \Delta h$ (PrefixTuning)
        - modifier $\Delta h$: 
            - low rank bottleneck: $f(v W_1)W_2, W_1 \in R^{d \times l}, W_2 \in R^{l \times (d|d_h)}$
            - $v$: PLM layer input $x$(Prefix Tuning/LoRA) or (head-)attention $h$(Adapter) 
            - $f$: Identity mapping(LoRA), ReLU activation function(Adapter), Softmax function(Prefix Tuning)
            - parallel(PrefixTuning/LoRA) or sequential(Adapter)
            - scaling or not: yes(LoRA), no(Prefix Tuning/Adapter)
        - Results
        - Claimed comparable performance to fine-tuning not generalize well to other benchmark
        - parallel adapter beats sequential adapter
        - ffn modification utilize the added parameters more effectively than (head-)attention, except for the case with less than 0.1% parameters added
        - scaling composition function better than vanilla additive one
        - Mix-And-Match adapter utilizing the good of Prefix-tuning and Adapter works better
    - **Intrinsic Dimensionality Explains the Effectiveness of Language Model Fine-Tuning, 2020, Facebook** 
    - **Revisiting Parameter-Efficient Tuning: Are We Really There Yet?, 2022**
    - **Parameter-efficient fine-tuning of large-scalepre-trained language models, 2023, Tsinghua**
    - **Scaling Down to Scale Up: A Guide to Parameter-Efficient Fine-Tuning, 2023, UMass**
    
    - **[LoRA for Stable Diffusion Finetuning](https://huggingface.co/blog/lora)**
        - **[Github](https://github.com/cloneofsimo/lora)**
        - **DreamBooth: Fine Tuning Text-to-Image Diffusion Models for Subject-Driven Generation, 2022, Google**
          - In one word: personalization of text-to-image diffusion models by finetuning
        - **An Image is Worth One Word: Personalizing Text-to-Image Generation using Textual Inversion, 2022, NVIDIA**
        - **Pivotal Tuning for Latent-based Editing of Real Images, 2021**
        - **[The Illustrated Stable Diffusion](https://jalammar.github.io/illustrated-stable-diffusion/), 2022**
        - **[The Annotated Diffusion Model](https://huggingface.co/blog/annotated-diffusion), 2022**
- Automation:
    - **Large Language Models Are Human-Level Prompt Engineers, 2023**
- Applications: 
    - **Prompt tuning GPT-2 language model for parameter-efficient domain adaptation of ASR systems, 2022, Amazon** (OK)
- Agents:
  - **Generative Agents: Interactive Simulacra of Human Behavior, 2023, Stanford**
