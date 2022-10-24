---
layout: post
title: Transforms
categories:
- Cheat sheets
tags:
- Graphics
---

{% raw %}

## Homogeneous Matrices

Right handed coordinate. Counterclockwise rotation.

$$
S = \begin{bmatrix} S & 0 & 0 & 0 \\
0 & S & 0 & 0 \\
0 & 0 & S & 1\end{bmatrix}
$$

$$
T = \left [ \begin{array}{c|c} I & \vec{d} \\
\hline  0 & 1 \end{array} \right ]
$$

$$
R_x = \begin{bmatrix} 1 & 0 & 0 \\
0 & \cos{\theta_x} & -\sin{\theta_x} \\
0 & \sin{\theta_x} & \cos{\theta_x} \end{bmatrix} 
$$

$$
R_y = \begin{bmatrix} \cos{\theta_y} & 0 & \sin{\theta_y} \\
0 & 1 & 0 \\
-\sin{\theta_y} & 0 & \cos{\theta_y} \end{bmatrix} 
$$

$$
R_z = \begin{bmatrix} \cos{\theta_z} & -\sin{\theta_z} & 0 \\
\sin{\theta_z} & \cos{\theta_z} & 0 \\
0 & 0 & 1 \end{bmatrix} 
$$

$$
R_{yzx} = \begin{bmatrix} c_y c_z & s_y s_x - c_y c_x s_z & c_x s_y + c_y s_z s_x \\
 s_z & c_z c_x & - c_z s_x \\
-c_z s_y & c_y s_x + c_x s_y s_z & c_y c_x - s_y s_z s_x \end{bmatrix}
$$

$$
R_{xzy} = \begin{bmatrix} c_z c_y & - s_z & c_z s_y \\
s_x s_y + c_x c_y s_z & c_x c_z & c_x s_z s_y - c_y s_x \\
c_y s_x s_z - c_x s_y & c_z s_x & c_x c_y + s_x s_z s_y \end{bmatrix}
$$

$$
R_{zyx} = \begin{bmatrix}  c_z c_y & c_z s_y s_x - c_x s_z & s_z s_x + c_z c_x s_y \\
 c_y s_z & c_z c_x + s_z s_y s_x & c_x s_z s_y - c_z s_x \\
 -s_y & c_y s_x & c_y c_x  \end{bmatrix}
$$

$$
R_{zxy} = \begin{bmatrix} c_z c_y - s_z s_x s_y & - c_x s_z & c_z s_y + c_y s_z s_x \\
c_y s_z + c_z s_x s_y & c_z c_x & s_z s_y - c_z c_y s_x \\
-c_x s_y & s_x & c_x c_y \end{bmatrix}
$$

$$
R_{xyz} = \begin{bmatrix} c_y c_z & - c_y s_z & s_y \\
c_x s_z + c_z s_x s_y & c_x c_z - s_x s_y s_z & - c_y s_x \\
s_x s_z - c_x c_z s_y & c_z s_x + c_x s_y s_z & c_x c_y \end{bmatrix}
$$

$$
R_{yxz} = \begin{bmatrix} c_y c_z + s_y s_x s_z & c_z s_y s_x - c_y s_z & c_x s_y \\
 c_x s_z & c_x c_z & - s_x \\
c_y s_x s_z - c_z s_y & c_y c_z s_x + s_y s_z & c_y c_x \end{bmatrix}
$$

$$
H = S \times T \times R
$$

$$
H^{-1} = \left [ \begin{array}{c|c} \frac{1}{S} \times R^T & -R^T \times \vec{d} \\
\hline  0 & 1 \end{array} \right ]
$$

Global transformations are pre-multiply.

Local transformations are post-multiply.

To transform from right handed to left handed, simply reverse the z-axis component.

## Quaternion

$\vec{q} = \binom{\cos{\frac{\theta}{2}}}{\sin{\frac{\theta}{2}} \cdot \hat{u}}$, where $\theta$ is angle, $\hat{u}$ is axis.

Denote $\cos{\frac{\theta}{2}}$ by $S$, $\sin{\frac{\theta}{2}}\cdot \hat{u}$ by $\vec{V}$:

$\vec{q}_1 + \vec{q}_2 = \binom{S_1 + S_2}{\vec{V}_1 + \vec{V}_2}$

$\vec{q}_1 \times \vec{q}_2 = \binom{S_1 S_2 - \vec{V}_1 \vec{V}_2}{S_1\vec{V}_2+S_2\vec{V}_1+\vec{V}_1\times\vec{V}_2}$

$\left\| \vec{q} \right\| = \sqrt{S^2 + \left\| \vec{V} \right\| ^2}$

$\vec{q}^{-1} = \frac{1}{\left\| \vec{q} \right\|^2} \binom{S}{-\vec{V}}$

To rotate a vector $\vec{p}$ by quaternion $\vec{q}$: 

$\vec{q} \cdot \binom{0}{\vec{p}} \cdot \vec{q}^{-1}$

To convert a rotation matrix to quaternion (assume `mat3` is row-major):

```cpp
double tmp = rot[0][0] + rot[1][1] + rot[2][2];
if (tmp <= -1.0) {
    if ((rot[0][0] > rot[1][1]) && (rot[0][0] > rot[2][2])) {
        V.x = sqrt(1.0 + rot[0][0] - rot[1][1] - rot[2][2]) * 0.5;
        S = (rot[2][1] - rot[1][2]) / V.x * 0.25;
        V.y = (rot[0][1] + rot[1][0]) / V.x * 0.25;
        V.z = (rot[0][2] + rot[2][0]) / V.x * 0.25;
    }
    else if (rot[1][1] > rot[2][2]) {
        V.y = sqrt(1.0 + rot[1][1] - rot[0][0] - rot[2][2]) * 0.5;
        S = (rot[0][2] - rot[2][0]) / V.y * 0.25;
        V.x = (rot[0][1] + rot[1][0]) / V.y * 0.25;
        V.z = (rot[1][2] + rot[2][1]) / V.y * 0.25;
    }
    else {
        V.z = sqrt(1.0 + rot[2][2] - rot[0][0] - rot[1][1]) * 0.5;
        S = (rot[1][0] - rot[0][1]) / V.z * 0.25;
        V.x = (rot[0][2] + rot[2][0]) / V.z * 0.25;
        V.y = (rot[1][2] + rot[2][1]) / V.z * 0.25;
    }
}
else {
    S = 0.5 * sqrt(1 + tmp);
    V.x = (rot[2][1]-rot[1][2]) / S * 0.25;
    V.y = (rot[0][2]-rot[2][0]) / S * 0.25;
    V.z = (rot[1][0]-rot[0][1]]) / mQ[VW] * 0.25;
}
```

To convert a quaternion to rotation matrix:

```cpp
mat3 m;
m.Identity();
double w = S;
double x = V.x;
double y = V.y;
double z = V.z;
m[0][0] = 1 - 2 * y * y - 2 * z * z;
m[0][1] = 2 * x * y - 2 * w * z;
m[0][2] = 2 * x * z + 2 * w * y;
m[1][0] = 2 * x * y + 2 * w * z;
m[1][1] = 1 - 2 * x * x - 2 * z * z;
m[1][2] = 2 * y * z - 2 * w * x;
m[2][0] = 2 * x * z - 2 * w * y;
m[2][1] = 2 * y * z + 2 * w * x;
m[2][2] = 1 - 2 * x * x - 2 * y * y;
return m;
```
