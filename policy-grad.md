# Policy gradient
- Policy gradient growth as a tree
   - Root: Vanila Policy Gradient(REINFORCE)
   - Growth dimension:
      - Onpolicy -> Offpolicy
      - Policy(Actor) -> Actor/Critic
      - Stochastic -> Deterministic
      - Single actor -> Multiple actors
      - Reward maximize -> Reward + Entropy maximize
      - Variance reduction
         - Sampling weight cap
         - Step size cap
   - Growth tree

# Algorithm summary as a table

Stochastic/Deterministic | On/Off-policy |   Algorithm    | Objective | Gradient | Keynotes
------------------------ | ------------- | --------- | --------- | -------- | ------------
Stochastic               | Onpolicy      | REINFORCE |  $J(\theta) = \sum_s \mu^{\pi}(s) \sum_a \pi_{\theta}(a\|s) Q^{\pi}(s,a)$ | $\nabla_{\theta} J(\theta) = E_{s \sim \mu^{\pi}, a \sim \pi_{\theta}(\cdot \| s)} \left[ Q^{\theta}(s, a) \nabla_{\theta} \ln \pi_{\theta}(a \| s) \right ]$  |       