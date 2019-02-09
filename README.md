# A tiny project of Markov Localization in c++ 

---

## Definition of Variables:

$z_{1:t}$ represents the observation vector from 0 to t (range measurements, bearing, images, etc).

$u_{1:t}$ represents the control vector from 0 to t (yaw/pitch/roll rates and velocities).

$m$ represents the map (grid maps, feature maps, landmarks).

$x_t$ represents the pose (position (x,y) + orientation $\theta$ ).

## Task

Given those variables, the task for localization of vehicle is to estimate the **posterior** distribution for the state x at time t, which is:

$belief(x_t)=p(x_t|z_{1:t}, u_{1:t}, m)$.

As we can see, in order to predict the state x at t, we should have archived store of huge amout dataset from the beginning until t.  

## Markov localization technique

First, the above posterior distribution can be rewritten based on Bayes rule as:

$p(x_t|z_{1:t}, u_{1:t}, m)=\frac{p(z_t|x_t,z_{1:t-1},u_{1:t},m)p(x_t|z_{1:t-1},u_{1:t},m)}{p(z_t|z_{1:t-1},u_{1:t},m)}$.

$p(z_t|x_t,z_{1:t-1},u_{1:t},m)$ is the observation model and $p(x_t|z_{1:t-1},u_{1:t},m)$ is the motion model.

For motion model, using total probability rule, $p(x_t|z_{1:t-1},u_{1:t},m)$ can be further represented by:

$p(x_t|z_{1:t-1},u_{1:t},m)=\int p(x_t|x_{t-1},z_{1:t-1},u_{1:t},m)p(x_{t-1}|z_{1:t-1},u_{1:t},m)dx_{t-1}$.

With the Markov assumption:

*A Markov process is one in which the conditional probability distribution of future states (ie the next state) is dependent only upon the current state and not on other preceding states.This can be expressed mathematically as:*

$p(x_t|x_{t-1},...,x_0)=p(x_t|x_{t-1})$

Since we already have the t-1 state $x_{t-1}$, observations $z_{1:t-1}$ and control $u_{1:t-1}$ can be neglected in $p(x_t|x_{t-1},z_{1:t-1},u_{1:t},m)$. Owing to the control is in the next time sample, $u_t$ can be removed in $p(x_{t-1}|z_{1:t-1},u_{1:t},m)$. To this end, we have 

$p(x_t|z_{1:t-1},u_{1:t},m)=\int p(x_t|x_{t-1},z_{1:t-1},u_{1:t},m)p(x_{t-1}|z_{1:t-1},u_{1:t},m)dx_{t-1}=\int p(x_t|x_{t-1},u_t,m)p(x_{t-1}|z_{1:t-1},u_{1:t-1},m)dx_{t-1}$,
where $p(x_t|x_{t-1},u_t,m)$ is the system transition model.

Note the term $p(x_{t-1}|z_{1:t-1},u_{1:t-1},m)$ is the belief at time sample t-1, so the whole predicting system can be easily processed in a recursive manner. 

That is the core of Markov localization.


For visualization of markdown with latex, I use VS Code with the [extension](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one).




