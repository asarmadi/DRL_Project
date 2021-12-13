# Trajectory Optimization output as reference for Deepmimic on Solo12
## Deepmimic
Deepmimic is an algorithm which uses motion capture data to train a policy using Proximal Policy Optimization (PPO).
Cartwheel            |  Side flip 
:-------------------------:|:-------------------------:
![jumping](/Figs/humanoid_cartwheel.gif)  |  ![bounding](/Figs/humanoid_sideflip.gif) 

[Gifs are taken from https://bair.berkeley.edu/blog/2018/04/10/virtual-stuntman/]

The goal is to train a policy which imitates a given reference motion. Each motion is consists of target poses. In this work, the following pose components are considered

![formula](/Figs/1.png)

The reward function for each timestep is as

![formula](/Figs/2.png)

where each reward term is reported in the following table

![formula](/Figs/3.png)

## Solo12
Solo12 is a 12 Degree of Freedom (DOF) robot unveiled by [Open Dynamics Robot Initiative](https://github.com/open-dynamic-robot-initiative) and all the hardware and software has been open sourced.

## Trajectory Optimization
The full-body reference trajectories for Deepmimic is generated using a trajectory optimization algorithm described in [Ponton et al. paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9350175). In this paper, the full-body trajectory optimization is solved by formulating the problem as a centroidal dynamics optimization problem. This approach does not simplify the dynamics or kinematics. Three reference motions (i.e., jumping, bounding, and walking) are generated for Solo12 as follows:

Jumping            |  Bounding             |  Pace
:-------------------------:|:-------------------------:|:-------------------------:
![jumping](/Figs/solo12_jump_two_jumps_trajectory.gif)  |  ![bounding](/Figs/solo12_bounding_1_trajectory.gif)  |  ![pacing](/Figs/solo12_pace_trajectory.gif)

## Policy
A multi-layer fully connected neural network is considered as the policy. The policy has 2 layers with 1024 and 512 neurons in each layer. The activation function for the hidden layers is relu and for output is tanh. The policy is trained using PPO methodology. Various combination of hyperparameters are considered to achieve the desired goal. Following figure shows the mean reward per episode during the training

![formula](/Figs/mean_reward.png)

The policy is run during the inference as follows

![jumping](/Figs/solo12_motor_rotor_test_1.gif)

It could be seen that the policy is not able to mimic the reference trajectory. 
