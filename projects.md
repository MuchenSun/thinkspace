---
layout: page
title: Projects
permalink: /projects/
---

Most of my work is about **StuPyd programming language** and **ROS**, here are some of my projects.

### StuPyd Programming Language
*StuPyd* is an interpreted high-level programming language for programming education. It's still under development, but we offer beta version for you to try. Here is the [official tutorial page](https://stupyd.github.io/tutorial/).

We offer two formats of implementation:

1. As command-line interpreter, you can follow the instrctions [here](https://github.com/StuPyd/stupyd-lang).
2. As *Jupyter-Notebook* kernel(it's still a demo version), you can follow the instructions [here](https://github.com/StuPyd/demo-kernel).
3. We are trying to build an online platform based on *Jupyter-Notebook*, it will release soon.

### Docker Image for Turtlebot3 Development
A *Docker* container based on ubuntu and novnc aimed to help to Turtlebot3 development. It has pre-installed *ROS(kinetic)* and related packaegs.

For quick use, you can run the following commands:
```
docker pull muchensun/ros_turtlebot3_vnc
docker run -e RESOLUTION=1280x800 -p 6080:80 muchensun/ros_turtlebot3_vnc
```
Then browse `http://127.0.0.1:6080/` in your browser(*Chrome* is recommended).

For more information, you can visit [here](https://github.com/MuchenSun/ros_turtlebot3_vnc).