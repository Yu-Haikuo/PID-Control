# ROS PID Control

## Summary of Project
In this PID Control Project, we are going to use ROS to design a PID feedback-control program which simulates a husky robot going automatically to the pillar and stopping at the desired distance we want, while trying to minimize angular & forward error and time needed to get to the pillar.

## Initial Conditions
![Screenshot%20from%202022-10-14%2014-59-32.png](https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/Screenshot%20from%202022-10-14%2014-59-32.png)
The husky robot is starting from origin `(0, 0)`, and the pillar is placing at point `(12.5, 12.5)` based on the calculation result of my matriculation number. The predetermined distance to the pillar that the husky robot would stop at is `1.3`. 

## Design of PID Control
The PID Control Algorithm is implemented in `BotControl::pidAlgorithm()` in BotControl.cpp. First, the algorithm calculates `Dx` and `Dy` as the `x` and `y` distance of husky robot to the pillar. Then, it saves the current forward and angular error as `error_forward_prev` and `error_angle_prev`. After that, it calculates and updates forward and angular error, regularizes the error angle within `[-p, p]`, obtains corresponding integral and derivative items with previously saved forward and angular error, and finally gets the control signal – `trans_angle_` and `trans_forward_`, and publishes all results. In my design of PID Control Algorithm, **only Proportional Control (P Control) is involved.**

## Result
<p align="center">
    <img height="450" width="800" src="https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/ezgif.com-gif-maker.gif">
</p>
