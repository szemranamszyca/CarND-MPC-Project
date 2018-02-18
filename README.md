# Model Predictive Control
Self-Driving Car Engineer Nanodegree Program
This project contains implementation of Model Predictive Control
---

# Project information:

## Goal
The main goal of this project was to navigate a track in prodvided simulator without leaving it at any moment.

### Model

Model should combines state and actutation from previous timestamp to calculate state for the current timestamp, based on equations:

```
x[t] = x[t-1] + v[t-1] * cos(psi[t-1]) * dt
y[t] = y[t-1] + v[t-1] * sin(psi[t-1]) * dt
psi[t] = psi[t-1] + v[t-1] / Lf * delta[t-1] * dt
v[t] = v[t-1] + a[t-1] * dt
cte[t] = f(x[t-1]) - y[t-1] + v[t-1] * sin(epsi[t-1]) * dt
epsi[t] = psi[t] - psides[t-1] + v[t-1] * delta[t-1] / Lf * dt
```

where:
```
    x, y - car's position.
    psi - ccar's heading direction.
    v - cars velocity.
    cte - cross-track error.
    epsi - orientation error.
```

Outputs are:
```
    a - car's acceleration (throttle).
    delta  - teering angle.
```

### Timestep Length and Elapsed Duration (N & dt)
+ N = 10
+ dt = 0.1

Optimizer  for these values considered one second duration in which its try to determine a corrective trajectory.


### Polynomial Fitting and MPC Preprocessing:
Waypoints from the simulator are transformed to the car coordinate system. Then a 3rd degree polynomial is fitted to the transformed waypoints. TUsing these polynomial coefficients cte and epsi are calculated. Also, these values are used by the solver to create a reference trajectory.

### Model Predictive Control with Latency

+  original kinematic equations depend on the actuations from the previous timestep, but with a delay of 100ms (timestep interval). The actuations are applied another timestep later, so the equations have been altered to account for this.
+ additional penalty (combination of velocity and delta) was added and result is much more reliable
