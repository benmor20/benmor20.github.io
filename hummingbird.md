# Robotic Path Planning with Hummingbird

## The Problem

In the summer of 2022, I worked in the Olin College RoboLab on a project called
Hummingbird. The goal of this project was to create a swarm of drones that could
each complete different tasks in a cramped, enclosed environment. For that
summer, the team's goal was to provide proof of concept within a ten week
period.

My specific task was to create a path planning algorithm and to test it in a
simulation. I spent my first two weeks researching the current
implementations of drone swarm algorithms, where I found and implemented
algorithms such as gradient descent, particle swarm optimization, and artifical
potential fields. However, each one I implemented had one or more of the
following issues:

- **High computation time**, especially to avoid other drones - This would cause a
  drone to either stay put while it or another computer worked out its path.
- **Built for a static environment** - The presence of other drones moving along
  vastly different paths would often cause collisions.
- **Imprecision** - Many algorithms were designed for flying around large, open areas
  in which a difference of a meter is not significant. For the cramped, enclosed
  environment I was working in, precision needs to be at the centimeter level.

## The Solution

Because of these issues, I created my own algorithm that I have not seen
elsewhere in my literature review, which I named Dynamic Gradient Descent (DGD),
and is largely based on the gradient descent algorithm. In the classic
algorithm, the environment is pre-mapped and the target is pre-planned. Using that
information, the drone will create a topographical map of its surroundings,
where the target is a "sink" (like a valley), and each obstacle is a "source"
(like a mountain). It would then place a metaphorical ball at its current
position on that map, and "roll" it down the hill until it reached the valley -
the target. The drone will then follow that exact path to reach the target
itself.

This works well in a static environment with plenty of time to compute a path.
However, if the obstacles (the other drones) are moving, the planned path is no longer valid.
Additionally, with many obstacles, calculation time vastly increases. To solve
these issues, I desgined DGD to support real-time calculation. With DGD, every loop
(a fraction-of-a-second long, ideally), the drone takes a single "step"
forward. To calculate that step, it takes the current positions of all the
other drones, along with any other obstacles, and creates the same topographical
map, then lpaces a ball at its position. It then let the ball roll down the
hill, except only for a small amount of time. It follows the direction
the ball moved until it recalculates the target velocity in the next loop,
repeating until the goal is met.

DGD fixes both of the issues gradient descent has with one modification. First, it
no longer needs long computation times before the drone flies, as that
computation is spread out over the entire flight. Second, the drone can poll the location of the other drones every loop, instead of guessing
where they will be some amount of time in the future. This allows for easy,
real-time course corrections without precalculating the path of every drone in
the swarm.

## Results

DGD was a very successful algorithm. Here is a video of it running on a single drone:

<video src="assets/dgd.mp4"></video>

Here, lighter colors are "downhill" while darker colors are "uphill". The black dot is the drone using DGD, with a line coming from it in its direction of motion. Each of the dark blue dots is a source; a place the drone wants to avoid. In this case, they represent other drones following straight-line paths. Finally, the purple star represents the target. As you can see in the video, the drone using DGD dynamically navigates around other moving obstacles while still heading towards its target.

I implemented this system in large simulations, and while there were still some crashes (likely due to parameter tuning that I did not have time to fix within my research period), the number was significantly reduced compared to other systems without sacrificing path efficiency or accuracy and while having minimal computation time.
