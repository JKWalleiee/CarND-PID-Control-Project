# CarND-Controls-PID
Self-Driving Car Engineer Nanodegree Program

---

## Dependencies

* cmake >= 3.5
 * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1(mac, linux), 3.81(Windows)
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `./install-mac.sh` or `./install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```
    Some function signatures have changed in v0.14.x. See [this PR](https://github.com/udacity/CarND-MPC-Project/pull/3) for more details.
* Simulator. You can download these from the [project intro page](https://github.com/udacity/self-driving-car-sim/releases) in the classroom.

Fellow students have put together a guide to Windows set-up for the project [here](https://s3-us-west-1.amazonaws.com/udacity-selfdrivingcar/files/Kidnapped_Vehicle_Windows_Setup.pdf) if the environment you have set up for the Sensor Fusion projects does not work for this project. There's also an experimental patch for windows in this [PR](https://github.com/udacity/CarND-PID-Control-Project/pull/3).

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./pid`. 

Tips for setting up your environment can be found [here](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/0949fca6-b379-42af-a919-ee50aa304e6a/lessons/f758c44c-5e40-4e01-93b5-1a82aa4e044f/concepts/23d376c7-0195-4276-bdf0-e02f1f3c665d)

## Editor Settings

We've purposefully kept editor configuration files out of this repo in order to
keep it as simple and environment agnostic as possible. However, we recommend
using the following settings:

* indent using spaces
* set tab width to 2 spaces (keeps the matrices in source code aligned)

## Code Style

Please (do your best to) stick to [Google's C++ style guide](https://google.github.io/styleguide/cppguide.html).

## Project Instructions and Rubric

Note: regardless of the changes you make, your project must be buildable using
cmake and make!

More information is only accessible by people who are already enrolled in Term 2
of CarND. If you are enrolled, see [the project page](https://classroom.udacity.com/nanodegrees/nd013/parts/40f38239-66b6-46ec-ae68-03afd8a601c8/modules/f1820894-8322-4bb3-81aa-b26b3c6dcbaf/lessons/e8235395-22dd-4b87-88e0-d108c5e5bbf4/concepts/6a4d8d42-6a04-4aa6-b284-1697c0fd6562)
for instructions and the project rubric.

## Rubric
### Reflection
___
#### Describe the effect each of the P, I, D components had in your implementation.


- The proportional portion of the controller tries to steer the car toward the center line (against the cross-track error). if the proportional portion is too big, the car overshoots the central line very easily and go out of the road very quickly. This effect is greater if the PID controller uses only the P portion.

- The effect of the integral part is to eliminate possible biases effects in the controlled system. In a system with Bias, a PD controller can not archive the minimum desired error. In the system of this project, no bias is present, then, if we use the Integral portion in the PID controller, the vehicle will oscillate around the reference and with a PI controller the vehicle would be very unstable.

- The differential portion helps to counteract the proportional trend to overshoot the center line by smoothing the approach to the reference, since the effect of the differential portion acts before the proportional portion, anticipating the actions necessary to reach the reference.


#### Describe how the final hyperparameters were chosen.
The parameters were chosen manually by trial and error. Following the next steps:

- First, I made sure the vehicle could drive straight with zero as parameters. 

- Then, I add a proportional part (P controller) of 1.0, and the car started to follow the path but started to overshooting go out of it. In this step, I reduced the factor, subtracting 0.1 for each iteration, until reaching a smaller overshooting, using a P factor of 0.2. However, a differential control action was necessary.

- I do not implement an integral control action, since the system has no have bias.

- Then, I add a differential portion to try to overcome the overshooting, using a initial value of 2.0. From here, I increased the value of the differential factor until achieving an acceptable behavior in the follow-up of the reference, that is, the central line.

Originally I wanted to tune the controller to reduce the overshoot and establishment time of the system, however, I observed that tuning the controller by trial and error to reduce the error in the reference, that is, the vehicle oscillations, the result was satisfactory. So I started modifying the P and D portions of my PD controller. The final parameters of my PID controller were: {0.15, 0, 4.0}

### Video (Output).
A video of my implementation can be found in: !["video"](./videos/final_video.mp4).

