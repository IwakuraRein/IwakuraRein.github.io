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

<img src="http://latex.codecogs.com/svg.latex?S = \begin{pmatrix} S & 0 & 0 & 0 \\ 0 & S & 0 & 0 \\ 0 & 0 & S & 1\end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?T = \left ( \begin{array}{c|c} I & \vec{d} \\ \hline  0 & 1 \end{array} \right )">

<img src="http://latex.codecogs.com/svg.latex?R_x = \begin{pmatrix} 1 & 0 & 0 \\ 0 & \cos{\theta_x} & -\sin{\theta_x} \\ 0 & \sin{\theta_x} & \cos{\theta_x} \end{pmatrix} ">

<img src="http://latex.codecogs.com/svg.latex?R_y = \begin{pmatrix} \cos{\theta_y} & 0 & \sin{\theta_y} \\ 0 & 1 & 0 \\-\sin{\theta_y} & 0 & \cos{\theta_y} \end{pmatrix} ">

<img src="http://latex.codecogs.com/svg.latex?R_z = \begin{pmatrix} \cos{\theta_z} & -\sin{\theta_z} & 0 \\ \sin{\theta_z} & \cos{\theta_z} & 0 \\ 0 & 0 & 1 \end{pmatrix} ">

<img src="http://latex.codecogs.com/svg.latex?R_{yzx} = \begin{pmatrix} c_y c_z & s_y s_x - c_y c_x s_z & c_x s_y + c_y s_z s_x \\  s_z & c_z c_x & - c_z s_x \\ - c_z s_y & c_y s_x + c_x s_y s_z & c_y c_x - s_y s_z s_x \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?R_{xzy} = \begin{pmatrix} c_z c_y & - s_z & c_z s_y \\ s_x s_y + c_x c_y s_z & c_x c_z & c_x s_z s_y - c_y s_x \\ c_y s_x s_z - c_x s_y & c_z s_x & c_x c_y + s_x s_z s_y \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?R_{zyx} = \begin{pmatrix}  c_z c_y & c_z s_y s_x - c_x s_z & s_z s_x + c_z c_x s_y \\  c_y s_z & c_z c_x + s_z s_y s_x & c_x s_z s_y - c_z s_x \\  - s_y & c_y s_x & c_y c_x  \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?R_{zxy} = \begin{pmatrix} c_z c_y - s_z s_x s_y & - c_x s_z & c_z s_y + c_y s_z s_x \\ c_y s_z + c_z s_x s_y & c_z c_x & s_z s_y - c_z c_y s_x \\ - c_x s_y & s_x & c_x c_y \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?R_{xyz} = \begin{pmatrix} c_y c_z & - c_y s_z & s_y \\ c_x s_z + c_z s_x s_y & c_x c_z - s_x s_y s_z & - c_y s_x \\ s_x s_z - c_x c_z s_y & c_z s_x + c_x s_y s_z & c_x c_y \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?R_{yxz} = \begin{pmatrix} c_y c_z + s_y s_x s_z & c_z s_y s_x - c_y s_z & c_x s_y \\ c_x s_z & c_x c_z & - s_x \\ c_y s_x s_z - c_z s_y & c_y c_z s_x + s_y s_z & c_y c_x \end{pmatrix}">

<img src="http://latex.codecogs.com/svg.latex?H = S \times T \times R">

<img src="http://latex.codecogs.com/svg.latex?H^{-1} = \left ( \begin{array}{c|c} \frac{1}{S} \times R^T & -R^T \times \vec{d} \\ \hline  0 & 1 \end{array} \right )">

Global transformations are pre-multiply.

Local transformations are post-multiply.

To transform from right handed to left handed, simply reverse the z-axis component.

## Quaternion

<img src="http://latex.codecogs.com/svg.latex?\vec{q} = \binom{\cos{\frac{\theta}{2}}}{\sin{\frac{\theta}{2}} \cdot \hat{u}}">, where <img src="http://latex.codecogs.com/svg.latex?\theta"> is angle, <img src="http://latex.codecogs.com/svg.latex?\hat{u}"> is axis.

Denote <img src="http://latex.codecogs.com/svg.latex?\cos{\frac{\theta}{2}}"> by <img src="http://latex.codecogs.com/svg.latex?S">, <img src="http://latex.codecogs.com/svg.latex?\sin{\frac{\theta}{2}}\cdot \hat{u}"> by <img src="http://latex.codecogs.com/svg.latex?\vec{V}">:

<img src="http://latex.codecogs.com/svg.latex?\vec{q}_1 + \vec{q}_2 = \binom{S_1 + S_2}{\vec{V}_1 + \vec{V}_2}">

<img src="http://latex.codecogs.com/svg.latex?\vec{q}_1 \times \vec{q}_2 = \binom{S_1 S_2 - \vec{V}_1 \vec{V}_2}{S_1\vec{V}_2+S_2\vec{V}_1+\vec{V}_1\times\vec{V}_2}">

<img src="http://latex.codecogs.com/svg.latex?\left\| \vec{q} \right\| = \sqrt{S^2 + \left\| \vec{V} \right\| ^2}">

<img src="http://latex.codecogs.com/svg.latex?\vec{q}^{-1} = \frac{1}{\left\| \vec{q} \right\|^2} \binom{S}{-\vec{V}}">

To rotate a vector <img src="http://latex.codecogs.com/svg.latex?\vec{p}"> by quaternion <img src="http://latex.codecogs.com/svg.latex?\vec{q}">: 

<img src="http://latex.codecogs.com/svg.latex?\vec{q} \cdot \binom{0}{\vec{p}} \cdot \vec{q}^{-1}">

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

### Interpolation

**SDouble**

$(2\cdot q_1 \cdot q_2)\cdot q2 - q_1$

**SBisect**

$\frac{q_1+q_2}{\left \| q_1+q_2 \right \|}$

**Slerp**

```cpp
quat o;
quat p = q0, q = q1;
double cosine = Dot(p, q);
if (cosine == 1) {
    o = (1 - u) * p + u * q;
    return o.Normalize();
}
if (cosine < 0) {
    p = -p;
    cosine = -cosine;
}
double sine = sqrt(1 - cosine * cosine);
double theta = acos(cosine);
o = (sin((1 - u) * theta) * p + sin(u * theta) * q) / sine;
return o.Normalize();
```

{% endraw %}
