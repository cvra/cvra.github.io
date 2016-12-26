---
layout: post
title: Project Moon Status update
---

> Captain's log, Stardate 94589.68.
> Project Moon is going as expected.
> Engineers have decided to use the master board as the main computing unit of the robot.
> They hope this simplification will lead to faster development and faster debug.

After the issues[^0] we encountered when we were running a Beaglebone Black as our main computer in the robot, we decided to adopt a simpler approach:
We will keep the network of control boards, but all the high level tasks (AI, navigation, etc.) will be running on a microcontroller (STM32F407/F429).
The PC (Intel NUC) will stay and will be used for computation-heavy tasks, acting like a "smart sensor".
This will make the system easier to program, debug and deploy.

We were especially interested in reducing complexity in order to allow a single person to deploy the robot's software.
To ease that goal, we also switched to a monolithic repository containing all the code and configuration running on our robots.

Since we could not use ROS on microcontrollers (yet ?), we needed some code to replace it.
The obvious starting point was the Aversive framework that we were already using from 2012 to 2014.
This framework was originally designed by a French team (Microb Technology) for the Eurobot contest.
It was originally written for 8-bit AVRs, but we modified it to run on top of ChibiOS on STM32s.

Aversive provides us with a complete navigation stack, from dead reckoning to local navigation and path planning.
This allowed us to quickly have a running setup, which could avoid obstacles using our proximity beacon system.
Here is a video of it in action:

<div class="ytvideo">
<iframe width="640" height="360" src="https://www.youtube.com/embed/ETCFCzpoWx0" frameborder="0" allowfullscreen></iframe>
</div>


## Footnotes
[^0]: [Goldorak post mortem](/blog/2016/goldorak-post-mortem)
