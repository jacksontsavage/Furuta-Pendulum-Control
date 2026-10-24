
# Rotary Inverted Pendulum Control

#### Author: Jackson Savage
#### Advisor: Suat Gumussoy
#### Closed Loop AI Lab, University of Texas at Dallas

This repository contains theory and simulation framework for the Quanser Qube-Servo 3 rotary inverted (Furuta) pendulum. Developed as a research project at UT Dallas, this digital twin serves as the foundation for physical hardware experiments and sim-to-real control transfer.

The project explores classical optimal control alongside modern reinforcement learning (RL) techniques to handle non-linear system dynamics, sensor limitations, and physical parametric uncertainties.

The project will be divided into phases that compound on each other. Phase 1-3 will be about classical control theory. The last phase will connect with the focus of the Closed Loop AI Lab.

### Phase 1: Modeling and Simulation
* Derive nonlinear equations of motion from first principles using **Lagrangian / Newton-Euler** mechanics.
* Linearize about the upright unstable equilibrium to obtain a state-space model (x = Ax + Bu, y = Cx + Du).
* Build a digital twin simulation in MATLAB/Simulink or Python incorporating:
  * Motor voltage saturation
  * Encoder quantization
  * Realistic fixed sample time
* Validate open-loop simulation behavior against published Qube-Servo 3 parameters.

### Phase 2: Balancing & Swing-Up Control 
* Verify system **controllability** and **observability**.
* Design an **LQR (Linear Quadratic Regulator)** controller for balancing at the upright equilibrium.
* Design a **Luenberger Observer** or **Kalman Filter** to estimate unmeasured state velocities.
* Implement an **energy-based swing-up controller** with switching logic to hand off control to LQR when near vertical.
* Test closed-loop robustness under step disturbances and sensor noise.

### Phase 3: Robustness & Sensitivity Analysis
* Inject parametric uncertainty in pendulum mass, length, and friction parameters.
* Perform **Monte Carlo simulations** and stability margin evaluations.
* Identify key parameter sensitivities critical for **sim-to-real transfer** on physical hardware.

### Phase 4: Learning-Based Control
* Implement a model-free / learning-based controller (e.g., Policy Iteration or Actor-Critic) in the simulator.
* Benchmark RL performance directly against the classical LQR baseline under identical disturbance and noise scenarios.
* Utilize MathWorks Qube-Servo RL examples as reference validation.
