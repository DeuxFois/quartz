---
title: 3.1 Three-Dimensional Graphics
updated: 2022-01-09 11:04:43Z
created: 2021-12-23 19:34:52Z
source: https://www.csee.umbc.edu/~ebert/435/notes/435_ch7.html
tags:
  - graphisme
  - s1
---

- We will use a right-handed coordinate system.
    
- Left-handed suitable to screens.
    
- To transform from right to left, negate the z values.
    

Right Handed Space                        Left Handed Space

![](../../../_resources/ch7_fig1_9c920fd320bf48d7a9001d2b5b11ac87.gif)

Transformations will be represented by 4x4 matrices.

## Scaling

|     |     |     |     |
| --- | --- | --- | --- |
| sx  | 0   | 0   | 0   |
| 0   | sy  | 0   | 0   |
| 0   | 0   | sz  | 0   |
| 0   | 0   | 0   | 1   |

## Translation

|     |     |     |     |
| --- | --- | --- | --- |
| 1   | 0   | 0   | tx  |
| 0   | 1   | 0   | ty  |
| 0   | 0   | 1   | tz  |
| 0   | 0   | 0   | 1   |

## Shear (xy)

|     |     |     |     |
| --- | --- | --- | --- |
| 1   | 0   | sh<sub>x</sub> | 0   |
| 0   | sh<sub>y</sub> | 0   |     |
| 0   | 0   | 1   | 0   |
| 0   | 0   | 0   | 1   |

## Rotation (along Z axis)

|     |     |     |     |
| --- | --- | --- | --- |
| cosq | -sinq | 0   | 0   |
| sinq | cosq | 0   | 0   |
| 0   | 0   | 1   | 0   |
| 0   | 0   | 0   | 1   |

## Rotation (along X axis)

|     |     |     |     |
| --- | --- | --- | --- |
| 1   | 0   | 0   | 0   |
| 0   | cosq | -sinq | 0   |
| 0   | sinq | cosq | 0   |
| 0   | 0   | 0   | 1   |

## Rotation (along Y axis)

**(sign of sine reversed to maintain right-handed coordinate system)**

|     |     |     |     |
| --- | --- | --- | --- |
| cosq | 0   | sinq | 0   |
| 0   | 1   | 0   | 0   |
| -sinq | 0   | cosq | 0   |
| 0   | 0   | 0   | 1   |

# Compositing 3D transformations

The example from the text:

Objective: Transform the directed line segments P<sub>1</sub>P<sub>2</sub> and P<sub>1</sub>P<sub>3</sub> from their starting position to their final position as indicated in the figure below. Thus,

- Point P<sub>1</sub> is to be translated to the origin,
- P<sub>1</sub>P<sub>2</sub> is to lie on the positive Z axis and
- P<sub>1</sub>P<sub>3</sub> is to lie in the positive y-axis half of the (y,z) plane.

![Example 1: Composite 3D Rotation/Translation](../../../_resources/ch7_fig2_577cd6b2883048dcb6faf029e627119f.jpg)

4 Steps:

- 1.  Translate P<sub>1</sub> to the origin
- 2.  Rotate about the Y axis

<img width="814" height="518" src="../../../_resources/ch7_fig3_7583c755ac2848e39f22497d18bb94fd.jpg" class="jop-noMdConv">

- 3.  Rotate about the X axis
- 4.  Rotate about the Z axis <img width="700" height="700" src="../../../_resources/ch7_fig4_fbc6e1386c51411eabae8751832f3787.jpg" class="jop-noMdConv">

* * *

Rotation About an Arbitrary Axis in Space

- Assume we want to perform a rotation about an axis in space passing through the point (x0, y0, z0) with direction cosines  (cx, cy, cz) by d degrees.  - How do we do this?

1.  First of all, we want to translate by -(x0, y0, z0)= |T|.
    
2.  Next, we rotate the axis into one of the principle axes, let’s pick Z (|Rx|, |Ry|).
    
3.  We rotate next by d degrees in Z ( |R<sub>z</sub>(d)|).
    
4.  Then we undo the rotations to align the axis.
    
5.  We undo the translation: translate  by (x0, y0, z0)
    

- The tricky part is (2) above.
    
- This is going to take  2 rotations,
    

1 about x  (to place the axis in the xz plane) and 1 about y (to place the result coincident with the z axis).

* * *

\- Rotation by Alpha about x:

How do we determine Alpha?

- Project  the vector into the yz plane as shown below.

The y and z components are cy and cz, the direction cosines of the unit vector along the arbitrary axis.

- It can be seen from the diagram below that

d= sqrt(cy2 + cz2), therefore cos(a) = cz/d sin(a) = cy/d

![](../../../_resources/ch7_fig_ex2_6d79828f2d7d4ab8bf7bb961ded5c4ac.gif)

* * *

\- Rotation by Beta about y:

How do we determine Beta?

Similar to above:

determine the angle Beta to rotate the result into the Z axis:

- The x component is cx and the z component is d.
- It can be seen from the diagram that:

cos(Beta)= d =  d/(length of the unit vector) sin(Beta)= cx =  cx/(length of the unit vector).

* * *

- Final Transformation:

M = |T|-1 |Rx|-1 |Ry|-1 |Rd| |Ry| |Rx| |T|

- If you are given 2 points instead, you can calculate the direction cosines as follows:

