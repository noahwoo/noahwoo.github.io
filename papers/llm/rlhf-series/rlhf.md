
## RLHF series:
- **Illustrating Reinforcement Learning from Human Feedback (RLHF), 2022, Hugging Face Blog**
   - [Github Link](https://github.com/huggingface/blog/blob/main/rlhf.md)
- **Deep reinforcement learning from human preferences, 2017, OpenAI**
- **A survey of preference-based reinforcement learning methods, 2017, JMLR**
- **[PPO implementation on spinningup](https://github.com/openai/spinningup/tree/master/spinup/algos/pytorch/ppo)**
   - In brief: 
      - vanilla PPO-2 implementation
      - GAE for advantage function
      - Parallel in trajectories generation and gradient calculation with MPI(data parallel)
   - Several notes:
      - Main loop: 
         - collect trajectories with current policy, `steps_per_epoch` for each epoch, distributed in `np=cpu` processes
            - for each trajectory step, sample action, `step` the env and record tuple $\{s_t, a_t, r(s_t, a_t), V_{\psi_{old}}(s_t), \log \pi_{\theta_{old}}(a_t|s_t)\}$, in code variable `(obs, act, rew, val, logp)`
            - when trajectory `done`, accumulate the decayed advantange for policy update and target for value function update
               - $\delta_t = r(s_t, a_t) + \gamma V_{\psi_{old}}(s_{t+1}) - V_{\psi_{old}}(s_t)$
               - `adv`: $A(s_t, a_t) = \sum_{t'=t}^T {(\gamma \lambda)}^{t'-t} \delta_{t'}$
               - `ret`: $G_t = \sum_{t'=t}^T \gamma^{t'-t} r(s_{t'}, a_{t'})$
         - update with gradient from loss, with tuple `(obs, act, ret, adv, logp)`
            - define $r_t(\theta) = \frac{\pi_{\theta}(a_t|s_t)}{\pi_{\theta_{old}}(a_t|s_t)}$
            - policy(parametered by $\theta$): $\frac{1}{N} \sum_{\text{episode}} \sum_t \min (r_t A(s_t, a_t), \text{clamp}(r_t, 1-\epsilon, 1+\epsilon) A(s_t, a_t))$
            - value function(parametered by $\psi$): $\frac{1}{N} \sum_{\text{episode}} \sum_t (\sum V_{\psi}(s_t) - G_t)^2$
         