---
layout: default

<script type="text/javascript"
  src="https://cdnjs.cloudflare.com/ajax/libs/mathjax/2.7.0/MathJax.js?config=TeX-AMS_CHTML">
</script>
<script type="text/x-mathjax-config">
  MathJax.Hub.Config({
    tex2jax: {
      inlineMath: [['$','$'], ['\\(','\\)']],
      processEscapes: true},
      jax: ["input/TeX","input/MathML","input/AsciiMath","output/CommonHTML"],
      extensions: ["tex2jax.js","mml2jax.js","asciimath2jax.js","MathMenu.js","MathZoom.js","AssistiveMML.js", "[Contrib]/a11y/accessibility-menu.js"],
      TeX: {
      extensions: ["AMSmath.js","AMSsymbols.js","noErrors.js","noUndefined.js"],
      equationNumbers: {
      autoNumber: "AMS"
      }
    }
  });
</script>


---
# CSCI 5611: Project 3
Sarah Boelter and Simanta Barman
## Simulation Images
![IK](ArtContest1.png)


![Crowd](ArtContest2.png)


## Features with Video and Descriptions

TO DO IS UPDATE THE README
Instructions for running simulations are located in gitHub readme in linked code below.

### Part 1: IK

#### Video 1
[Link to Video 1 if it doesn't render](https://www.youtube.com/embed/nQbxULdwHLg?si=wxOr16OzgcQNV0X6)
<iframe width="560" height="315" src="https://www.youtube.com/embed/nQbxULdwHLg?si=wxOr16OzgcQNV0X6" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


#### Video 2
[Link to Video 2 if it doesn't render](https://www.youtube.com/embed/sYhF_HGZLv0?si=WjgDNno71lfsZW9R)
<!-- <iframe width="560" height="315" src="https://www.youtube.com/shorts/WignRsKkIOM" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe> -->
<iframe width="560" height="315" src="https://www.youtube.com/embed/sYhF_HGZLv0?si=WjgDNno71lfsZW9R" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


#### Description of Simulation and Key Functions

The skeleton is made up of a shoulder joint that is rooted in the ground. The shoulder joint is connected to an elbow joint which is connected to two wrist joints. Each wrist joint is connected to a separate end effector. Both end effectors orient themselves to point at the mouse cursor and try to follow it. Changing the orientation of the end effector and following the mouse cursor is done by using the inverse kinematics algorithm.

Each segment of the skeleton has a predefined length, an initial angle (from the horizontal or $x$ axis) and a starting position. The starting position or the root of the skeleton is stored as a vector and represents the shoulder joint. Knowing the root position, the length ($l$) and the angle of the segment $$( \theta )$$ connecting the shoulder joint to the elbow joint allows us to calculate the position of the elbow joint. Projecting the length of the segment $l$ onto the $x$ and $y$ axis gives us $$l * cos(\theta)$$ and $$y = l * sin(\theta)$$vrespectively. To obtain the position of the elbow
joint we simply add projections to $x$ and $y$ coordinates of the root position. In other words, the elbow joint is located at $$(x + l * cos(\theta), y + l * sin(\theta))$$. Using this equation we updated the elbow joint. Now since the elbow joint is connected with the wrist joint by another segment and we updated the position of the elbow joint, we need to use the new position of the elbow joint to calculate the position of the wrist joint. 

Initially, the segment connecting the elbow and one of the wrist joints created $\theta_1$ angle with the horizontal axis. Since we changed the position of the elbow joint from it's initial position, the angle $\theta_1$ is no longer valid. To obtain the new angle we add angles from all the previous segments to $\theta_1$. In this case, $\theta_1' = \theta_1 + \theta$. Similarly, we
can calculate the next end-point of the current segment. This process of computing the new angle and using the length of the segment to calculate the new position of the end point is repeated until the end effecter is updated. This is known as forward kinematics.

The end effector following the mouse cursor results in changing the coordinates of the end effector. This changes the angle created by the segment connecting the wrist joint and the end effector. To obtain position of the end effector a vector can be created pointing from the start of this segment (wrist joint) to the cursor position. The vector can be calculated by subtracting the position of the wrist joint from the position of the cursor. To obtain the angle between the segment vector (pointing to the end effector) and the vector pointing to the cursor both from the wrist joint, we use the dot product of the two vectors. Before taking the dot product we normalize both vectors. We also clamp the dot product between $-1$ and $1$ to ensure that the arc cosine function is not evaluated with values for which it is undefined. Then taking the arc cosine of the dot product gives us the angle between the two vectors. Then based on the sign of the cross product of the two vectors we add or subtract the angle from the current angle of the segment. This updates the angle of the segment connecting the wrist joint and the end effector. This process is then propagated backwards to update the angles of all the previous segments. This is known as inverse kinematics.

To ensure collision occurs between the segments of the skeleton and the obstacles, we use the line segment and circle intersection test using Continuous Collision Detection (CCD). The line segment is created by connecting the start and end points of the segment. The circle is created using the radius of the obstacle. Since the segments of the skeleton does have width, we increase the radius of the circle by half of that width to ensure that we can treat the segment as a line. Then collision is checked between the line segment and the circle. If a collision is detected then the angle is
rolled back to the previous angle and incremental changes to the angle is made until collision. Note that rolling back to a previous angle requires going back to a previous state for the entire skeleton. For this, we need to use forward kinematics after changing the angles of the segments to ensure the segment end-points are updated correctly.

#### Attempted Point Breakdown and Timestamps

Video 1
* Single-Arm IK - 20 Points - Throughout video
* Multi-Arm IK - 20 Points - Throughout video - Base rotates back and forth for two arms, root stays stationary, joint limits are imposed in a way that makes sense for the setup, when the arm touches the ball, it changes colors in a rainbow.
* Joint Limits - 20 points - Throughout video, this video shows joint limits implemented on limbs
* User Interaction - 10 Points - Throughout video - Arm chases a ball on the screen.

Video 2
* IK Obstacles - 20 Points - Throughout video - Arms cannot make it through obstacles to get to the ball, they do not stick. 
* Joint Limits - 20 Points - This video shows the arms without imposed joint limits.


### Part 2: Crowd Simmulation
<iframe width="560" height="315" src="https://www.youtube.com/embed/Bh0Di3qIXho?si=mqgqybK1UU-5q_sx" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" allowfullscreen></iframe>


#### Description of Simulation and Key Functions
* TBD

#### Attempted Point Breakdown

* ...

## Writeup

Getting the IK simmulation was actually both really straightforward and very difficult.  We got a 3D version working fairly quickly, but struggled implementing angles in the Z direction while keeping the simmulation looking nice.  We eventually switched back to the 2D version of IK, which was much easier to work with, but We struggled getting collisions to work correctly.  Initially we thought it was my collision function so we tried doing several different shapes to do a workaround.  Eventually we realized that we weren't setting the arm back to the previous state if there was a collision, and only the state that had made it collide in the first place. 

## Code Location
[Link to code repository for IK and Crowd Simmulation](https://github.com/seboelter/Project3).

## Libraries and Tools
* Vec2 Library from In-Class Exercises

## Art Contest

![IK](ArtContest1.png)


![Crowd](ArtContest2.png)