V =|(x1 -x0)| **   |(y1 -y0)|** **   |(z1 -z0)|**

cx =  (x1 -x0)/ |V| cy =  (y1 -y0)/ |V| cz =  (z1 -z0)/ |V|, where |V| is the length of V

* * *

Transformations as a Change in Coordinate Systems

![](../../../_resources/ch7_fig6_872fcc6ab1e54fe8a07b9d6539eb9f94.gif)

\- P<sup>(i)</sup> is the representation of the point P in  coordinate system i.

- M<sub>i<-j</sub> is the transformation that converts the representation of point in coordinate system j into coordinate system i.
    
- P<sup>(i)</sup> = M<sub>i<-j</sub> \* M<sub>j<-k</sub> \* P<sup>(k)</sup>
    
- The example Above:
    

M<sub>1<-2</sub> = T(4,2) **M<sub>2<-3</sub> = T(2,3)* S(0.5, 0.5)*\* **M<sub>3<-4</sub> = T(6.7,1.8)* R(-45)*\* **M<sub>1<-4</sub> =  T(6,5)* S(0.5, 0.5) \*T(6.7,1.8)\*R(-45)**

- Note that M<sub>i<-j</sub> = M<sub>j<-i</sub> <sup>-1</sup>

* * *

# The 3D Viewing Pipeline

- Objects are modeled in object (modeling) space.
    
- Transformations are applied to the objects to position them in world space.
    
- View parameters are specified to define the view volume of the world, a projection plane, and the viewport on the screen.
    
- Objects are clipped to this View volume.
    
- The results are projected onto the projection plane (window) and finally mapped into the viewport.
    
- Hidden objects are then removed, the objects scan converted and shaded if necessary.
    

<img width="619" height="191" src="../../../_resources/ch7_pipeline_7114683ba12c4351be2a65d4d04aa1a5.jpg" class="jop-noMdConv">

* * *

## Spaces

### Object Space

definition of objects. Also called Modeling space.

### World Space

where the scene and viewing specification is made

### Eyespace (Normalized Viewing Space)

where eye point (COP) is at the origin looking down the Z axis.

### 3D Image Space

A 3D Perspected space.

Dimensions: -1:1 in x & y, 0:1 in Z.

Where Image space hidden surface algorithms work.

- Screen Space (2D)

Coordinates 0:width, 0:height

* * *

## The Computer Graphics Pipeline Viewing Process

![](../../../_resources/ch7_viewinglagorithm_8b1802ebb7b44ce2a951f93eca061.gif)

* * *

# Projections

We will look at several planar geometric 3D to 2D projection:

### Parallel Projections

Orthographic Oblique

### Perspective

- Projection of a 3D object is defined  by straight projection  rays (projectors) emanating from the center of projection (COP) passing through each point of the object and intersecting the  projection plane.

* * *

\- Perspective Projections:

- Distance from COP to projection plane is finite.
    
- The projectors are not parallel  & we specify a center of projection.
    
- Center of Projection is also called the Perspective Reference Point COP = PRP
    
- Perspective foreshortening: the size of the perspective projection of the object varies inversely with the distance of the object from the center of projection.
    
- Vanishing Point: The persepctive projections of any set of parallel lines that are not parallel to the projection plane converge to a vanishing point.
    

<img width="500" height="238" src="../../../_resources/ch7_vanish_f379fdde265847cda5845feebc3b67ec.jpg" class="jop-noMdConv">

- Example:

<img width="485" height="443" src="../../../_resources/ch7_proj2_e6a2172c0c134d0f9b057cf87ec22b86.jpg" class="jop-noMdConv">

* * *

- Example: 2 Point Perspective Projection

![](../../../_resources/ch7_proj3_607e59eff826429d9a2ebf5b3eec1306.jpg)

* * *

\- Parallel Projection

- Distance from COP to projection plane is *infinite.*
    
- Therefore,  the projectors are parallel lines & we specify a direction of projection (DOP)
    
- Orthographic: the direction of projection and the normal to the projection plane are the same. (direction of projection is normal to the projection plane)
    
- Example Orthographic Projection:
    

![](../../../_resources/ch7_proj4_bbf59547a91c41dba572216ec49175f1.jpg)

- Axometric orthographic projections use planes of projection that are not normal to a principal axis. (they therefore show mutiple face os an object.)
    
- Isometric projection: projection plane normal makes equal angles with each principle axis
    

All 3 axis are equally foreshortened allowing measurements along the axes to be made with the same scale.

- Example Isometric Projection:

<img width="800" height="425" src="../../../_resources/ch7_proj5_2d45aa9704d74925bde1aba63db159fb.jpg" class="jop-noMdConv">

* * *

- Oblique projections : projection plane normal and the direction of projection differ.
    
- Plane of projection is normal to a principle axis
    
- Projectors are not normal to the projection plane
    
- Example Oblique Projection
    

![](../../../_resources/ch7_proj6_aa50663ba1604c7f8ebbd0b7b348a10a.jpg)

* * *

Classification of Geometric Projections:

<img width="730" height="530" src="../../../_resources/ch7_proj7_b6cbf016653e4d588df3ed15295068d8.jpg" class="jop-noMdConv">

* * *

Note: Most of the figures in this chapter are scanned from and copyrighted in *Introduction to Computer Graphics* by Foley, Van Dam, Feiner, Hughes, and Phillips, Addison Wesley 1994.

* * *