---
layout: default
permalink: /projects/
title: Projects
search_exclude: true
---

# <u> Robotics Projects </u>

## **Eve: A self-driving car**

<img src="https://media.githubusercontent.com/media/abhichou4/blog/master/images/eve.gif" />

Eve is an electic vehicle with self-driving capabilities being developed by Project MANAS for the indian roads. The EV finished top 5 in the Mahidra Rise Prize competition, winning 20 lakhs for future developement.

I worked on object-detection and tracking, and lane-detection modules of the car in my tenure as Perception Head. It has been one of the most exciting projects I've worked on.

[Link to project website.](https://projectmanas.in/)
[Object detection repository.](https://gitlab.com/abhineet99/object-detection/-/tree/master/)

## **Solo: An autonomous ground vehicle**

<img src="https://media.githubusercontent.com/media/abhichou4/blog/master/images/solo-championship.jpg" width="60%" style="margin: 15px;" />

Solo is an autonous ground vehicle built by Project MANAS, capable of navigating courses with static obstacles. It bagged the grand prize at IGVC 2019, held at Oakland University, Michigan.

I worked on helping it avoid potholes and mapping model detection to ground frame using images and their corresponding point cloud. I learned a lot working on this project.

[Pothole detection repository.](https://gitlab.com/abhineet99/pothole_detection/-/tree/devel)

## **Fuse My Senses: Odometry estimation**

Odometry data is an important source of information to let an autonomous robot find it’s
location in its environment. A multitude of sensors are often used to enable a robot
to collect odometry information, but each sensor tends to have a unique error characteristic. 

As a part of this project, I build a simulated robot in gazebo with an array of sensors and a linear
kalman filter to combine there sources to give near accurate odometry, visualized in rviz.

**Note: Code for this was sadly lost.**

# <u> Open-source Projects </u>

## **Tensorflow**

TensorFlow is a free and open-source software library for machine learning and artificial intelligence. It can be used across a range of tasks but has a particular focus on training and inference of deep neural networks.

As part of Google Summer of Code 2020, I helped build batching into forward-mode automatic differentiation.

[Link to the project.](https://gist.github.com/abhichou4/449286bf1cec8dea9f2ac5735c6f3826)

## **NoobAutograd**

I toy-implementation for both forward-mode and backward mode automatic differentiation. It can successfully compute numerical gradients for scalar operations.

[Link to the project.](https://github.com/abhichou4/NoobAutograd)

## **Swift for Tensorflow Benchmarks**

Python is not the future of machine learning. It's slow, GIL forces using external C for concurrency, and accelerators force things into CUDA code. Building machine learning frameworks in python basically require writing a lot of code in another launguge (like C++) and giving it a python API.

One effort to create something more satisfying was Swift for Tensorflow. Swift is optimized for mordern hardware, has language integrated autodiff, and extendible on all levels (even the primative data types). This repo tries to benchmark S4TF models to that of Tesorflow, Keras & Pytorch.

[Link to the project.](https://github.com/abhichou4/s4tf-benchmark) 


