# Making Neatos Play Tag

For my Introduction to Computational Robotics final project, my group and I wanted with both a swarm of robots and fusing together multiple sensors. To do this, we decided to make the Neatos play tag with each other. Previous units in the class already involved mounting a camera onto the Neatos, so our solution was to use a combination of those Raspberry Pi cameras with the Neatos' built-in LiDAR system to create a sensor array to detect a semi-circle of sticky notes we placed on the back of each Neato. From there, we used the Neato's bump sensor to determine when one Neato has "tagged" another. By spinning up ROS2 nodes on a central computer, we could have each Neato play tag. You can see [our full website at this link](https://ajevans451.github.io/neato_tag/), including videos of the Neatos playing tag on their own.

## My Work

I did a majority of the sensing code for this project. I led development of an algorithm to detect semicircles of a given radius in a LiDAR point cloud, along with the integration of the camera with OpenCV to detect the different Neatos. Although this did not end up in the final version of the code, I also did extensive research into Kalman filtering, and how it can be used to combine these two pieces of data into one measurement of each other neato's position. Finally, I wrote the algorithm to determine the Neato's target path using a mixture of LiDAR-based obstacle avoidance and the location of other Neatos, which was fed to a potential field/gradient descent algorithm.

## Results

This project was very successful! We managed to get three Neatos playing tag with each other, with the ability to expand if we had access to more. These Neatos could pilot themselves through a game of tag until their batteries died. Additionally, we added a different mode to the code where the Neatos who were not "it" could be piloted remotely by people, while trying to avoid the Neato who was "it", autonomously controlled by our code. You can find [videos of the Neatos playing tag on our project website](https://ajevans451.github.io/neato_tag/assets/photos.html).
