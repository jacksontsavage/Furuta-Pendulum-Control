## Phase 3: Robustness Analysis

### 1. Introduce parametric uncertainty in pendulum mass, length, and friction

* Study: Chapter 12 & Chapter 13 in Feedback Systems (Astrom & Murray)

* Mass and Inertia Variation:

  * Apply variations (+/-10% to +/-30%) to pendulum mass (m_p) and arm mass (m_r) to account for physical manufacturing tolerances and wear.

  * Recalculate rotational inertias (J_p, J_r) dynamically based on mass and center-of-mass shifts.

* Length and Geometry Perturbations:

  * Introduce variations (+/-5% to +/-15%) in pendulum length (L_p) and rotary arm radius (r_r) to model dimensional uncertainty.

* Joint Damping and Friction Uncertainties:

  * Vary viscous damping coefficients (b_p, b_r) by up to +/-50% to reflect lubrication changes, temperature variations, and bearing wear over time.

  * Model unmodeled static (Coulomb) friction at both rotary joint axes.

### 2. Evaluate closed-loop stability under parametric perturbations

* Stability Margins Analysis:

  * Compute classical gain and phase margins for the linearized loop-at-a-break points using frequency response techniques (Bode / Nyquist plots).

  * Evaluate disk margin or vector margin to determine multivariable robustness to simultaneous gain/phase variations.

* Monte Carlo Simulation Runs:

  * Perform automated batch simulations (e.g., 100 to 500 trials) using randomly sampled parameter values from defined normal or uniform distributions.

  * Track performance metrics across runs: time-to-settle, maximum overshoot angle, control energy usage (integral of u^2 dt), and failure/fall-over rate.

* Worst-Case / Uncertainty Bound Testing:

  * Test boundary condition combinations (e.g., maximum mass + minimum friction) to establish structural stability limits.

  * Map out the operational flight envelope (catch zone and balance stability) across the parameter space.

### 3. Perform sensitivity analysis for sim-to-real transfer

* Parameter Sensitivity Ranking:

  * Calculate partial derivatives or sensitivity indices of key performance metrics (settling time, overshoot, peak voltage) with respect to each uncertain parameter.

  * Rank physical parameters from most critical to least critical to identify primary drivers of instability.

* Disturbance Rejection Evaluation:

  * Test sensitivity to sustained input disturbances, impulse pushes, and sensor measurement offset drift under worst-case parameter configurations.

* Sim-to-Real Calibration Strategy:

  * Document high-sensitivity parameters requiring precise physical measurement or online identification before hardware deployment.

  * Establish baseline requirements for adaptive control or robust gain margins ahead of Phase 4 deployment.