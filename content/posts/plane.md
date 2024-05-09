+++
title = "Drawing a plane perpendicular to a given line in Plotly"
description = ""
tags = ["script"]
categories = ["Visualization",]
math = true
+++ 

When I am working on my binder design task, I need to select suitable hotspot residues that can lead to design better binders.
In addition to hydrophobic amino acids, I also want to focus on the amino acids whose positions are close to the outside edge, and far away from the center of the protein.
Therefore, I expect a plane that can approximately cut the protein layer by layer with my expected distance to the outside edge.

By calculating the centers of two parts of my protein, I can easily determine one line. However, it took me lots of time to find the plane that can be perpendicular to this line.

Finally, I find the solution! 💪 TL;DR, [code is here!](https://github.com/Huilin-Li/plotly/tree/main)

<div align="center">
<img src="../images/plane2.png" alt="" width="500" height="auto">
<p><em>A plane perpendicular to the given line</em></p>
</div>

Assuming that we want to make a plane whose vertical distance to $p_1$ is $d01$. We can determine the point $p0$ by 
```python
from shapely.geometry import LineString

line = LineString([p1, p2])
p0_tmp = line.interpolate(d01)
p0 = np.array([p0_tmp.x, p0_tmp.y, p0_tmp.z])
```

<div align="center">
<img src="../images/plane1.png" alt="">
<p><em>5 important points who determine the plane</em></p>
</div>
Finding $p_3$, $p_4$, $p_5$ and $p_6$ took me lots of time. I finally find that they can be found by vector calculation. 
For example, we want the distance between $p_0$ and $p_3$/$p_4$/$p_5$/$p_6$ is $Radius = 3$. 

```python
P1 = p0
P2 = p2
Radius = 3
V3 = P1 - P2
V3 = V3 / np.linalg.norm(V3)
e = np.array([0,0,0])
e[np.argmin(np.abs(V3))] = 1
V1 = np.cross(e, V3)
V1 = V1 / np.linalg.norm(V3)
V2 = np.cross(V3, V1)
# p3 90 degree
s3 = np.pi/2
p3 = P1 + Radius*( np.cos(s3)*V1 + np.sin(s3)*V2 ) 
# p4 180 degree
s4 = np.pi
p4 = P1 + Radius*( np.cos(s4)*V1 + np.sin(s4)*V2 ) 
# p5 270 degree
s5 = np.pi*1.5
p5 = P1 + Radius*( np.cos(s5)*V1 + np.sin(s5)*V2 )
# p6 0 degree
s6 = 0
p6 = P1 + Radius*( np.cos(s6)*V1 + np.sin(s6)*V2 )
```

Becasue three points determine one plane. We can finally make the plane by
```python
plane1_x, plane1_y, plane1_z = np.array([p6, p3, p5]).T
fig.add_trace(go.Mesh3d(x=plane1_x, y=plane1_y, z=plane1_z, color="lightgreen", name = "", hoverinfo="skip", opacity=0.50))

plane2_x, plane2_y, plane2_z = np.array([p4, p3, p5]).T
fig.add_trace(go.Mesh3d(x=plane2_x, y=plane2_y, z=plane2_z, color="lightgreen", name = "", hoverinfo="skip", opacity=0.50))
```

