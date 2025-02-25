---
title: "Imitation Learning for Bimanual Manipulation"
date: 2024-09-01
author: ["Chenhao Wu"]
description: ""
longImage: "longpic.png"
showImage: "showpic.png"
---

## Abstract
This is a project I worked on during my internship at the JAKA Intelligent Perception Department from September 20th to December 20th, 2024.
The main purpose of the project was to implement fine-grained operations for the JAKA dual-arm robot based on the Action Chunking with Transformers(ACT) code framework.

## Action Chunking with Transformers(ACT)
- https://mobile-aloha.github.io/
- https://github.com/tonyzhaozh/act

ACT (Action Chunking with Transformers) algorithm is based on the concept of **imitation learning**, and is designed to address high-precision robotic manipulation tasks.
existing imitation learning algorithms perform poorly on fine-grained tasks that require high-frequency control and closed-loop feedback, as **compounding errors** and **non-Markovian behavior** can lead to catastrophic failure.

The main innovation of ACT is using action trunking and temporal ensembling to improve the performance of imitation learning algorithms on fine-grained manipulation tasks.
**Action chunking** groups multiple actions into a single chunk, effectively reducing the task horizon and mitigating error accumulation, and **temporal ensembling** is used to smooth the policy output.
Furthermore, the ACT policy is trained as a conditional variational autoencoder(CVAE), which allows it to generate diverse and temporally consistent actions.

<center>
<img src=pic1.png width=60%>
</center>

## Hardware: JAKA Dual-arm

**JAKA K1** is the hardware platform that we use for bimanual manipulations. The robot contians two **7-DOF single arms**,
we have also installed cameras at the end of each single arm to furher enchance the algorithm.

<center>
<img src=pic2.png height=200>
<img src=pic3.png height=200>
</center>

Python SDK of the JAKA dual-arm robot is used to control the robot.
It allows for the acquisition of the DH matrix and enables fast forward and inverse kinematic analysis.

## Software: JAKA-ACT

### Dataset Acquisition

Imitation learning requires dataset that is collected from human experts.
Here we collected the dataset using the **Pico VR device** or **sensor gloves**.
The dataset contains the timesteps, joint angles, joint velocities, images, etc. And they are stored in the HDF5 format.

<center>
<img src=pic4.png height=200>
<img src=pic5.png height=200>
</center>

### Imitation Learning Model

The entire imitation learning framework still employs the ACT framework introduced by Stanford University.
This framework was originally designed for a six-degree-of-freedom robotic arm, and both the arm's configuration and the end-effector differ from our hardware setup.
Therefore, We modified the model to adapt it to our hardware platform.

### Simulation and Deployment

The imitation learning model is deployment on virtual environments before deploying it on the real robot.
Here we created different virtual environments using **MuJoCo**.

<center>
<img src=pic6.png height=200>
<img src=pic7.png height=200>
</center>

Datasets required for training in virtual environments are relatively easy to obtain. This is because, for tasks such as grasping or insertion, the positions of the target objects are known. Tasks can be completed simply by programming the specified paths based on the target positions.

There are different evaluation metrics for whether a task is completed. 
We need to set different reward functions for each tasks.

{{< video src="video1.mp4" width="30%" autoplay=True >}}

The final training results for grasping tasks are shown below.

{{< video src="video2.mp4" width="50%" autoplay=True >}}

In real environment, The final training results for grasping tasks are shown below.
It can be observed that directly transferring training strategies from virtual environments to real robots still faces significant challenges, resulting in low success rates.
And this is not only related to the parameter settings in the model but also largely depends on the quality of the training dataset.

{{< video src="video3.mp4" width="50%" autoplay=True >}}
