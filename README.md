# ros-pca9685-board


> A ROS node to control servos on a PCA9685 board

The primary purpose of this project is to develop a ROS node capable of controlling servos for steering and throttle, such as would be used by an RC car, using the standard PCA9685 board, controlled over the I2C bus from a Raspberry Pi.

You should have completed at least the ROS tutorial for writing a [Teleoperation Node for a Joystick](http://wiki.ros.org/joy/Tutorials/WritingTeleopNode).

## Table of Contents

- [Install](#install)
- [Usage](#usage)
- [API](#api)
- [Maintainers](#maintainers)
- [Contributing](#contributing)
- [License](#license)

## Dependencies

This project depends on the [wiringPi](http://wiringpi.com/) library for I2C communication. It may already be present on your Pi. You can learn more about it [here](http://wiringpi.com/download-and-install/).

## Install

On your Pi, once you've verified wiringPi has been installed, create a Catkin workspace directory and clone this repository into `src`.

```
mkdir -p ~/catkin_ws/src
cd src
git clone git@github.com:liamondrop/ros-pca9685-board.git
```

You should now be able to build with Catkin.

```
cd ~/catkin_ws
catkin_make
source devel/setup.bash
```

## Usage

In the launch file, there is an example servo configuration. This will set the parameters for the steering and throttle servos, as well as the pwm frequency. You may need to change these for your particular setup.

Launch the `pca9685_board` node.

```
roslaunch pca9685_board pca9685.launch
```

The `pca9685_board` node listens to two topics:

 - `/servo_absolute`: this topic accepts a pca9685_board::Servo message, which is built by the project. The main purpose of this topic is to send pwm signals to help you fine tune the configuration of your servos. The following messages are an example of using this topic to find the pulse value corresponding to the steering servo's center, i.e. 333.
    ```
    rostopic pub servo_absolute pca9685_board/Servo "{name: steering, value: 300}"
    rostopic pub servo_absolute pca9685_board/Servo "{name: steering, value: 350}"
    rostopic pub servo_absolute pca9685_board/Servo "{name: steering, value: 330}"
    rostopic pub servo_absolute pca9685_board/Servo "{name: steering, value: 335}"
    rostopic pub servo_absolute pca9685_board/Servo "{name: steering, value: 333}"
 - `/servos_drive`: this topic accepts a standard geometry_msgs::Twist message, using the `linear.x` value for the throttle and the `angular.z` value for the steering. Note that these values should be between -1.0 and 1.0 inclusive.

Assuming you have completed the TeleopTurtle Joystick tutorial above, you could control the servos by simply relaying the `/turtle1/cmd_vel` topic to the `/servos_drive` topic.

```
rosrun topic_tools relay /turtle1/cmd_vel /servos_drive
```

## Maintainers

[@liamondrop](https://github.com/liamondrop)

## Contributing

PRs accepted.

Small note: If editing the README, please conform to the [standard-readme](https://github.com/RichardLitt/standard-readme) specification.

## License

MIT © 2018 Liam Bowers
