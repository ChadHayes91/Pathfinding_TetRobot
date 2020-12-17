<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Project Description and Code

Link to Code: [https://github.com/ChadHayes91/Graphics_AStar_TetRobot](https://github.com/ChadHayes91/Graphics_AStar_TetRobot)

This general goal of this project is to create an almost spider-looking robot that pathfinds to the user's mouse location. Movement is constrained to only three directions: right, left, or opposite, which is accomplished by the regular tetrahedron core "tumbling" from the top-most vertex (the only vertex in a tetrahedron which is not a part of the base equilateral triangle). This project was split into four "phases" which start simply and build off each other which I've outlined below. The end result of this project (and its final visualization) can be found in the YouTube video posted at the bottom of this page. 

#### Phase One: Base Tetrahedron Creation and Movement Computations
We start by creating a circle of radius 200 (200 units in Processing) centered at the origin (point (0, 0, 0)) and create the three vertices at each 120 degree increment (120, 240, 360 or 0 degrees). These three vertices form an equilateral triangle which can serve as the base for the regular tetrahedron. The final vertex (the top vertex) of this tetrahderon can be computed by applying the pythagorean theorem: $$s * \sqrt(2 / 3)$$ where $$s$$ is the length of an arbitrary side of our base equilateral triangle (since all sides have the same length). Finally, the centroid of this tetrahedron is computed by taking the average of all four vertices.

I've provided below visualizations of the three possible movements: right, left, and opposite. The grey tetrahedron is the ending position and each vertex is colored to visually show how each vertex changes positions.

<p align="center">
  <img width="200" height="200" src="https://github.com/ChadHayes91/Pathfinding_TetRobot/blob/master/Images/Tet_Left.PNG?raw=true">
  <img width="200" height="200" src="https://github.com/ChadHayes91/Pathfinding_TetRobot/blob/master/Images/Tet_Right.PNG?raw=true">
  <img width="200" height="200" src="https://github.com/ChadHayes91/Pathfinding_TetRobot/blob/master/Images/Tet_Oppo.PNG?raw=true">
</p>
<p align = "center">
   Figure 1: left move &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
   Figure 2: right move &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
   Figure 3: opposite move
</p>

Computing a move is simple:  one of the edges of the original tetrahedron remains the same (and therefore, two vertices remain in the same position) and since we need to maintain a base equilateral triangle we can rotate the unmoving edge 60 degrees to find the location of the final vertex in the base equilateral triangle. We rotate 60 degrees since all angles in an equilateral triangle are 60 degrees. Now that we know the location of the three vertices which form the base equilateral triangle, we can re-compute the location of the top vertex using the same formula (s * sqrt(2 / 3)) mentioned above.

Note the formula for rotating a vector around a point with an accompanying explaination and proof can be found here:
[https://matthew-brett.github.io/teaching/rotation_2d.html](https://matthew-brett.github.io/teaching/rotation_2d.html)

#### Phase Two: Animation
At this point, we've simply used geometry/trigonometry to compute the starting and end points for all four vertices for each possible movement. However, we need to know the location of each vertex for our current tetrahedron at each frame in order to animate our robot. As previously mentioned, for each possible movement, two vertices stay in the same location (which two vertices depends on the movement direction). Additionally, movement is consistent regardless of the direction: the vertex currently at the top of the tetrahedron moves to the furthest position, and the vertex furthest from the edge that remains in the same location will become the new vertex at the top of the tetrahedron. Since we need to preserve the structure of the regular tetrahedron at each frame, we can compute the position of the two moving vertices at each frame by using Rodrigues’ rotation formula:


#### Phase Three: Leg Computations and Inverse Kinematics

#### Phase Four: Pathfinding Using A*



### Video Demonstration

<iframe width="560" height="315" src="https://www.youtube.com/embed/RLJW0QGB1VQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
