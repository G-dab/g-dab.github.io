---
title: "EBUG: Elastic-beam Buckling and Undulating Generation"
date: 2025-09-01
author: ["Siyue Yao", "Hao Li", "Yuliang Lu", "Chenhao Wu", "Mark R. Cutkosky", "Genliang Chen"]
description: "Multi-modal robot locomotion via elastic-beam undulation and snap-through buckling"
longImage: "longImage.png"
showImage: "showImage.png"
---

## Abstract

Multi-modal locomotion significantly enhances the mobile capabilities of robots in field environments. However, existing realizations rely on complex mechanical systems, are vulnerable to external interferences, and incur high costs. This work proposes **EBUG**, a robot using elastic beams as motion generation units. Two patterns, **crawling** and **jumping**, are achieved through undulating deformations and the snap-through buckling process of a single beam. Under such a simple design, the total cost of EBUG is less than **$5** while showcasing comprehensive motion abilities. EBUG PRO (59g) can crawl at **28 mm/s (0.4 BL/s)** and achieve consecutive jumps at **106.1 mm/s (1.5 BL/s)**, with a maximum jumping height of **61 mm (4.1 BT)**. The combination and switching between crawling and jumping enhances adaptation across various terrains and endows obstacle-crossing ability.

## Background & Motivation

Caterpillars utilize undulation-driven crawling to traverse sand, grass, and concrete; locusts employ powerful jumping to overcome pebbles and leaf litter; crickets robustly integrate two patterns, enabling rapid and adaptive movement across wild scenarios where a single mode would be insufficient. These creatures inspire that multi-modal locomotion enhances and broadens the functions and performance for mobile robot designs.

However, existing implementations depend on complex systems or external stimuli, lacking simple yet effective, high-performance, and cost-efficient designs. One direct method is to integrate several motion generation units required for multiple patterns, which inevitably increases complexity, weight, and cost. Elastic units possess intrinsic deformation and energy storage-release capacities, promoting a high degree of freedom under limited actuators, thus suitable for integrating different patterns. Yet the design, performance, and cost of such mobile robots remain to be improved.

**Contributions of this work:**
- Propose a framework leveraging the deflection and energy storage properties of elastic beams to generate multi-modal motions (crawling and jumping) through undulating deformation and snap-through buckling.
- Establish analytical kinetostatic and dynamic models for the beams to facilitate analysis and optimal design.
- Present straightforward strategies for motion adjustment (turning) by tuning the restriction angle of actuators.
- Achieve 28 mm/s crawling and 106.1 mm/s jumping, with 61 mm maximum jump height, terrain adaptation, and obstacle-crossing abilities.

## Method

### Crawling and Jumping Patterns

The basic design concept of EBUG leverages the deformation of an elastic beam to realize different patterns. Actuated at both ends, the beam deforms into an undulating shape, suitable for mimicking the crawling pattern like a seal. An intermediate region of the beam moves up and down in an approximately linear trajectory under a certain pattern of the actuators. Combining the assumption that no slipping occurs for the contact region with the ground, an enclosed and cyclic motion can be generated. Crawling is achieved only when the contact region detaches from the ground during the upward deformation of the beam; otherwise, the robot swings back and forth without moving forward.

The jumping pattern is achieved by the burst of energy. The **snap-through buckling** process is triggered, changing from a high-energy state to a low-energy one under the same actuation rapidly, often at the 10 ms level. An effective jump is generated when the contact with the ground occurs during the buckling. The released energy alters when the actuation is different, providing control over the jump height and forward distance.

### Kinetostatic and Dynamic Modeling

The kinetostatic model is established based on a discretization method, facilitating analysis and rapid optimization of beam parameters. The elastic beam is discretized into serial segments, and the deformation under given actuation angles is solved through geometric and force equilibrium.

The snap-through buckling process is simulated by the dynamic model of the elastic beam, established based on the serial mechanism equivalence as:

$$\Phi \cdot \ddot{\Theta} + C(\Theta, \dot{\Theta}) = Q$$

where $\Phi$ is the generalized inertia matrix, $C(\Theta, \dot{\Theta})$ denotes the elastic terms (damping neglected), and $Q$ is the generalized force. The energy variance during buckling shows that the potential energy decreases continuously while the kinetic energy first increases from zero and then decreases back to zero. Without considering energy loss, earlier contact leads to better jumping conditions due to higher energy conversion efficiency and a longer contact time.

### System Integration

Based on the theoretical analysis, EBUG is constructed using **four servo motors**, with every two motors actuating one elastic beam at both ends. The motors are connected utilizing a carved 3D-printed PLA base, reducing the weight as much as possible. The beam is made of **carbon fiber (CF)** for both fine elasticity and manufacturing convenience. The rotational joints are set to a minimum height of 6 mm from the ground, with a distance of 50 mm.

Two versions are built:
- **EBUG**: standard servo motors, lower cost
- **EBUG PRO** (59g): higher-performance actuators for enhanced locomotion speed

The selection of beam deflection parameter $\lambda$ is a balance between two patterns: crawling demands a larger $\Delta L$ with a larger $\lambda$, which is further restricted by the torque limit.

## Experiments

### Crawling Locomotion

