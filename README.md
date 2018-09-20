# FCND-Controls-CPP
Udacity Flying Car Controls Assignment 
Date: 24 Sept 2018


## Intro

This step was to tune the mass so that the quadcopter dowsnt fall to the ground. I tuned to the following number:

Mass = 0.4855



## Body rate control

Requirements: The controller should be a proportional controller on body rates to commanded moments. The controller should take into account the moments of inertia of the drone when calculating the commanded moments.

There are two funcitons to configure in this step, GenerateMotorCommands() and BodyRateControl()

### GenerateMotorCommands()

This funciton calculates the thrust per rotor based on the total given thrust and momentum and saves the values in cmd.desiredThrustsN

First step is to convert the rotor-to-rotor length that is given to a prependicular distance from the rotor to the axes using the formula below: 

length = L / (2 * sqrtf(2))

To calculate the thrust, I first calculated p_bar, q_bar by deviding the momentum per axis by the distance of the rotor to the exis. 

Below is the code for GenerateMotorCommands()

    VehicleCommand QuadControl::GenerateMotorCommands(float collThrustCmd, V3F momentCmd)
    {
        //L = full rotor to rotor distance that is defined in QuadControlParams.txt
        // i need to calculate the perpendicular distance to axes that is:
    
        float length = L / (2.f * sqrtf(2.f)); //perpendicular distance to axes
    
        float f_total = collThrustCmd;  // (F_1 + F_2 + F_3 + F_4)
        float p_bar = momentCmd.x / length; // p_bar = tau_x / length
        float q_bar = momentCmd.y / length;  // q_bar = tau_y / length
        float r_bar = -momentCmd.z / kappa; // r_bar -1 * tau_z / kappa  -- Reversing the direction becouse the z conrdinate upp is negative. 
    
        cmd.desiredThrustsN[0] = (f_total + p_bar + q_bar + r_bar) / 4.f; // front left
        cmd.desiredThrustsN[1] = (f_total - p_bar + q_bar - r_bar) / 4.f; // front right
        cmd.desiredThrustsN[2] = (f_total + p_bar - q_bar - r_bar) / 4.f; // rear left
        cmd.desiredThrustsN[3] = (f_total - p_bar - q_bar + r_bar) / 4.f; // rear right
        return cmd;
    }


### BodyRateControl()

And BodyRateControl() follows below:


    V3F QuadControl::BodyRateControl(V3F pqrCmd, V3F pqr)
    {
        V3F momentCmd;
        V3F I = V3F(Ixx, Iyy, Izz);
        momentCmd = I * kpPQR * ( pqrCmd - pqr );
        return momentCmd;
    }


In QuadControlParams.txt, kpPQR is (41.5, 41.5, 5)

Result:

- PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
- PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds




## Roll pitch control

Requirements: The controller should use the acceleration and thrust commands, in addition to the vehicle attitude to output a body rate command. The controller should account for the non-linear transformation from local accelerations to body rates. Note that the drone's mass should be accounted for when calculating the target angles.



In QuadControlParams.txt, kpBank = 13

Result:
- PASS: ABS(Quad.Roll) was less than 0.025000 for at least 0.750000 seconds
- PASS: ABS(Quad.Omega.X) was less than 2.500000 for at least 0.750000 seconds



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





