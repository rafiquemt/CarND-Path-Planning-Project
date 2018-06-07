# CarND-Path-Planning-Project
Writeup for the Path Planning Project

This project involved writing a path planner to go around a simulated track meeting certain critera. 

The project meets the criteria set out in the rubric

* The code compiles with `cmake` and `make`.
* The car is able to drive 4.32 miles without violating the conditions set below
* The car drives according to the speed limit
* The car doesn't exceed the total acceleration of 10 m/s^2 and jerk of 10 m/s^2
* The car doesn't collide with any of the other vehicles on the track
* The car stays in it's lane, unless to switch lanes. 
* The car is able to change lanes to avoid slower moving traffic when possible. It uses a greedy algorithm to do so. That is explained in further detail below

# Reflection
The algorithm relied on the `spline` library to generate a smooth path. It selected lanes using a 
greedy algorithm to find the lane with the fastest moving traffic

## Smooth path generation

Smooth Path generation was ensured by using a spline to fill out the path. The newly generated path was appended to the end of the previously unused path points to ensure continuity. 

The spline output ended up meeting the acceleration and jerk requirements. Primarily because it ensured generating smooth and continuous paths.

For getting started (i.e. starting from 0mph), we slowly ramp up the velocity in increments of 0.224 m/s per simulator callback. This removes high acceleration and jerk in the beginning

## Lane changing Algorithm

The lane changing algorithm was a greedy algorithm. It only changed lanes if the car was slowed down below the goal speed of 50mph. If there was a vehicle 30m ahead that wasn't moving fast enough, it would look in the neighboring lanes to see if it was safe to change into it (i.e. no car 30m ahead and 10m behind). 
It would then pick the lane with the fastest moving car / no car within 50ms 

`canSwitchLane` in `main.cpp` lines `176-211` checked if a lane was possible to switch into and
what the speed in that lane would be. Not that it only looked ahead and behind 50m. This was to 

`main()` in `main.cpp` lines 314 to 359 selected the lane with the highest possible velocity. 

The actual lane change is initiated if there is a large enough buffer around the car. It doesn't check to see if any car actually speeds up from behind. The assumption is that it's a big enough gap to change lanes

The greedy algorithm doesn't do much look ahead. It can cause it to miss lane changes that would be more
optimal and result in a faster completion of the loop. 

It still works reasonably well. Was able to complete 4.32 miles in about 5min 30s. 
