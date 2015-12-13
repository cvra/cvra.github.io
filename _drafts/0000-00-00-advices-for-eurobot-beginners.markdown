---
layout: post
title: Advices for Eurobot beginners
author: Antoine
---
#General
* Honestly assess your skills, time and budget
* KISS: Keep it Simple and Stupid
* Base meta: reliable robots win
* Two robots for first year are big no no
* Beacons ?
* You should know where to cut corners. If you don't, read the rest.
* Download all the relevant documentation/datasheet/code before going to the contest as you may not have fast internet on the contest location (or any internet at all).
* Document everything, but don't over document.
    You'll need to keep track of your all your doc so try having fewer documents and don't be too verbose.
    Prefer lists and drawings over long paragraphs and descriptions.
* Communicate a lot, but never prolongate meetings more than they need.
    Once you're done updating the team or solving your problem, go back to work.

#Mechanics

I strongly advise you against cutting corners when building / buying the wheelbase of your robot.
A quality base will really help you reach a good level of precision when moving around on the table and will be easier to work with.
You can find our current design [on our GrabCAD](https://grabcad.com/library/differential-wheels-platform-for-mobile-robot-1).
If you have any questions about the design just ask.

Encoders on the motor's shaft are easy to use but if your robot slips  you will loose precision. This can be avoided using encoders on a second set of wheel (you can see it on one of our robots below.

![debra](/images/2009/debby4.jpg)

You should stick to quality hardware for motors and sensor if you can afford it. Buy from known brands: Sick, Baluff, Baumer for the sensors and Maxon or Faulhaber for the motors. If you are willing to spend some time searching, eBay can be a good source to buy them cheaply (secondhand should not be a problem with this type of hardware).

#Electronics
* Have spare boards available if you can afford it.
    Swapping a whole board is much faster to do in a rush, for example if you destroy your main board before a match.
    Having complete spares also allows your programmers to bring some boards home for testing without the complete robot.
    A good rule of thumb would be to have one or two spare boards for expensive items that you only have once in the robot.
    For the rest aim for around 50% of spares: If you need 10 boards, order 15.
* Pick your microcontrollers after discussing the pro and cons with the software team.
    You should not only look at raw performance, but also take community support and existing tooling.
    At the time of writing two architectures are really ahead of others: ARM's Cortex-Ms that we use in our robots and Arduino.
    The Arduino has a huge community and the tools are really simple to use (but very limited).
    On the other hand ARMs are harder to learn and use, but have very powerful tools such as a full featured debugger, hardware networking support, very high performance etc.
* Before ordering a batch of PCBs, be sure to make a prototype version and test it thoroughly.
    We made this mistake with our motor controller board and had [some issues](https://github.com/cvra/motor-control-board/issues) that we could not fix due to the high cost (and turnaround time) of redoing a batch.
    You can also lower the risk of failure by having your design reviewed by someone else before ordering.
* When wiring the robot take your time.
    Cutting corners by soldering wires directly instead of mounting connectors, making bad solders or skipping on isolation must be avoided.
    It will reduce your robot reliability and make your debugging sessions a nightmare.
    In the same idea, pick connectors which cannot be reversed, to avoid painful consequences.

#Software

* My main advice is to keep it simple here and only do complicated stuff later.
* Store your code in a version control system.
    This is the *single most important* advice for your programming team.
    Doing this allows you to freely experiment with new ideas without having to fear breaking existing code.
    It also permits several team members to work in parallel and merge their work later on.
    Finally it also doubles as a backup of your code, in case you make a mistake or experience a drive crash before the contest.
    The choice of the software is not critical but sticking with Git or Mercurial is a good idea as they have a huge community and allow you to work without internet connection.
* Write good code.
    While this is a highly subjective matter about which a lot have been written, a global concensus seems to be "optimize for reliability."
    While it might be counter-intuitive, most of the programming time is spent reading code.
    You may be wandering through function documentation to figure out a parameter, then reading some code to figure what it does, etc.
    During debugging you will also be reading a lot of code and trying to map its execution in your head, maybe with the help of someone else who did not write the code.
    That means you should pick up meaningful names and divide functions so that the code feels more like a description of what it does rather than a list of instructions.

**Insert funny image of programmers splitting their hair in half to break the wall of text .**

* Write independent modules that you can completely test in isolation.
    It is really good to know that some module is behaving properly and that you are not breaking its functionality.
    The high-level modules should be platform-independent so that you can test them on your computer, which have way better tools such as GDB, Valgrind, etc.
    It will also help you later if you decide to upgrade to another microcontroller.
    Later you might take the step to [unit tests](https://en.wikipedia.org/wiki/Unit_testing), but that will probably be the topic of another post.
* Integrate early, integrate often.
    You should aim to have a working system very early that you can iterate upon and improve.
    This keeps the team motivated and discovers design problems very early.
    You might also run in "integration" hell at the end of the project if you don't do it, where you have to fix all the integration bugs at once when you are the most pressured.
* Review *all* the code.
    Code reviews keep a high code quality because other people might notice bugs or weird edge conditions that you may not have thought of.
    It might also reveal if you are writing "bad" (unreadable) code, and the team will ask you to fix it.
    All the code should be reviewed, even if someone feels that they cannot make mistakes, but sometimes it can be hard to convince everybody to comply.
    The team will also have a better knowledge of the complete systems, which is both useful for debug, and fun.
* Working in pair is funnier and faster than working alone.
    This seems counterintuitive at first because you are effectively using two team members to perform a single task but it is a documented industry practice (just search for Pair Programming on your favorite search engine).


#Conclusion

* Don't hesitate to send us questions via email
