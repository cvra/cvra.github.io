---
layout: page
title: Landminefree
---

In 2018 we decided to take part in another contest during summer break.
Our attention was caught by [Landminefree](http://www.landminefree.org), a mine mapping competition organized during [IROS](http://www.ieee-ras.org/conferences-workshops/financially-co-sponsored/iros).
The goal of the contest is to detect landmines and to tag their position in a 20m x 20m outdoor field.
The mines can either be buried or laying on the floor.

<div class="ytvideo">
<iframe src="https://www.youtube.com/embed/9-8dVB5uTqQ" frameborder="0" allowfullscreen></iframe>
</div>

We have been aching to build a rover for a long time and this was a great opportunity.
It was also a chance to build a completely different robot using our [CAN stack](https://cvra.ch/robot-software).
The deadline was very short: we started to work on the robot in July, with the contest taking place in October.
We also had fewer ressources, as most of our members were on vacation or internships.

<img src="/images/landminefree/robot.jpg" alt="Sisyphus" width="600">

As we need to navigate in a harsh outdoor environment, we opted for a rocker-bogie mechanism.
This design was heavily inspired by [NASA's Mars rovers](https://opensourcerover.jpl.nasa.gov/), such as Curiosity and Pathfinder.
Each of the six wheels has its own motor and can be steered independently.
This allows the robot to move freely and rotate on itself, while keeping good traction on uneven terrain.

To detect the buried mines, the robot is equipped with three coils used as pulse induction metal detectors.
By charging the coil and monitoring its discharge, we are able to tell wether or not metal is present and wether this metal is ferrous or not.
This system was built using our motor control board to save development time.
Unfortunately this proved to be very limiting, because of the current measurement bandwidth and the peak power output of our H-bridge.
This means that we are not able to detect mines from very far away and are sensitive to our own motors' noise.

For surface mine detection, we use an Intel Realsense D435 sensor.
This is a RGBD camera, meaning that we get a 3D point cloud instead of a 2D image.
By removing the background (planar surfaces, like walls and ground) and clustering the remaining points, we detect surface mines and know their position.
It works very well, even in the presence of highly reflective metal surfaces and outdoor sunlight.

To position our robot on the map, we use our brand new ultra wide band beacon system.
While originally intended for Eurobot, it works very well on Landminefree's larger map.
The system is made of several fixed anchors with known positions.
The robot carries a tag, capable of measuring its distance to each anchor.
Its position can then be estimated using a Kalman filter.
The system works very well; it reliably gives us our position within a 5cm accuracy.

While the robot was not perfect due to lack of testing and late integration, it impressed the jury.
We were awarded the "Best Design" prize.
It was a very enjoyable trip, and we were very glad to have the opportunity to visit IROS and listen to a few presentations.

# Downloads

- [Landminefree 2018 rules](/ressources/rules/landminefree_2018.pdf)
- [Technical report](/ressources/misc/report_landminefree.pdf)
- [Slides of the talk we gave about our robot](/ressources/misc/landminefree_iros_slides.pdf)
