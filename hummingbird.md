# Path Planning with Hummingbird

## The Problem

In the summer of 2022, I worked in the Olin College RoboLab on a project called
Hummingbird. The goal of this project was to create a swarm of drones that could
each complete different tasks in a cramped, enclosed environment. For that
summer, the team's goal was to provide proof of concept within a ten week
period.

My specific task was to create a path planning algorithm and to test it in a
simulation. I spent my first few weeks browsing the literature on current
implementations of drone swarm algorithms, where I found and implemented
algorithms such as gradient descent, particle swarm optimization, and artifical
potential fields. However, each one I implemented had one or more of the
following issues:

- High computation time, especially to avoid other drones. This would cause a
  drone to either stay put while it or another computer worked out its path
- Optimized for a static environment. The presence of other drones moving along
  vastly different paths would often mess up many algorithms
- Imprecision. Many algorithms were designed for flying around large, open areas
  in which a difference of a meter is not large. For a cramped, enclosed
  environment, this would not do.

## The Solution

Because of these issues, I created my own algorithm that I have not seen
elsewhere in my literature review, which I named Dynamic Gradient Descent (DGD),
which is largely based on the gradient descent algorithm. In the classic
algorithm, the environment is pre-mapped and target pre-planned. Using that
information, the drone will create a topographical map of its surroundings,
where the target is a "sink" (like a valley), and each obstacle is a "source"
(like a mountain). It would then place a metaphorical ball at its current
position on that map, and "roll" it down the hill until it reached the valley -
the target. The drone will then follow that exact path to reach the target
itself.

This works great in a static environment with plenty of time to compute a path.
However, if the obstacles move the planned path is no longer valid.
Additionally, with many obstacles, calculation time vastly increases. To solve
these issues, I decided to allow for real-time calculation. With DGD, every loop
(a fraction of a second long, ideally), the drone will take a single "step"
forward. To calculate that step, it will take the current positions of all the
other drones, along with any other obstacles, and create the same topographical
map, and place a ball at its position. It will then let the ball roll down the
hill, except only for a small amount of time. It will then follow the direction
the ball moved until it recalculates the target velocity in the next loop,
repeating until the goal is met.

DGD fixes both of the issues gradient descent has with one change. Firstly, it
no longer needs long computation times before the drone flies, as that
computation is spread out over the entire flight. Secondly, it allows the drone
to poll the current locations of the other drones each loop, instead of guessing
where they will be some amount of time in the future. This allows for easy,
real-time course corrections without precalculating the path of every drone in
the swarm.

## Results

TODO: Pull pictures and videos from last summer
