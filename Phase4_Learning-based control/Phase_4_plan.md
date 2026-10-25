

## Detailed Phase 4 Breakdown

* **Study Chapter 1–4: Bertsekas, Reinforcement Learning and Optimal Control**
  * Review foundational Dynamic Programming principles, cost-to-go approximations, and policy/value iteration algorithms.
  * Understand theoretical guarantees and limitations of approximate dynamic programming for underactuated nonlinear systems.
* **Implement a simple learning-based controller in simulator (policy iteration or small actor-critic)**
  * **Learn Machine Learning on MATLAB:** Configure Reinforcement Learning Toolbox environment using continuous state and action specifications.
  * **Try different ML Models:** Test lightweight control architectures with bounded action spaces matching hardware limits.
  * Design a shaped reward function penalizing state tracking errors, angular velocities, control effort, and abrupt action changes.
* **Compare against LQR baseline on disturbance and noise scenarios**
  * **Use frequency and time domain analysis:** Benchmark transient responses, settling times, and peak deviations under step disturbances and sensor noise.
  * Evaluate how gracefully the learning-based policy handles unmodeled dynamics and parameter drift.
* **Use MathWorks Qube-Servo reinforcement learning examples as a reference point**
  * **Compare and contrast with published results:** Cross-reference training convergence metrics, reward curves, and policy stability.
  * Document performance divergences between custom simulator setup and MathWorks virtual twin environment.
