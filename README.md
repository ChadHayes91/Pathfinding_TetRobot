<script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>

# Project Description and Code

Link to Code: [https://github.com/ChadHayes91/Graphics_AStar_TetRobot](https://github.com/ChadHayes91/Graphics_AStar_TetRobot)

This general goal of this project is to create an almost spider-looking robot that pathfinds to the user's mouse location. Movement is constrained to only three directions: right, left, or opposite, which is accomplished by the regular tetrahedron core "tumbling" from the top-most vertex (the only vertex in a tetrahedron which is not a part of the base equilateral triangle). This project was split into four "phases" which start simply and build off each other which I've outlined below. The end result of this project (and its final visualization) can be found in the YouTube video posted at the bottom of this page. 

#### Phase One: Base Tetrahedron Creation and Movement Computations
We start by creating a circle of radius 200 (200 units in Processing) centered at the origin (point (0, 0, 0)) and create the three vertices at each 120 degree increment (120 or $$\frac{2\pi}{3}$$, 240 or $$\frac{4\pi}{3}$$, 360 degrees or $$2\pi$$ radians). These three vertices form an equilateral triangle which can serve as the base for the regular tetrahedron. This uses the fact that an equilateral triangle can be inscribed inside a circle. The final vertex (the top vertex) of this tetrahderon can be computed by applying the pythagorean theorem: $$s \times \sqrt{\frac{2}{3}}$$ where $$s$$ is the length of an arbitrary side of our base equilateral triangle (since all sides have the same length). Finally, the centroid of this tetrahedron is computed by taking the arithmetic average of all four vertices, or in other words, the geometric point which computed by taking the average of all $$x$$, $$y$$, and $$z$$ coordinates.

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

Computing a move is simple:  one of the edges of the original tetrahedron remains the same (and therefore, two vertices remain in the same position), and since we need to maintain a base equilateral triangle, we can rotate the unmoving edge 60 degrees to find the location of the final vertex in the base equilateral triangle. We rotate 60 degrees since all angles in an equilateral triangle are exactly 60 degrees. Now that we know the location of the three vertices which form the base equilateral triangle, we can re-compute the location of the top vertex using the same formula $$\big(s \times \sqrt{\frac{2}{3}}\big)$$ mentioned above.

Note the formula for rotating a vector around a point with an accompanying explaination and proof can be found here:
[https://matthew-brett.github.io/teaching/rotation_2d.html](https://matthew-brett.github.io/teaching/rotation_2d.html)

#### Phase Two: Animation
At this point, we've used geometry and trigonometry to compute the starting and end points for all four vertices for each possible movement. However, we need to know the location of each vertex for our current tetrahedron at each frame in order to animate our robot. As previously mentioned, for each possible movement, two vertices stay in the same location (which two vertices depends on the movement direction). Additionally, some aspects of movement are consistent regardless of the direction: the vertex currently at the top of the tetrahedron moves to the furthest position, and the vertex furthest from the edge that remains in the same location will become the new vertex at the top of the tetrahedron. Since we need to preserve the structure of the regular tetrahedron at each frame, we can compute the position of the two moving vertices at each frame by using Rodrigues’ rotation formula:
<p align="center">
$$W = \cos(\alpha)U + \sin(\alpha)\underline{V} \times U + (1 - \cos(\alpha))\big(\underline{V} \cdot U)\underline{V}$$
</p>

Where:
* $$U$$ is the vector we are rotating
* $$\underline{V}$$ is the axis of rotation; this is represented as a normalized vector (its magnitude is 1)
* $$\alpha$$ is the angle that we're rotating $$U$$ around $$\underline{V}$$ 
* $$\times$$ represents the cross product operation
* $$\cdot$$ represents the dot product operation
* $$W$$ is the resulting vector after rotating $$U$$ around $$\underline{V}$$ for $$\alpha$$ radians

In our example, consider the vector (or edge of our base equilateral triangle) that does not move at all during rotation. We can use normalize this vector and use it as our axis of rotation. Let's call this vector AB (where A and B are two vertices). We can name the top vertex T and consider the vector AT, this will be the vector we are rotating. Finally, we need to find $$\alpha$$; which can be done by taking the dot product of vector AT and the vector AF where F is vertex T's final location after fully rotating calculated in the previous phase. Since the dot product can be interpreted in multiple ways: $$AT \cdot AF = AT.x * AF.x + AT.y * AF.y + AT.z * AF.z = \|AT\| \|AF\| cos(\alpha)$$, we can back into the angle between these two vectors. We can then divide $$\alpha$$ by the time between each frame to find where vector AT should be rendered at each frame. We can repeat this same process for the other moving vertex.

Further information about Rodrigues' formula as well as a proof can be found here: [https://www.youtube.com/watch?v=Fh3nMi87cB8](https://www.youtube.com/watch?v=Fh3nMi87cB8). 
<br />
<br />
Note that the formula I've used above uses the right-hand rule and the linked video computes rotation in the opposite direction.

#### Phase Three: Leg Computations and Inverse Kinematics

Since we are inevitably animating a robot with legs instead of only a large tetrahedron, we no longer need to render the entire tetrahedron shown in the images above. We still compute the start and ending position of all vertices like the large tetrahedron was still there, but we now replace what's really rendered with a smaller tetrahedron centered around the centroid of the previous large tetrahedron. This new, smaller tetrahedron is used as the robot's core (now called TetRobot's core) where we will attach legs to. Legs will eventually attached to each of the four vertices of the TetRobot core and span to the location of the vertices of the large, non-rendered tetrahedron. Computations for the TetRobot's core movement are conducted in the same manner as the ones mentioned above used for the large tetrhedron, but simply on a smaller scale.
<br />
<br />
Next, we need to attach legs to each of the four vertices in our TetRobot core which involves some inverse kinematics calculations. Consider the vector HF which spans from a hip vertex (H) to the location of its closest corresponding "foot" (F). A "hip" vertex is one of the four vertices which define the TetRobot core. A "foot" point is one of the four vertices of the large, non-rendered tetrahedron. A straight line from the hip to its closest corresponding foot is first constructed. However, using a leg as a straight line looks terribly unnatural, so we need to construct a bent leg. In order create a bent leg, consider the following steps:

*Consider an arbitrary up vector named $$U$$ with magnitude 1: $$U = (0, 0, 1)$$
*Compute the cross product $$HF \times U = N$$
*Compute yet another cross product: $$HF \times N = T$$
*The vector $$T$$ points in the direction we're interested in
*Finally, normalize $$T$$ and multiply it by the scalar $$\big(\|HF\| * 0.6 \big) \underline{T} = K$$
*K is now the geometric point of our knee; we can now store and render vectors $$HK$$ and $$KF$$ (hip to knee and knee to foot)
*This process is repeated for all four legs

<p align="center">
  <img width="200" height="200" src="https://github.com/ChadHayes91/Pathfinding_TetRobot/blob/master/Images/Double_Cross_P3_Vis.PNG?raw=true">
</p>
<p align = "center">
   Figure 4: Bent Leg Computations &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;
</p>

SLERP for animation

#### Phase Four: Pathfinding Using A*



### Video Demonstration

<iframe width="560" height="315" src="https://www.youtube.com/embed/RLJW0QGB1VQ" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
