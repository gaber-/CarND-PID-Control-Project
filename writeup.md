**PID Controller** 

**PID Controller Project**

The goals / steps of this project are the following:
* Implement a PID class
* Keep the car in the simulator on track using the cte and the PID with an appropriate set of parameters
* Control the car speed via PID
* Summarize the results with a written report



---
### Paremeters effect

The P I and D components are simply added together, which makes it easy to analyze them separately.

#### 1. P

The P (proportional) component of the PID is simply multiplied by the error passed to the update function, this means that it tries to correct (in our case steer) harder the bigger the error is.

Since we control the steering angle, and not the direction of the car the p component causes the car to overshoot the central line and wobble back and forth. A too large value of p can make the correction of direction too steep, and cause the car to go out of control.

#### 1. D

The D (derivative) component of the PID is multiplied to the difference between current and previous error. This means that the D tends to keep the error constant, attenuating the tendency to oversteer of the p component.

If the track was a circle an optimal value of P and D would cause the car to follow a circular path leaning outwards, because it would always understeer slightly.

#### 1. I

The I (integrative) part of the PID is proportional to the sum of the errors. It tries to correct the bias left by the P and D components. A too large I tends to cause the car to overcorrect, like the P component, also, unlike the other components, it has a "memory" (the previous errors) and may not correct itself readily.

Thus if the track was circular, and given that the P and D components allow the car to draw a circle, the I would center the trajectory. Since the track is not circular, it tends to correct the bias on large curves.


### Tuning

#### 1. Steering PID parameters

At first I looked for a P parameter where the steering angle/error proportion made sense, since a too large value made the car take steep turns and lose control with a large P, and not steer significantly with a too small P.

Then I tuned the P and D components toghether, looking for a combination that gave a stable trajectory.

Lastly I used the I component to reduce the almost constant error that appeared on the first curve of the track (I didn't do enough tests to see if there's an improvement over the whole track)

#### 2. Speed PID parameters

For the speed control (where I set the throttle to reach a given speed) I noticed that the P component alone was enough to get a stable value close to the seeked one, so I just used the I component to bring it up to the desired value.

A larger value of P alongside with D would probably yeld a faster correction of the speed (eg in case of change in slope) but since P and I alone performed well enough I left them there.


### Challenges

#### 1. Parameters

The track is not uniform, and therefore there isn't an optimal set of parameters that fit the whole track perfectly.

A smaller value of P and D gives a nicer trajectoy, however it makes the car understeer and fail to follow the track in presence of steep curves.

Scaling up P and D it becomes easier to tackle steep curves, but the car tends to wobble more, and sometime lose control.

#### 2. Speed

Increasing speed (or rather, decreasing the number of reads) poses two challenges:

* Since the car runs farther between reads the error tends to be larger and the car tends to oversteed and lose control more easily, which could be tackled with smaller P and D values but

* Fewer reads means the car notices steep curves later, and would therefore need an even steeper correction angle, and thus larger P and D values. 
