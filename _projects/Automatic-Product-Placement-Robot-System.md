---
title: "Automatic Product Placement Robot System"
collection: projects
type: "Project"
permalink: /projects/Automatic-Product-Placement-Robot-System
venue: "Los Angeles, California"
date: 2022-11-01
location: "Los Angeles, California"
---
<iframe width="560" height="315" src="https://www.youtube.com/embed/5BqRX_-W-MU?list=PL-mMKObk0AO5WAtTd5SqoPdd3P7WQd20R" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
# Motivation and Background

![image01](/images/image01.jpg)

Nowadays, Supermarkets are everywhere in our lives. They ensure our daily supplies. The human cost of a supermarket consists mainly of the cashier and the staff to tidy the product shelf.Our team wants to design a robot which can replace those staff to tidy the product. In this way, the supermarket can save a lot of human cost.
# Objectives
## Type 1: Single arm movement:
Simple things like beverages, snacks, etc, which are light weight and small volume. Works could be done by only a single robot arm. Changing the grab method depending on its shape.

## Type 2: Dual-arm Collaboration: 
Large goods like a pack of bottle water, watermelon, a pack of roll paper.Because of the limitation of volume or weight, a single arm could not work very well. In this case we need two or more arms to cooperate together.
What's more, if we want to organize the shelf quickly, two robot arms need to work together at the same time without collision.

## Type 3: Dual- arm event system:
Goods like milk, juice, yogurt, ice-cream, etc, which are placed into the refrigerator or window.One of the robot arm is used to open the door and the other one could place the goods into their place


# Approach

## MATLAB method:
<iframe width="560" height="315" src="https://www.youtube.com/embed/HIrzbnVCv98" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>


![image03](/images/image03.jpg)
In this method, our team concentrated more on the dynamic and control. So the simulation in this method will be closer to reality. 

### Dynamics:
We use the UR5 robot arm to conduct the movement and set the three-fingers gripper as the end effector.
We first establish the urdf file in ROS. But when we go ahead, we found some problems with constructing the physics world. As a result of that,  we changed the platform to Matlab and then did the rest of the project.
![image04](/images/image04.jpg)
We use the UR5 robot arm to conduct the movement and set the three-fingers gripper as the end effector.
We first establish the urdf file in ROS. But when we go ahead, we found some problems with constructing the physics world. As a result of that,  we changed the platform to Matlab and then did the rest of the project.

### Joint Controller
![image05](/images/image05.jpg)
We use the PD controller to directly control the Joint acceleration of the UR5 robot arm. 


$$
\ddot{\theta} = k_p \theta + k_d\dot{\theta}
$$

Firstly, the error between real and desired angle or angular velocity was used to calculate the input angular acceleration. Then, based on the state of the model, we use an inverse dynamic function to get the real input torque of each joint. 
By the way, to make sure the joint goes along the shortest cycle, we add a saturation component to limit the error of real and desired angle in the range of [-pi pi].

$$
\tau = D(q)\ddot{q}+C(q,\dot{q})\dot{q}+g(q)
$$

Using the dynamic function, desired torque can be calculated. Instead of writing our own code, the ur5_inverseDynamic block is a built-in module of Matlab. It helps us save a lot of time. 
### planner

![image08](/images/image08.jpg)
From high to low planner could be divided into Event, Action, Command.  

First of all, an event could have its own template such as pick and place, as follows. We need to set up the position and orientation of where to pick and place so that we can get the high level commands of each action from the m-file “ur5bhLancher”. 

```matlab

% where to pick
Target1=abstacles1+[0 0 0.1];
YPR1=[0 0 0];
offset1=(eul2rotm(YPR1)*eul2rotm([-pi/2 -pi/2 0])*[0;0;0.2]).';

% where to place
Target2=abstacles2+[0 0 0.1];
YPR2=[0 0 0];
offset2=(eul2rotm(YPR2)*eul2rotm([-pi/2 -pi/2 0])*[0;0.0;0.2]).';

EventTable={'lease' 'None'  InitialTarget       InitialYPR   0.001 0;
    'grab'  'RRT'       Target1-offset1    YPR1   3 0;
    'lease' 'None'      Target1-offset1    YPR1   0.2 0;
    'lease'  'IK-Linear'    Target1     YPR1   0.5 0;
    'grab'  'None'          Target1     YPR1   0.5 0;
    'grab'  'RRT' InitialTarget    InitialYPR   1.5 1;
    'grab'  'RRT' Target2    YPR2   3 1;
    'lease'  'None' Target2    YPR2   0.2 1;
    'lease'  'IK-Linear' Target2-offset2    YPR2   0.5 2;
    'grab'  'None' Target2-offset2    YPR2   0.2 2;
    'grab'  'RRT' InitialTarget    InitialYPR   3 2;
     'grab'  'RRT' InitialTarget    InitialYPR   3 2};

```

