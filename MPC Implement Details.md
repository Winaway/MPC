# MPC Implement Details
### The Model
_Student describes their model in detail. This includes the state, actuators and update equations._
The model includes three parts. The first part is the state,  working as a input of the model. 
 The second part of the model is update equations (Solver). The update equations include vehicle model, constraints and cost function. According to  the input state, the Solver will search in the constraints for actuate values that  minimize the cost.
The third part is the output of the model. the model output a couple of value that include steering angle and  throttle.
The vehicle actuate the include steering angle and  throttle and Vehicle move to the next state. The new loop begins.

### Timestep Length and Elapsed Duration (N & dt)
_Student discusses the reasoning behind the chosen N (timestep length) and dt (elapsed duration between timesteps) values. Additionally the student details the previous values tried._
The timestep length multiplies elapsed duration equals to the duration over which future predictions are made.  There is a horizon of the duration, because the environment will change enough that it won't make sense to predict any further into the future. In the case of vehicle, the horizon will be a few seconds. In this project, I choose 1 second as the horizon. We’ll refer to this as T.  T = N * dt, N is the number of timesteps.
dt is the time elapses between actuations. The smaller the dt is , the more  accurate it will be to approximate a continuous reference trajectory. On the other hand , The smaller the dt is, the larger the N is. While N determines the number of variables optimized by the MPC, a large N may cause huge  computational cost. So there is  a tradeoff between dt and N.
I tried dt = 0.05 and N = 20. At the beginning, the vehicle runs well, but as time go on , the error increases rapidly. I think this mainly because of the latency.
And then I tried dt = 0.2 and N = 5. but i get a green line like this. I don’t know why this happens. When I change the N value to 10. The vehicle runs on the track well, but it tend to slow down to a low speed on the corner.
![](MPC%20Implement%20Details/Snip20181006_2.png)
I finally found that with dt = 0.1 and N = 10 the vehicle can run the best.

### Polynomial Fitting and MPC Preprocessing
_If the student preprocesses waypoints, the vehicle state, and_or actuators prior to the MPC procedure it is described./

To make it simple for us to calculate,  I transform the coordinate of the waypoint form map-coordinate to car-coordinate with the car position as the origin zero point and the car heading as x-axis .

### Model Predictive Control with Latency
_The student implements Model Predictive Control that handles a 100 millisecond latency. Student provides details on how they deal with latency._

In order to handle the latency, I changed the input state of the MPC from St to S(t+dt). I take advantage of the vehicle motion model to get S(t+dt).