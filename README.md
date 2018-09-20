# FCND-Controls-CPP
Udacity Flying Car Controls Assignment 
Date: 24 Sept 2018




## Body rate control

Requirements: The controller should be a proportional controller on body rates to commanded moments. The controller should take into account the moments of inertia of the drone when calculating the commanded moments.

First step is to convert the rotor-to-rotor length that is given to a prependicular distance from the rotor to the axes using the formula below: 

float length = L / (2.f * sqrtf(2.f)); //perpendicular distance to axes


$$
\begin{align}
\tau_x &= (F_1 + F_4 - F_2 - F_3)l \\
\tau_y &= (F_1 + F_2 - F_3 - F_4)l \\
\tau_z &= \tau_1 + \tau_2 + \tau_3 + \tau_4
\end{align}
$$

Result:

- PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
- PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds




## Roll pitch control

Requirements: The controller should use the acceleration and thrust commands, in addition to the vehicle attitude to output a body rate command. The controller should account for the non-linear transformation from local accelerations to body rates. Note that the drone's mass should be accounted for when calculating the target angles.





## Altitude controller

Requirements: The controller should use both the down position and the down velocity to command thrust. Ensure that the output value is indeed thrust (the drone's mass needs to be accounted for) and that the thrust includes the non-linear effects from non-zero roll/pitch angles.

Additionally, the C++ altitude controller should contain an integrator to handle the weight non-idealities presented in scenario 4.




## Lateral position control

Requirements: The controller should use the local NE position and velocity to generate a commanded local acceleration.





## Yaw control

Requirements: The controller can be a linear/proportional heading controller to yaw rate commands (non-linear transformation not required).






## Calculating the motor commands given commanded thrust and moments

Requirements: The thrust and moments should be converted to the appropriate 4 different desired thrust forces for the moments. Ensure that the dimensions of the drone are properly accounted for when calculating thrust from moments.




## Evaluation

Requirements: Ensure that in each scenario the drone looks stable and performs the required task. Specifically check that the student's controller is able to handle the non-linearities of scenario 4 (all three drones in the scenario should be able to perform the required task with the same control gains used).





