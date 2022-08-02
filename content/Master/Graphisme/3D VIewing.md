# Specification of a View Volume

### View Plane (projection plane) is defined by :

- VRP = View Reference Point
    
- VPN = View Plane Normal
    
- Window on the View Plane: To specify a min & max value for 2 orthogonal directions on the view plane, we introduce the Viewing Reference Coordinate(VRC) System.
    
- then define umin, umax, vmin,vmax
    

### View Reference Coord. system(VRC):

- Origin = VRP
- n axis: VPN
- v axis: Projection of View Up Vector (VUP) onto the View Plane
- u axis: an axis mutually orthogonal to n & v to make a RH coordinate system

<img width="548" height="244" src="../../../_resources/ch7_view1_490a22058b94405d810e78bdd6078041.jpg" class="jop-noMdConv">

Projection:

<img src="../../../_resources/ch7_view2_9cdf242f0ae343898a5a6a7fe7f0d649.jpg" alt="" width="561" height="217" class="jop-noMdConv">

- Center of Window (CW)
    
- Projection Reference Point (PRP) - defines the center of projection and the direction of projection (also called COP)
    
- specified in VRC, not world coordinates
    

\- Projection type

- Perspective Projection:

![](../../../_resources/ch7_view3_d4b5d262cd01495ea100a0fef1f75316.jpg)

- Parallel Projection:

![](../../../_resources/ch7_view3_2_60557f3281df43c5a0c5f85463e073f0.jpg)

* * *

- The View Volume is then the portion of the world that is to be projected onto the projection plane and then transformed to the viewport.
    
- Perspective View volume: truncated pyramid with apex at the PRP (COP) and the edges passing through the corners of the window.
    
- Introduce  hither(front) and yon(back) clipping planes to give a finite view volume.
    

![](../../../_resources/ch7_view4_e1531327fe98480885f2abdee50b4c88.jpg)

- Reasons for doing this:
    
- Clip away objects that are too close to the PRP and would make the rest of the scene hard to understand.
    
- Clip away objects that are very far away and won’t add any useful information to the final scene. This will reduce the computational expense of computing the final image.
    

* * *

- Default Viewing parameters in Normalized Projection coordinates(NC) or WC:

VRP(WC)=(0,0,0),VUP(wc)=y,VPN(wc)=Z, PRP(nc)= (.5, .5, 1)

<img width="749" height="538" src="../../../_resources/ch7_view5_ec4af3f94401458fbbed433abd92978b.jpg" class="jop-noMdConv">

- (a) The Default Viewing Specification
- (b) Default Parallel Projection View Volume
- © Defaul Perspective Projection View Volume

* * *

# Viewing Transfomations

Basic Case

- Assume VP is normal to  the Z axis
    
- PRP=(0,0,0)
    
- The projection plane is at z=d ![](../../../_resources/ch7_view6_ba97ac0cabd347299098442f81d3bde9.jpg)
    

\- By Similar triangles,

X<sub>p</sub>/d = x/z        Y<sub>p</sub>/d = y/z

Xp = x/ (z/d)   Yp = y/(z/d)

In a 4x4 Matrix, this becomes

| 1  0     0    0 | Mper= | 0  1     0    0 |       | 0  0     1    0 |       | 0  0    1/d   0 |

\[X Y Z W\]<sup>T</sup> = Mper * \[x y z 1\]<sup>T</sup> = \[ x y z z/d\]

Now projecting back to 3 space (divide by w)

\[ x/(z/d)  y/(z/d) d\].

* * *

Projections and the Canonical View Volume

- The canonical perspective view volume:

![](../../../_resources/ch7_view7_45ed9bb4d0dd4be78b45185c42084b70.jpg)

- 6 clipping planes:

x=z, x= -z, y=z, y=-z, z=z<sub>min</sub>,  z= -1

- Why clip against this?

> - Easier
>     
> - Suitable for perspective
>     

- So, we need to find the transformation that takes our perspective view volume and

transforms it to the canonical view volume

- In Summary,

3D Viewing Process

![](../../../_resources/ch7_view8_a47a67a8a2154a02aea2b2f7246b00d4.jpg)

* * *

# 3D Clipping

- We will look at entending Sutherland-Hodgman to 3D.
    
- Others can be extended just as easily.
    
- For perspective canonical view volume, the  tests are real simple:
    

Inside if:

Left:   x > z Right:  x &lt; -z Bottom: y &gt; z Top:    y &lt; -z Back:   z &gt; -1 Front:  z < zmin

Sample Intersection Calculation:

use parametric representation of the line:

x = x0 + t (x1-x0)      (1) y = y0 + t (y1-y0)      (2) z = z0 + t (z1-z0)      (3)

y=z plane:

equation 2=equation 3 gives t = (z0 - y0) /((y1- y0) - (z1 -z0)) plug back into equations 1 & 2 to get x & y, know z=y.

* * *

# BackFace Removal (culling)

- Assume we have outward pointing polygon normals ( & they are normalized).

Take cross product of first edge with last edge , since polygons defined clockwise.

(P0->P1 x Pn P0).

- Form the eye vector (PRP -P0 ) & normalize.
    
- If we take N · E, this gives us the cosine of  the angle between them (* magnitude of each vector =1).
    
- The polygon is facing us if the angle is <= 90 degrees.
    
- Reject as a backface if N · E  < 0.
    
- What will this do for us?
    
- It gets rid of polygons that we won’t see in closed opaque objects.
    
- In wireframes of a convex polyhedron, it does hidden line removal.
    
- In shaded rendering of a closed polyhedron, it does hidden surface removal.
    

\- How much computation should this save?

* * *

Note: Most of the figures in this chapter are scanned from and copyrighted in *Introduction to Computer Graphics* by Foley, Van Dam, Feiner, Hughes, and Phillips, Addison Wesley 1994.

* * *