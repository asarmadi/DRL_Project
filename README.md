# Trajectory Optimization output as reference for Deepmimic on Solo12
## Deepmimic
[Deepmimic](https://dl.acm.org/doi/pdf/10.1145/3197517.3201311) is an algorithm which uses motion capture data to train a policy using Proximal Policy Optimization (PPO).
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
Solo12 is a 12 Degree of Freedom (DOF) robot unveiled by [Open Dynamics Robot Initiative](https://github.com/open-dynamic-robot-initiative) and all the hardware and software has been open sourced. The simulation environment is the one provided in [this repository](https://github.com/open-dynamic-robot-initiative/robot_properties_solo) that uses the PyBullet simulation software.

## Trajectory Optimization
The full-body reference trajectories for Deepmimic is generated using a trajectory optimization algorithm described in [Ponton et al. paper](https://ieeexplore.ieee.org/stamp/stamp.jsp?arnumber=9350175). In this paper, the full-body trajectory optimization is solved by formulating the problem as a centroidal dynamics optimization problem. This approach does not simplify the dynamics or kinematics. Three reference motions (i.e., jumping, bounding, and walking) are generated for Solo12 as follows:

Jumping            |  Bounding             |  Pace
:-------------------------:|:-------------------------:|:-------------------------:
![jumping](/Figs/solo12_jump_two_jumps_trajectory.gif)  |  ![bounding](/Figs/solo12_bounding_1_trajectory.gif)  |  ![pacing](/Figs/solo12_pace_trajectory.gif)

## Policy Training
A multi-layer fully connected neural network is considered as the policy. The policy has 2 layers with 1024 and 512 neurons in each layer. The activation function for the hidden layers is relu and for output is tanh. The policy is trained using PPO methodology. In Deepmimic paper, two design decisions are considered crucial for allowing the model to learn the tasks: Initial State Distribution and Early termination. In this project, I have considered both of them. For early termination, 4 termination conditions are considered: base stability, base impact, imitation length, and knee impact. In the base stability condition, when the base angle in x and y direction is greater than a threshold, then the episode is terminated. In base impact condition when the base contacts the ground and in knee impact condition if the upper legs contact the ground, then the episode is terminated. In imitation length condition, makes sure the episode does not go beyond the reference trajectory length. I found the early termination very important to find better performances. Initially, only base stability condition was used , but after adding the other 3 termination conditions, the performance improved a lot.

One Termination Condition            |  Multiple Termination Condition 
:-------------------------:|:-------------------------:
![fo](/Figs/mean_reward.png) | ![fo](/Figs/mean_reward.png)
![jumping](/Figs/solo12_jump_two_jumps_trajectory.gif)  |  ![bounding](/Figs/solo12_bounding_1_trajectory.gif)


Another important challenge in this project is to choose between joints position or torque control. In the deepmimic the have considered the position control. However, I thought torque control makes the problem more easier to solve.

Torque Control            |  Position Control
:-------------------------:|:-------------------------:
![fo](/Figs/mean_reward_torque.png) | ![fo](/Figs/mean_reward_position.png)
![jumping](/Figs/torque.gif)  |  ![bounding](/Figs/position.gif)


Various combination of hyperparameters are considered to achieve the desired goal. Following figure shows the mean reward per episode during the training for Jumping trajectory. It could be seen that the policy is not able to mimic the reference trajectory. 
