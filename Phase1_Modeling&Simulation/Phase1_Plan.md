
# Project Plan

Plan will be divided by phases and then key items

## Phase 1: Modeling and Simulation

### Derive nonlinear equations of motion from first principles using Lagrangian / Newton-Euler mechanics.
* Practice similar kinematics (Inverted Pendulum on a cart) to learn different dynamics compared to rotary.
* Plan to make the state variables those that the Qube-Servo has as sensors (Motor Encoders) and what can be derived from.
* Understand how the actuator affects the system.
### Linearize about the upright unstable equilibrium to obtain a state-space model (x = Ax + Bu, y = Cx + Du).
* Initial Condition is the pendulum is upright, a unstable equilibrium has linear coefficient around it. 
* sin(theta) = theta; theta is close to zero
* Refresh on Linear Algebra. How a matrix element affects the equation.
* Build the state-space. Understand the importance of ABCD.
### Build a digital twin simulation in MATLAB/Simulink or Python incorporating product parameters
* Study up on the datasheet and github of the Qube-Servo.
* Figure how product specification affect system behavior
* Create a simulation with visiuals and parameters/initial conditions.
* Compare a system with linear and nonlinear equations
### Validate open-loop simulation behavior against published Qube-Servo 3 parameters
* Compare my own model to the Qube-Servo 3 Published model.
* Fine Tune Adjustments
* Test a PID Controller to stabilize the system.

