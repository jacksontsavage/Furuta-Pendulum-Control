# Phase 2 Plan: Balancing & Swing-Up Control

### 1. Verify system controllability and observability
- Study: Chapter 7 in Feedback Systems (Astrom & Murray)
- Controllability Proof:
  - Construct the controllability matrix C = [B, AB, A^2*B, A^3*B] using state-space matrices A and B.
  - Compute the rank of C (e.g., using rank(ctrb(A, B)) in MATLAB or numpy.linalg.matrix_rank in Python).
  - Verify that rank(C) = 4 (full rank), proving that the single motor input can steer all 4 system states.
- Observability Proof:
  - Construct the observability matrix O = [C; C*A; C*A^2; C*A^3] using state-space matrices A and C.
  - Compute the rank of O (e.g., using rank(obsv(A, C)) in MATLAB or numpy.linalg.matrix_rank in Python).
  - Verify that rank(O) = 4 (full rank), proving that measuring the two encoder angles provides sufficient information to reconstruct all 4 states.
- Conceptual Understanding:
  - Controllability: Realize that motor torque applied to the rotary arm can indirectly impart momentum to the unactuated pendulum through physical cross-coupling.
  - Observability: Recognize that tracking position changes over time reveals internal velocities without requiring physical velocity sensors (tachometers).

### 2. Design an LQR (Linear Quadratic Regulator) controller for balancing at the upright equilibrium
- Study: Chapter 6 & Chapter 7 in Feedback Systems (Astrom & Murray)
- Formulate the Cost Function:
  - Define the quadratic cost function: J = integral(x^T * Q * x + u^T * R * u) dt.
  - Select state penalty matrix Q = diag([q1, q2, q3, q4]) to weight error in rotary arm angle (theta), pendulum angle (alpha), arm velocity (theta_dot), and pendulum velocity (alpha_dot).
  - Select control penalty matrix R = [r1] to penalize excessive motor voltage and prevent actuator saturation (+/-15 V).
- Calculate Gain Matrix:
  - Solve the Continuous-Time Algebraic Riccati Equation (CARE) to determine the optimal full-state feedback gain matrix K = [k1, k2, k3, k4].
  - Implement the state-feedback law: u = -K * x (or u = -K * x_hat when using an observer).
- Iterative Tuning:
  - Adjust diagonal entries of Q and R using Bryson's Rule or manual gain-tuning to achieve rapid balancing recovery with minimal overshoot and safe control effort.

### 3. Design a Luenberger Observer or Kalman Filter to estimate unmeasured state velocities
- Study: Chapter 7 in Feedback Systems (Astrom & Murray)
- Structure the Estimator Dynamics:
  - Set up the observer differential equation: x_hat_dot = A * x_hat + B * u + L * (y - C * x_hat), where x_hat is the estimated state vector and y contains physical encoder readings.
- Design Options:
  - Luenberger Observer: Select gain matrix L via pole placement (e.g., placing observer poles 3 to 5 times faster than closed-loop LQR poles to ensure fast convergence without noise amplification).
  - Kalman Filter: Define process noise covariance Q_w and measurement noise covariance R_v (based on encoder resolution: 2048 counts/rev) to derive optimal gain L dynamically.
- Integration & Validation:
  - Feed estimated velocities (theta_dot_hat and alpha_dot_hat) directly into the LQR controller.
  - Compare true simulated velocities (x3, x4) against estimated velocities (x_hat3, x_hat4) to ensure observer error decays rapidly to zero.

### 4. Implement an energy-based swing-up controller with switching logic to hand off control to LQR when near vertical
- Study: "An industrial pendulum: a case study in control" (Astrom & Furuta, 2000)
- Energy-Based Swing-Up Controller:
  - Define total mechanical energy of pendulum: E = E_k + E_p.
  - Define target potential energy at upright vertical position: E0 = m * g * l.
  - Compute energy error E_error = E - E0 and compute a non-linear control force proportional to E_error * cos(alpha) * alpha_dot to pump energy into pendulum swing-by-swing.
- Define "Catch Region" (Thresholds):
  - Establish precise handoff criteria based on state boundaries:
    - Pendulum angle within threshold: |alpha| < alpha_threshold (e.g., +/-10 to +/-15 deg from vertical).
    - Pendulum angular velocity below threshold: |alpha_dot| < alpha_dot_threshold.
- Implement Switching Logic (State Machine):
  - Construct conditional state machine (e.g., using if/else logic or Simulink Stateflow):
    - State 0 (Swing-Up): Apply energy-shaping control voltage while |alpha| > alpha_threshold.
    - State 1 (Balancing): Switch instantaneously to LQR control law u = -K * x_hat when states fall inside catch region.
    - State 2 (Fallback / Safety): Re-engage swing-up or zero voltage if pendulum drops outside catch region or motor voltage saturates.

### 5. Test closed-loop robustness under step disturbances and sensor noise
- Study: Chapter 12 in Feedback Systems (Astrom & Murray)
- Step Disturbance Testing:
  - Inject brief step-input torque pulses (simulating external physical nudges or pushes) directly onto rotary arm and pendulum joints.
  - Plot state trajectories (theta, alpha) and control signal (u) to measure settling time, recovery speed, and peak deflection angles.
- Sensor Noise & Quantization Simulation:
  - Add discrete quantization blocks representing physical 2048 counts/rev quadrature encoders (~0.00307 rad resolution).
  - Add Gaussian zero-mean white noise to output signals (y) to simulate electrical and sensor noise.
- Actuator Constraint Verification:
  - Verify calculated control voltage u(t) remains safely within motor voltage saturation limits (+/-15 V) under noise and small disturbance conditions.