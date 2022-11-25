# ROS PID Control

## Summary

In this PID Control Project, we are going to use ROS to design a PID feedback-control program which simulates a husky robot going automatically to the pillar and stopping at the desired distance we want, while trying to minimize angular & forward error and time needed to get to the pillar.

## Initial Conditions

![Screenshot%20from%202022-10-14%2014-59-32.png](https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/Screenshot%20from%202022-10-14%2014-59-32.png)
The husky robot is starting from origin `(0, 0)`, and the pillar is placing at point `(12.5, 12.5)` based on the calculation result of my matriculation number. The predetermined distance to the pillar that the husky robot would stop at is `1.3`. 

## Design of PID Control

The PID Control Algorithm is implemented in `BotControl::pidAlgorithm()` in `BotControl.cpp`. First, the algorithm calculates `Dx` and `Dy` as the `x` and `y` distance of husky robot to the pillar. Then, it saves the current forward and angular error as `error_forward_prev` and `error_angle_prev`. After that, it calculates and updates forward and angular error, regularizes the error angle within `[-p, p]`, obtains corresponding integral and derivative items with previously saved forward and angular error, and finally gets the control signal – `trans_angle_` and `trans_forward_`, and publishes all results. In my design of PID Control Algorithm, **only Proportional Control (P Control) is involved.**

## Result

<p align="center">
    <img height="450" width="800" src="https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/ezgif.com-gif-maker.gif">
</p>

## Tuning

### Objective of tuning

My objective of tuning is mainly to **minimise the forward & angular error when reaching the target, and then trying to reduce the time needed to reach the target on the basis of achieving minimized error**. My strategy of tunning is to tune angular error related parameters first in order to achieve minimized angular error and shortest time to find the correct direction. After that, tunning forward error related parameters in order to achieve minimized forward error and then trying to reduce time needed to reach the target.

### Angular Error

Based on several trials I found that setting `Ki_a` and `Kd_a` to `0` and only assigning value to `Kp_a` could help maintain zero angular error. Then I increased `Kp_a` from `1` to `7` by `1` each time, and I observed that if `Kp_a` is set too low, then it may not reach zero angular error, although it can maintain very well and the error line plotted in the figure is quite stable; if `Kp_a` is set too high, then it can reach zero angular error quickly but it is quite difficult to maintain there quietly. Instead, the error line plotted in the figure oscillates severely around zero error horizontal line. After several trials, I finally found the most appropriate value – `4.5` for `Kp_a`.

Thus, in the final angular error feedback-control program, **only Proportional Control is involved**. That means, the control signal is only proportional to the error. It is the most effective method comparing with adding I control and D control under the current circumstances.

### Forward Error

After tunning angular error related parameters and obtained minimized angular error, I started tunning forward error related parameters. Same as how I did previously, after several trials, I also found that setting `Ki_f` and `Kd_f` to `0` and only assigning values to `Kp_f` could help maintain zero forward error. Then I increased `Kp_f` from `1` to `4` by `1` each time, and I also observed similar phenomenon – if `Kp_f` is set too low, then it took longer time to reach zero forward error, which appears as a long arc on the picture; if `Kp_f` is set too high, then it can reach zero forward error quickly but it is quite difficult to maintain there quietly. Instead, the error line plotted in the figure oscillates severely around zero error horizontal line. After several trials, I finally found the most appropriate value – `2.8` for `Kp_a`.

Therefore, in the final forward error feedback-control program, **only Proportional Control is involved**. That means, the control signal is only proportional to the error. It is also the most effective method comparing with adding I control and D control under the current circumstances.

## Performance

![error_angle.png](https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/error_angle.png)
<div align="center">
    <em>Angular Error versus Time</em>
</div>
<br/>

It can be seen that after tuning, the angular error of the husky robot reaches zero in around five seconds since its first moving, and then keeps at zero after starting, without overshooting. It means the husky robot finds its correct orientation in around five seconds since its starting, and then it keeps pointing to the correct direction without losing the target.

![error_forward.png](https://github.com/Yu-Haikuo/Robotics-ROS-PID-Control/blob/main/Figures/error_forward.png)
<div align="center">
    <em>Forward Error versus Time</em>
</div>
<br/>

It is clear to see that the forward error continues to drop since the beginning, and finally reaches zero at 20th second, without overshooting. That means, the husky robot starts moving directly towards the target since the beginning, and finally arrives and stops at predetermined distance we desire at 20th second.

## Conclusions and Key Learning Points

In conclusion, the husky robot is able to find and maintain at the correct pillar direction in `5` seconds, and stop at predetermined distance in less than `20` seconds with its PID Control Algorithm, and only Proportional Control is involved in this process.

The key learning points include the project building in `Ubuntu` and `ROS`, the initialization of variables, the implementation of control signals (`trans_angle_` and `trans_forward_`), and the tuning process together with the analysis of PID Control Performance.
