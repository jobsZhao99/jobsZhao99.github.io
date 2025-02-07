---
layout: archive
title: "CV"
permalink: /cv/
author_profile: true
redirect_from:
  - /resume
---

{% include base_path %}

Education
======
* M.S. in Information Technology Project Management and M.S. in Engineering/Industrial Management, Westcliff University, 2024-2026 Mar. (expected)
* M.S. in Mechanical Engineer, University of Southern California, 2022-2023
* B.S. in Flight Vehicle Power Engineering,Civil Aviation University of China, 2017-2021

Work experience
======
* **[Laser Robotics](https://laser-robotics.com "Laser-robotics")**DEC 2023 - Now
  * **System Integration—DC-DC—Prototype Design**
  * **System Integration Engineer** Led firmware and hardware integration for the HECTOR project, focusing on the Low-Level Control Board. Managed multiple communication protocols (SPI, I2C, UART, RS485, CAN, LWIP/UDP) within FreeRTOS to ensure seamless system functionality.
  * **Power System Designer** Designed custom battery systems and Power Distribution Boards using KiCAD and Altium Designer. Developed solutions by using DCDC(MPS4575), LDO(AMS 3.3) and incorporating emergency switches, relays, and power monitoring(INA237).
  * **Prototype Design Engineer** Directed rapid prototyping using 3D printing, significantly accelerating iterations of
the robot’s body. Optimized PCB board and wiring layouts, enhancing overall system integration and functionality.

* **Spring Airlines** Jul 2021 - Nov 2021
  * Aeronautical Engineer trainee 
  * Worked with inspection engineers on aircraft maintenance tasks like lubricating landing gear and replacing filters,
following safety regulations. My training covered areas such as wiring and sealants, and I helped maintain the Airbus
A320 series using important maintenance documents.

* **Yuantuo Industrial Design Consulting Co., Ltd** Aug 2020 - Sep 2020
  * Engineer Assistant—3D Printing— Prototype Design
  * Developed prototypes through 3D printing and sheet metal design. Contributed to projects such as solar collectors
and portable heater for EV.
  


Projects
======
  <ul>{% for post in site.projects reversed %}
    {% include archive-single-project-cv.html  %}
  {% endfor %}</ul>

Skills
======
* Programming
  *  Python, C/C++, C#,  Google Script,FreeRTOS 
* Mechanical Drawing
  * Solidworks, CroE, 3DMax, AutoCAD
* Electrical Drawing
  * Altium Designer, KiCAD
* Software
  * MATLAB, Simulink, CubeIDE, Simcape, PyBullet,Unity3D

Publications
======
  <ul>{% for post in site.publications reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  

  
<!-- Teaching
======
  <ul>{% for post in site.teaching reversed %}
    {% include archive-single-cv.html %}
  {% endfor %}</ul>
  
Service and leadership
======
* Currently signed in to 43 different slack teams -->
