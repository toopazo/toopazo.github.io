---
layout: default
---
# [Home page](https://toopazo.github.io/)

![](pajaroAndrea.jpg)
[Credits: Andrea Silva](https://andreasilvau.myportfolio.com/)

# [](#header-1)Autonomy and controls
> Some interesting definitions given in: Kendoul, Farid. "Survey of advances in guidance, navigation, and control of unmanned rotorcraft systems." Journal of Field Robotics 29.2 (2012): 315-378.

- [Definition 5] Autonomous RUAS: A RUAS is defined to be autonomous relative to a given mission (relational notion) when it accomplishes its assigned mission successfully, within a defined scope, with or without further interaction with human or other external systems. A RUAS is fully autonomous if it accomplishes its assigned mission successfully without any intervention from a human or any other external system while adapting to operational and environmental conditions.

- [Definition 6] Autonomy Level (AL): The term “autonomy level” is used in different contexts in the research community. In Huang (2008) and Huang, Messina, \& Albus (2007) for example, AL is equivalent to human independence (HI). In this paper, AL is defined as a set of progressive indices, typically numbers and/or names, identifying a RUAS capability for performing autonomously assigned missions. A RUAS’s AL can be characterized by the missions that the RUAS is capable of performing (mission complexity or MC), the environments within which the missions are performed (environment complexity or EC), and independence from any external system including any human element (external system independence or ESI). Note that this AL definition is similar to the contextual autonomous capability (CAC) definition in the NIST ALFUS framework (Huang, 2008), except for HI, which is replaced here by ESI.
    
- [Definition 7] Automatic Flight Control System (AFCS): Automatic control 10 can be defined as the process of manipulating the inputs to a dynamical system to obtain a desired effect on its outputs without a human in the control loop. For RUAS, the design of flight controllers consists of synthesizing algorithms or control laws that compute inputs for vehicle actuators (rotors,
    aileron, elevator, etc.) to produce torques and forces that act on the vehicle in controlling its 3D motion (position, orientation, and their time derivatives). AFCS, called also autopilot, is thus the integrated software and hardware that serve the control function as defined. 
    
- [Definition 8] Navigation System (NS): In the broad sense, navigation is the process of monitoring and controlling the movement of a craft or vehicle from one place to another. For RUAS, navigation can be defined as the process of data acquisition, data analysis, and extraction and inference of information about the vehicle’s states and its surrounding environment with the objective of accomplishing assigned missions successfully and safely. This information can be metric, such as distances, topological, such as landmarks, or any other attributes that are useful for mission achievement. The main autonomy-enabling functions of a navigation system, from lower to higher level, are as follows: Sensing, State Estimation, Perception, Situational Awareness.
    
- [Definition 9] Guidance System (GS): A guidance system can be defined as the “driver” of a RUAS that exercises planning and decision-making functions to achieve assigned missions or goals. The role of a guidance system for RUAS is to replace the cognitive processes of a human pilot and operator. It takes inputs from the navigation system and uses targeting information (mission goals) to make appropriate decisions at its high level and to generate reference trajectories and commands for the AFCS at its low level. GS decisions can also spark requests to the navigation system for new information. A guidance system comprises various autonomy-enabling functions including trajectory generation, path planning, mission planning, and reasoning and high level decision making.

https://www.grc.nasa.gov/www/k-12/airplane/tunmodel.html

* * *

