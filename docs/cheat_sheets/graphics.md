---
layout: post
title: Computer Graphics Tips
categories:
- Cheat sheets
tags:
- Graphics
---
{% raw %}

## Barycentric coordinates

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Barycentric_RGB.svg/220px-Barycentric_RGB.svg.png)

* <img src="http://latex.codecogs.com/svg.latex?G"> is the center of mass of a triangle <img src="http://latex.codecogs.com/svg.latex?\triangle ABC">. Then we can represent any coplanar point as <img src="http://latex.codecogs.com/svg.latex?u \overline{GA} + v \overline{GB} + w \overline{GC}"> where <img src="http://latex.codecogs.com/svg.latex?u + v + w = 1">.
* <img src="http://latex.codecogs.com/svg.latex?G = \frac{A+B+C}{3}">.
* A barycentric coordinate equals to the area the point forms with the edge it opposes divides by the area of the triangle.
  * <img src="http://latex.codecogs.com/svg.latex?u = \frac{\|GB \times GC\|}{\|AB \times AC\|}">, <img src="http://latex.codecogs.com/svg.latex?v = \frac{\|GA \times GC\|}{\|AB \times AC\|}">, <img src="http://latex.codecogs.com/svg.latex?w = \frac{\|GA \times GB\|}{\|AB \times AC\|}">.
* If none of the coordinates is negative, than the point is inside the triangle.
* Notice that when interpolating using barycentric coordinates in the screen space, we must consider the impact of depth values.
  * <img src="http://latex.codecogs.com/svg.latex?u \prime = \frac{Z_A}{Z} u">, <img src="http://latex.codecogs.com/svg.latex?v \prime = \frac{Z_B}{Z} v">, <img src="http://latex.codecogs.com/svg.latex?w \prime = \frac{Z_C}{Z} w">.

## Normal Map

![img](https://learnopengl.com/img/advanced-lighting/normal_mapping_tbn_vectors.png)

* The values stored in the normal map are in the tangent space, between 0 and 1. It uses two channels for bitangent value and tangent values (the normal value can be retrieved based on the way how we do normalization):
  * ```cpp
    float3 e1 = v1 - v0;
    float3 e2 = v2 - v0;
    float3 dUV1 = uv1 - uv0;
    float3 dUV2 = uv2 - uv0;
    float f = 1.0f / (dUV1.x * dUV2.y - dUV2.x * dUV1.y);
    float3 t = (e1 * dUV2.y - e2 * dUV1.y) * f;
    float3 b = (e2 * dUV1.x - e1 * dUV2.x) * f;
    float3 n = normalize(cross(t, b));
    ```
* When shading, the tangent and bitangent vectors are calculated from the UV coordinates (the tangent aligns with the texture's x-axis and the bitangent aligns with the texture's y-axis). The normal vector retrieved by cross producting the tangent and bitangent. 
* With tangent, bitangent, and normal vectors, we can construct a TBN matrix. Then read the normal values from the normal map. Then we can use the TBN matrix to transform it into world space.
* Notice that the normal map texture must be in linear space.

## Cook-Torrance BRDF

<img src="http://latex.codecogs.com/svg.latex?\begin{aligned} L_O(p, \omega_o) & = \int_\Omega(k_d f_{diffuse} + k_s f_{specular})L_i(p, \omega_i) n \cdot \omega_i \mathrm{d}x \\ f_{specular} & = \frac{DFG}{4(\omega_o \cdot n)(\omega_i \cdot n)} \end{aligned}">

where <img src="http://latex.codecogs.com/svg.latex?D"> is Normal Distrubution Function, <img src="http://latex.codecogs.com/svg.latex?F"> is Fresnel Term, and <img src="http://latex.codecogs.com/svg.latex?G"> is Geometry Term.

### Blending

- Metallic Workflow:<img src="http://latex.codecogs.com/svg.latex?\begin{aligned}k_d & = (1-\mathrm{metallic})(1-F) \\ k_s & = 1\end{aligned}">
- Specular Workflow: TODO

### Diffuse Term

- Lambertian: <img src="http://latex.codecogs.com/svg.latex?\frac{c}{\pi}">
- Oren-Nayar: TODO

### Normal Distribution Function

Describe the smoothness of the surface. The smoother the surface is, the more the lobe concentrates on the reflection direction.

- GGX: <img src="http://latex.codecogs.com/svg.latex?\frac{\alpha ^2}{\pi ((n \cdot h)^2(\alpha^2-1)+1)^2}">
  - <img src="http://latex.codecogs.com/svg.latex?\alpha"> is roughness.
  - <img src="http://latex.codecogs.com/svg.latex?h"> is <img src="http://latex.codecogs.com/svg.latex?\frac{l + v}{\Vert l+v \Vert}">.
- Beckman: TODO

### Geometry Term

Describe the impact of self occlusion and shadowing.

- Schlick-GGX: TODO

### Fresnel Term

Describe the metallic of the material. The closer to the metal, the more the lobe conentratese on the gazing angle.

- Schlick: <img src="http://latex.codecogs.com/svg.latex?R + (1-R)(1-h \cdot v)^5">
  - <img src="http://latex.codecogs.com/svg.latex?R"> is the reflection coefficient.
- Fresnel Equation: TODO

## BTDF

TODO

## Ray-AABB Intersection

* The ray equation is: <img src="http://latex.codecogs.com/svg.latex?R(t) = \begin{pmatrix} x_o \\ y_o \\ z_o \end{pmatrix} + t \begin{pmatrix} x_{dir} \\ y_{dir} \\ z_{dir} \end{pmatrix}">.
* The minimum and maximum of the AABB is <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} x_{min} \\ y_{min} \\ z_{min} \end{pmatrix}"> and <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} x_{max} \\ y_{max} \\ z_{max} \end{pmatrix}">.
* We can calculate out the <img src="http://latex.codecogs.com/svg.latex?t"> by which the ray intersects with the AABB's minimum/maximum plane. E.g., <img src="http://latex.codecogs.com/svg.latex?t_{x \_ min} = \frac{x_{min}-x_o}{x_{dir}}">.
* The ray intersects with the AABB only when the maximum of <img src="http://latex.codecogs.com/svg.latex?t_{min}"> is less than the minimum of <img src="http://latex.codecogs.com/svg.latex?t_{max}">.

