
<h1 style="text-align: center;">Homework 2: Mesh Edit</h1>
<h2 style="text-align: center;">David Ban </h2>
<h2 style="text-align: center;">Sp 2024 </h2>




# Overview


This project aims to go over the fundamental topics of geometric modeling, including Bezier curves and half edge data structures. These implementations included mesh operations such as edge manipulations (such as flips and splits) along with subdivision.


# Task 1


To generate Bezier curves, I added the de Casteljau's algorithm, which would apply linear interpolation n-1 times (once for each pair of connected points) for a step when there are n points, and return us new n-1 control points. We could then apply this until there is only one point left, which would become a point on the Bezier curve. 



| ![](1.png)  <center><b>Level 0</b></center>|  ![](2.png) <center><b>Level 1</b></center>|
|--|--|
|  ![](3.png) <center><b>Level 2</b></center>|  ![](4.png) <center><b>Level 3</b></center>|
| ![](5.png) <center><b>Level 4</b></center> |  ![](6.png) <center><b>Level 5</b></center>|
| ![](7.png) <center><b>Completed Curve</b></center> |  ![](8.png) <center><b>Modified t-value and shifted points</b></center>|



# Task 2

In order to evaluate Bezier surfaces, I edited 3 functions. You can extend Task 1 to here, by just turning it into 3d - the concept remains the same.
- `evaluateStep()`: This is the same as the previous task, except now in 3 dimensions instead of 2. It still uses de Casteljau's algorithm and calculates the next step.
- `evaluate1D()`: Recursively call `evaluateStep()` on the list of 3D points until one point remains, which it will then return
- `evaluate()`: This will first iterate through each row of the initial 2D array of control points, and then evaluate them at their `u` position using the `evaluate1D` function, and then adding that point into a new list of points. After that, we can run `evaluate1D` on the list of new points to return the final surface point.

![](9.png)
peep that 11 framerate though


# Task 3

To compute the area weighted vertex norms, I iterated through the halfedges to get to every face that is connected to the given vertex. Then, for each face, I computed the area weighted normal by using the orthogonal vector, and weighted it by the normal. I then added all of these weighted normals, then normalized the final vector to give us the approximate unit normal. 

| ![](10.png)  <center><b>Teapot with Flat Shading</b></center>|  ![](11.png) <center><b>Teapot with Phong Shading</b></center>|
|--|--|

# Task 4
I implemented the edge flip operation by storing every single pointer to each halfedge, edge, face, and vertex of the initial triangle. Then, I updated the pointers (after many hairs were pulled out).

- This was particularly hard for me just because it was difficult for me to totally visualize the entire edge flip. I had to draw it out (in code), which I've shown below.
```
                  c                              c
             h1   |     h5                h2     h0    h1
       a        h0|h3        d        a                     d
                  |                        h4    h3   h5
             h2   b      h4                      b

```

| ![](13.png)  <center><b>Regular teapot</b></center>|  ![](12.png) <center><b>Teapot with edge flips</b></center>|
|--|--|



# Task 5
To do an edge split, I moved the initial splitting edge such that it becomes 3 additional edges. I then, similarly to Task 5, saved all of the edges, half edges, etc, and then added the extra parts as well. Finally, at the very end, I changed the pointers to have the correct orientation.
- A tip that I kept to keep myself sane is that I checked to make sure how many extra edges I needed, and if that checked out in my code. And if so, which edges correlated to each one. This was much simpler after the headache that was task 4 for me.


| ![](13.png)  <center><b>Regular teapot</b></center>|  ![](14.png) <center><b>Teapot with edge splits</b></center>|
|--|--|

| ![](13.png)  <center><b>Regular teapot</b></center>|  ![](15.png) <center><b>Teapot with edge flips and edge swips</b></center>|
|--|--|

# Task 6

To implement loop subdivision, I followed the steps in the spec which is summarized as follows:
- Iterate through all the original vertices, and find the new vertex value by calculating the degree of the vertex, and the weight of u. I then stored it into the `newPosition` field.
- Iterate through all the original edges, and compute the position of the new vertex that would get split during the subdivision, and also save this in the `newPosition` field, but for the edge this time.
- Call `splitEdge()` on each original edge, ensuring that the `isNew` value for each newly created edge is true. Also make sure to set the position of the new vertex position to that stored value from part 2. 
- Iterate through each newly created edge, and call `flipEdge()` if the edge is connected to an original vertex and a new vertex.
- Copy the precomputed newPosition values onto the vertex's actual position value.


| ![](21.png)  <center><b>No Subdivision</b></center>|  ![](16.png) <center><b>1 Iteration of Subdivision</b></center>|
|--|--|
| ![](17.png)  <center><b>2 Iterations of Subdivision</b></center>|  ![](18.png) <center><b>3 Iterations of Subdivision</b></center>|
| ![](19.png)  <center><b>4 Iterations of Subdivision</b></center>|  ![](20.png) <center><b>5 Iterations of Subdivision</b></center>|


I noticed (which you can see on the cube above) that sharp corners and edges are smoothed out, which is not necessarily always desired. It should still look like a cube after subdivision, not a weird, assymetrical, semi-spherical blob shape. 
- This could be remedied by making sure that each face is identical, making it non-assymetric.


| ![](22.png)  <center><b>No Subdivision with identical faaces</b></center>|  ![](23.png) <center><b>1 Iteration of Subdivision</b></center>|
|--|--|
| ![](24.png)  <center><b>2 Iterations of Subdivision</b></center>|  ![](25.png) <center><b>3 Iterations of Subdivision</b></center>|
| ![](26.png)  <center><b>4 Iterations of Subdivision</b></center>|  ![](27.png) <center><b>5 Iterations of Subdivision</b></center>|
