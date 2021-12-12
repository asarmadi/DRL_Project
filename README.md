# Trajectory Optimization output as reference for Deepmimic on Solo12
## Deepmimic
Deepmimic is an algorithm which uses motion capture data to train a policy using Proximal Policy Optimization (PPO).
![Cartwheel](/Figs/humanoid_cartwheel.gif)

[Gifs are taken from https://bair.berkeley.edu/blog/2018/04/10/virtual-stuntman/]
The goal is to train a policy which imitates a given reference motion. Each motion is consists of target poses. In this work, the poses are $x_{base}$ the position of the robot base, $q_{base}$ the base quaternion, $\omega_{base}$, the base angular velocity, and $q_{joint}$ the joint positions. Corresponding reference values are shown with $\hat$.
![formula](https://render.githubusercontent.com/render/math?math=e^{i%20\pi}=-1)
## Trajectory Optimization

## Solo12
Solo12 is a 12 Degree of Freedom (DOF) robot unveiled by [Open Dynamics Robot Initiative](https://github.com/open-dynamic-robot-initiative) and all the hardware and software has been open sourced.