## Ray-Box Intersection

* Transform the ray into the box's local space. Then perform the Ray-AABB intersection test.

## Ray-Sphere Intersection

* Connect the ray origin <img src="http://latex.codecogs.com/svg.latex?O"> and the center <img src="http://latex.codecogs.com/svg.latex?C">. Use dot production and cross production to compute the length of cathetuses.
* Use Pythagorean theory to compute the <img src="http://latex.codecogs.com/svg.latex?t">.

## Ray-Plane Intersection

* <img src="http://latex.codecogs.com/svg.latex?N"> is the normal and <img src="http://latex.codecogs.com/svg.latex?P_0"> is a point on the plane. <img src="http://latex.codecogs.com/svg.latex?P"> is the intersection point. Then <img src="http://latex.codecogs.com/svg.latex?N \cdot P P_0 = 0">
* The ray equation is: <img src="http://latex.codecogs.com/svg.latex?R = O + D * t">. Then <img src="http://latex.codecogs.com/svg.latex?(O + D * t_0 - P_0) \cdot N = 0">.
* Due to distributive law, <img src="http://latex.codecogs.com/svg.latex?D \cdot N * t_0 + (O - P_0) \cdot N = 0">.
* <img src="http://latex.codecogs.com/svg.latex?t_0 = \frac{(P_0 - O) \cdot N }{D \cdot N}">.
* The ray intersects with the plane when <img src="http://latex.codecogs.com/svg.latex?t_0"> is not negtive.

## Ray-Triangle Intersection

* First find the intersection point between the ray and the plane of the triangle. Then determin if the point is inside the triangle. 

## Ray-Disk Intersection

* First find the intersection point between the ray and the plane of the disc. Then determin if the point is inside the disc. 

## Early-Z Testing

* Without Early-Z Testing, the depth test is performed after the fragment shader. With this we can access the depth buffer in the fragment shader.
* With Early-Z Testing, the depth test is performed before the fragment shader. With this we can reduce the cost of fragment shading.

{% endraw %}