After that, the Action Commander can send messages to the Trajectory Planner depending on the system clock. Let the planner decide the specific path for the robot arm. Besides, trajectory planner will only be triggered at the start of each action, desire q could be acquired from them and select the state depending on event timestamp;

![image09](/images/image09.jpg)

None means It doesn’t need path planning in this situation;
IK_Linear means the robot arm only need to move according to a straight line;

RRT_Planner means we need to use the RRT planning method in this situation.

## Pybullet method
<iframe width="560" height="315" src="https://www.youtube.com/embed/KcSJRZIzz0Y" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/5BqRX_-W-MU" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>




In this method, our team concentrates more on the Planning Algorithm. We will ignore the gravity and dynamics of the robot arm and the target items, and assume the robot can execute commands accurately.

In this method we have realized the Dual-arm collaboration movement to place the items in a narrow shelf.

### Single arm planing

The basic algorithm used in this task is Bi-RRT planning method. Bi-RRT is an optimized algorithm after RRT. Bi-RRT can reduce the number of searches and invalid search points compared with Single-RRT. The better path can be obtained and used in the actual control by means of multiple searches and replacements.

![RRT](/images/RRT.jpg)

In the pseudocode above, it briefly describes the logic of Bi-RRT.  α(i) means a random sampled configure.

### Multiple-arm collaboration planning

The algorithm used in this task is fast-Bi-RRT. This algorithm can significantly enhance the computation speed among two or more robot arm collaboration tasks. The block diagram of two robot arms’ Bi-RRT is showed below:

![MRRT](/images/MRRT.jpg)

The second step “conduct interpolation” is aimed to make the node number of two paths become equal. After that, it is easier to get the certain path range between two robot arms’ that have collision.


### Why we need to use fast-Bi-RRT 

The main reason that we need to use fast-Bi-RRT is that it can Improve the probability of obtaining a valid solution. 

We can imagine that if we directly conduct the Bi-RRT between two robot arms, the variables consist of 12 joint angles. That means we need to find the right point in a 12-dimensional space. As a result of that, the possibility for us to get the  valid solution will decrease to a huge extent, compared with conducting Bi-RRT in two robot arms separately.
	
The second reason is that the fast-Bi-RRT can enhance the computation speed.

In fast-Bi-RRT, it only recomputes the path in the collision path range among two robot arms, rather than recomputes the path of two robot arms in the full space. In this way, fast-Bi-RRT has reduced a huge amount of computation load. So that it can save a lot of time.

When we use Bi-RRT for two robot arms, it is difficult for us to get a solution. But when we use the  fast-Bi-RRT for two robot arms, it only need around 30 secs to get the solution.


### Iterative Solutions of Inverse Kinematics
![image10](/images/image10.jpg)
![image11](/images/image11.jpg)

Using this Pseudo Inverse Jacobian in the Inverse Kinematic can get a faster and more smoothly solution than normal inverse Jacobian.

# Result

computation time for 4 arms planning is around 1 min
computation time for 2 arms planning is around 30 secs

code：
https://drive.google.com/drive/folders/1MTfzre8VmzQjZE36oGhTfisAO4Xk3qF0?usp=share_link

slides：
![slides](/files/AME 547 presentation-v1.pptx)


# Conclusions
In this project, we have realized the Single arm capture movement, and dual-arm collaboration movement. Those programs can let the machine do the action mostly needed in the supermarket. Because of the lack of time, we haven’t integrated two methods in one platform.

In the future, we need to mount them on an AGV with a slam and planning function. In this way, it can highly reduce human engagement during the robot’s working.
