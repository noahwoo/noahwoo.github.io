
## Instruct Finetuning

- **SELF-INSTRUCT: Aligning Language Model with Self Generated Instructions, 2022**
    - In one word: improve the instruction following capabilities of pretrained LM by bootstrapping off its own generation
    - Methods: definition of instruction data -> $(I_t, [X_t, Y_t]+)$ , $X_t$ can be empty
      1. Instruction generation
        - 175 tasks pool with instruction from human annotation, more from bootstrapping in the following
        - sample 8 tasks from the pool, 6 from human annotation, 2 from model generated tasks
        - in-context learning prompt like: `Come up with a series of tasks: \n\n Task 1: {Instruction for existing task-1} \nTask 2: {Instruction for existing task-2}\n...\nTask 8: {Instruction for existing task-8\nTask 9: }`
      2. Identify if the instruction represents a classification task
        - task type(classification or not) used for instance generation
        - 12 classification and 19 non-classification few-shot prompts
      3. Instance generation: few-shot prompting with instruction-input-output demonstations
        - Input-first approach: for non-classification task
          - LLM first come up the input
          - then produce the output based on the input
        - Output-first approach: for classification task, solve the label inbalance problem
          - generate the output label first
          - generate the input condition on the output label
      4. Filter low-quality data
          - instruction diversity: use instruction with ROUGE-L overlap score < 0.7 for any existing ones
          - instance diversity and consistency: dedup instance, filter out instance with same input but different output
      5. Finetune LM to follow the instructions
    - Results: 
      - 52K instructions and 82K instances generated with good diversity and quality(92% valid instruction, 54% valid input/output and instruction)
- **The False Promise of Imitating Proprietary LLMs, 2023, UC Berkeley**
  - In one word: critically analyze the efficiency of ChatGPT distilled imitation model on specific and broad coverage tasks, 1.5B to 13B model(GPT-2 and LLaMA), 0.3M to 150M tokens, evaluated with human and GPT-4
  - Method:
    - Dataset
      - Task-specific imitation:
        - Natural Questions: plus 6000 more additional examples by in-context prompting ChatGPT with 5 QA demos. 
      - Broad-coverage imitation: ShareGPT-Mix
        - ShareGPT: 50K clean dialogue examples shared by users
        - HC3: ~27K response from ChatGPT for ~24K questions
        - Discord ChatGPT Bots: 10K input-output examples from *r/ChatGPT* and *Turing AI*
    - Finetuning objective & Evaluation dimensions 
      - standard LM loss only on the model output
      - Study how to improve imitation model by
        - increasing the amount of imitation data
        - vary the capabilities of underlying base LM
  - Conclusion:
    - imitation model improves on evaluation tasks with supported imitation training data
    - no improvement on task with no supported imitation training data
    - imitation model mimicks ChatGPT *style*, but *facturality* is week
    - broad-coverage imitation fails to close the gap across most tasks
    - gap between imitation model and ChatGPT cannot be fixed by finetuning on more imitation training data
    - increasing the imitation model size and pre-training data quality are the right direction