To achieve undulating deformation of the elastic beams, the motors closer to the rear of EBUG lead those closer to the head by $\pi/2$ phase (a quarter of one period, T/4), for both sides. Moreover, the two sides differ from each other by an additional $\pi$ phase (half of one period, T/2). This leads to the result that when one side is touching down on the ground, the other side is moving backward in the air. Two sides work in turn, resulting in a periodical crawling locomotion with a higher speed.

The rotational speed for the servo motors is tuned to its maximum value (about 1 ms/°). Under such circumstances, the average speed of EBUG PRO reaches about **28 mm/s (0.4 BL/s)**.

### Turning via Crawling

The angle variance for straight locomotion is achieved by the phase difference between the two motors on the same side. Therefore, a different phase lead or lag, within the same rotation range, is capable of tuning the motion directions. When making a right turn, the angles for servo motors #1, #2, and #4 are the same, while that for #3 changes by a $\pi$ phase. This asymmetric actuation generates turning motions at **18°/s**.

### Jumping Locomotion

The jumping process begins when the motor at the restriction end (#2) rotates to a given angle $\theta_r$. The motor at the actuation side (#1) rotates gradually until the elastic beam is close to the snap-through buckling limitation. Subsequently, motor #1 further actuates, surpassing the limitation, which triggers the snap-through buckling phenomenon. The elastic beam touches down on the ground and pushes EBUG PRO into the air. EBUG PRO reaches the highest position and then lands, ready for the next step of jumping by the elastic beam on the other side.

Although more energy will be released if the beams on both sides buckle simultaneously, the slight inconsistency at both sides often prevents a simultaneous trigger and wastes energy. As a consequence, the beams buckle in turn, which utilizes the energy more efficiently in a periodic manner.

Different restricted angles $\theta_r$ (5°, 25°, 45°) result in distinct deformation of the elastic beam at the buckling limitation, further influencing the pushing force. $\theta_r = 45°$ corresponds to a larger energy release in both horizontal and vertical directions than 25°, while 5° corresponds to the smallest. For the first cycle, the locomotion speed can be deduced as **56 mm/s (0.8 BL/s)**, nearly two times faster than crawling.

### Turning via Jumping

Leveraging the relationship between the contact force and $\theta_r$, more complicated motions can be achieved, including left and right turns. The controlling strategy is that the elastic beam corresponding to the outer side of the turning provides a larger contact force in the forward direction than that to the inner side. Specifically, motors on both sides are given $\theta_r = 20°$ when EBUG goes straight. When making a left turn, the motor on the right side provides $\theta_r = 30°$ while that on the left side provides 10°, making the buckling of the beam on the right side more powerful and leading to a left turn after several cycles.

## Results

### Terrain Adaptation

Utilizing the elastic beam to achieve multi-modal locomotion is beneficial for enhancing adaptation to various terrains. The jumping pattern typically shows a higher locomotion speed than the crawling pattern. On smooth ground, the crawling speed is 20.9 mm/s (0.3 BL/s), slower than on fabric ground. On the contrary, the jumping speed is five times faster than crawling, reaching **106.1 mm/s (1.5 BL/s)**, faster than jumping on fabric ground. This is because the smooth ground provides less friction than the fabric ground, affecting crawling more significantly than jumping.

In comprehensive demonstrations, EBUG crosses complex terrains including fabric ground, foam board, and sandy ground. For speed performance, the jumping pattern is utilized on the fabric ground. Subsequently, EBUG switches to the crawling pattern to overcome the foam board with a slope in a more stable manner. Eventually, the crawling pattern is maintained when entering the sandy ground since the jumping pattern loses efficacy. The flexible switching between the two modes enhances environmental adaptation and motion capacity.

### Obstacle Crossing

The jumping pattern endows EBUG PRO with obstacle-crossing ability. For a narrow obstacle (0.07 BL × 0.33 BT), it can be crossed through one step of jumping. For a wide obstacle (0.5 BL × 0.5 BT), two steps of jumping are required. The contact region for the jumping pattern is relatively narrow, and thus the place that EBUG PRO lands greatly influences whether the next jump is effective. Through tests, the maximum height of the obstacles that EBUG PRO can cross over at a high success rate is **0.5 BT**.

The complete obstacle-crossing process integrates both modes: EBUG PRO first approaches the obstacle by crawling until it bumps, then switches to the jumping pattern using several consecutive jumps to cross the obstacle, and afterwards switches back to crawling for more stable forward movement.

### Video Demonstration

{{< video src="video.mp4" width="60%" autoplay=True >}}

## Paper

<center>
<a href="EBUG_IROS.pdf" class="home-social-btn" target="_blank" style="display:inline-flex; align-items:center; gap:6px; padding:10px 24px; border:1px solid #d0ccc2; border-radius:24px; background-color:#fff; color:#555; font-size:0.9rem; text-decoration:none; transition:all 0.2s ease;">
    <svg viewBox="0 0 24 24" width="18" height="18" fill="currentColor"><path d="M14 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V8l-6-6zm-1 7V3.5L18.5 9H13z"/></svg>
    Download Paper (PDF)
</a>
</center>

**Citation:**
```
@inproceedings{yao2025ebug,
  title={EBUG: Elastic-beam Buckling and Undulating Generation for Multi-modal Robot Locomotion in Environmental Exploration},
  author={Yao, Siyue and Li, Hao and Lu, Yuliang and Wu, Chenhao and Cutkosky, Mark R. and Chen, Genliang},
  booktitle={IEEE/RSJ International Conference on Intelligent Robots and Systems (IROS)},
  year={2025}
}
```
