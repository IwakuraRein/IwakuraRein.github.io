---
layout: post
title: Computer Graphics Tips
categories:
- Cheat sheets
tags:
- Graphics
---
{% raw %}

## Pipeline

![](https://vulkan-tutorial.com/images/vulkan_simplified_pipeline.svg)

## Photometry

![](img/Photometry.png)

## Barycentric coordinates

![img](https://upload.wikimedia.org/wikipedia/commons/thumb/0/05/Barycentric_RGB.svg/220px-Barycentric_RGB.svg.png)

* <img src="http://latex.codecogs.com/svg.latex?G"> is the center of mass of a triangle <img src="http://latex.codecogs.com/svg.latex?\triangle ABC">. Then we can represent any coplanar point as <img src="http://latex.codecogs.com/svg.latex?u \overline{GA} + v \overline{GB} + w \overline{GC}"> where <img src="http://latex.codecogs.com/svg.latex?u + v + w = 1">.
* <img src="http://latex.codecogs.com/svg.latex?G = \frac{A+B+C}{3}">.
* A barycentric coordinate equals to the area the point forms with the edge it opposes divides by the area of the triangle.
  * <img src="http://latex.codecogs.com/svg.latex?u = \frac{\|GB \times GC\|}{\|AB \times AC\|}">, <img src="http://latex.codecogs.com/svg.latex?v = \frac{\|GA \times GC\|}{\|AB \times AC\|}">, <img src="http://latex.codecogs.com/svg.latex?w = \frac{\|GA \times GB\|}{\|AB \times AC\|}">.
* If none of the coordinates is negative, than the point is inside the triangle.
* Notice that when interpolating using barycentric coordinates in the screen space, we must consider the impact of depth values.
  * <img src="http://latex.codecogs.com/svg.latex?u \prime = \frac{Z_A}{Z} u">, <img src="http://latex.codecogs.com/svg.latex?v \prime = \frac{Z_B}{Z} v">, <img src="http://latex.codecogs.com/svg.latex?w \prime = \frac{Z_C}{Z} w">.

## Spherical Harmonics

![](img/Sphericalfunctions.svg.png)

Spherical Harmonics are a set of 2D basis functions <img src="http://latex.codecogs.com/svg.latex?B_i(\omega)"> defined on the **sphere**.

The functions on a sphere can be projected into Spherical Harmonics: <img src="http://latex.codecogs.com/svg.latex?f(x) \approx \sum l_i B_i (x)">, where <img src="http://latex.codecogs.com/svg.latex?l_i"> is the coefficent and <img src="http://latex.codecogs.com/svg.latex?B_i(x)"> is the SH.

Higher the degree of SH, higher the frequncy of the information it can encode.

A rotation R about the origin that sends the unit vector r to r'. Under this operation, a spherical harmonic of degree l and order m transforms into a linear combination of spherical harmonics of the same degree.

The integral of the multiplication of two functions is the dot production of the two corresponding SH coefficients.

### Precompute Radiance Transfer (PRT)

For static scenes, we can project the BRDF, the Visibility, and Lambertian to SH. Since Lighting can also be projected to SH, we can render this scene in **different lighting conditions** in real-time (solve the Rendering Equation with a simple dot production).

Notice the light are allowed to rotate due to the rotational behavior of the spherical harmonics (see above).

For glossy material, the BRDF also need to be projected to SH. In this case, the number of coefficents needed for the scene is squared (a matrix). Then the shading is a vector-matrix multiplication instead of a dot production.

## MSAA

![](https://pic3.zhimg.com/80/v2-c27b1744269105ae5c60a879f463cf66_720w.webp)

After enabling MSAA N, the cost for frament shading remains unchanged; the cost for rasterizaion is N times more; the size of fragement buffer, depth buffer and stencil buffer is N times more.

When creating the frame buffers, graphics API needs to know wether MSAA is enabled or not. E.g., Vulkan's [`VkImageCreateInfo`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VkImageCreateInfo.html) requires a [`VkSampleCountFlagBits`](https://registry.khronos.org/vulkan/specs/1.3-extensions/man/html/VkSampleCountFlagBits.html) bit.

Is MSAA compatible with Early-Z Testing? (my assumption: after enable MSAA the cost for fragment shading with Early-Z testing will increase)

## Normal Map

![](https://learnopengl.com/img/advanced-lighting/normal_mapping_tbn_vectors.png)

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
- Beckman: <img src="http://latex.codecogs.com/svg.latex?\frac {\exp \left (-\frac {\tan ^2 \theta_h}{\alpha ^ 2} \right )}{\pi \alpha^2 \cos ^4 \theta_h}">

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

## Summed Area Table (SAT)

![](img/sat.png)

Used for approximating depth of field or glossy reflection.

We can do one inclusive scan for row and one inclusive scan for column to get SAT.

## Image Based Lighting (IBL)

Based on Cook-Torrance, use <img src="http://latex.codecogs.com/svg.latex?\mathrm{Irridiance} \cdot \int k_d f_{diffuse} \cos \theta _i \mathrm{d}\omega _i + \int k_s L_i f_{specular} \cos \theta _i \mathrm{d}\omega _i"> to approximate <img src="http://latex.codecogs.com/svg.latex?\int L_i \cdot f_r \cos \theta _i \mathrm{d}\omega _i">.

Apparently the diffuse term is <img src="http://latex.codecogs.com/svg.latex?\frac{\mathrm{Irridiance} \cdot c}{\pi}">. We can pre-bake the irridiance map:

![](img/tex_irradiance_cube.png)

To querry the map, sample along the reflection angle.

Then, use <img src="http://latex.codecogs.com/svg.latex?\mathrm{Radiance} \int f_{specular} \cos \theta _i \mathrm{d}\omega _i"> to approximate the specular term (<img src="http://latex.codecogs.com/svg.latex?k_s"> is 1).

Pre-baked radiance mipmap:

![](img/radiance_map.png)
  - Left: 0 roughness
  - Right: 1 roughness

Pre-baked BRDF LUT:

![](img/lut.png)

Since Fresnel term is subjected to view angle (Schlick's approximation), the BRDF LUT has a roughness dimension and a <img src="http://latex.codecogs.com/svg.latex?\cos \theta_v"> direction.

Codes:

```c
vec3 getIBLContribution(PBRInfo pbrInputs, vec3 n, vec3 reflection)
{
	float lod = (pbrInputs.perceptualRoughness * uboParams.prefilteredCubeMipLevels);
	vec3 brdf = (texture(samplerBRDFLUT, vec2(pbrInputs.NdotV, 1.0 - pbrInputs.perceptualRoughness))).rgb;
	vec3 diffuseLight = SRGBtoLINEAR(tonemap(texture(samplerIrradiance, n))).rgb;
	vec3 specularLight = SRGBtoLINEAR(tonemap(textureLod(prefilteredMap, reflection, lod))).rgb;
	vec3 diffuse = diffuseLight * pbrInputs.diffuseColor;
	vec3 specular = specularLight * (pbrInputs.specularColor * brdf.x + brdf.y);
	return diffuse + specular;
}
```

## Intersection

### Ray-AABB Intersection

* The ray equation is: <img src="http://latex.codecogs.com/svg.latex?R(t) = \begin{pmatrix} x_o \\ y_o \\ z_o \end{pmatrix} + t \begin{pmatrix} x_{dir} \\ y_{dir} \\ z_{dir} \end{pmatrix}">.
* The minimum and maximum of the AABB is <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} x_{min} \\ y_{min} \\ z_{min} \end{pmatrix}"> and <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} x_{max} \\ y_{max} \\ z_{max} \end{pmatrix}">.
* We can calculate out the <img src="http://latex.codecogs.com/svg.latex?t"> by which the ray intersects with the AABB's minimum/maximum plane. E.g., <img src="http://latex.codecogs.com/svg.latex?t_{x \_ min} = \frac{x_{min}-x_o}{x_{dir}}">.
* The ray intersects with the AABB only when the maximum of <img src="http://latex.codecogs.com/svg.latex?t_{min}"> is less than the minimum of <img src="http://latex.codecogs.com/svg.latex?t_{max}">.

### Ray-Box Intersection

* Transform the ray into the box's local space. Then perform the Ray-AABB intersection test.

### Ray-Sphere Intersection

* Connect the ray origin <img src="http://latex.codecogs.com/svg.latex?O"> and the center <img src="http://latex.codecogs.com/svg.latex?C">. Use dot production and cross production to compute the length of cathetuses.
* Use Pythagorean theory to compute the <img src="http://latex.codecogs.com/svg.latex?t">.

### Ray-Plane Intersection

* <img src="http://latex.codecogs.com/svg.latex?N"> is the normal and <img src="http://latex.codecogs.com/svg.latex?P_0"> is a point on the plane. <img src="http://latex.codecogs.com/svg.latex?P"> is the intersection point. Then <img src="http://latex.codecogs.com/svg.latex?N \cdot P P_0 = 0">
* The ray equation is: <img src="http://latex.codecogs.com/svg.latex?R = O + D * t">. Then <img src="http://latex.codecogs.com/svg.latex?(O + D * t_0 - P_0) \cdot N = 0">.
* Due to distributive law, <img src="http://latex.codecogs.com/svg.latex?D \cdot N * t_0 + (O - P_0) \cdot N = 0">.
* <img src="http://latex.codecogs.com/svg.latex?t_0 = \frac{(P_0 - O) \cdot N }{D \cdot N}">.
* The ray intersects with the plane when <img src="http://latex.codecogs.com/svg.latex?t_0"> is not negtive.

### Ray-Triangle Intersection

* First find the intersection point between the ray and the plane of the triangle. Then determin if the point is inside the triangle. 

### Ray-Disk Intersection

* First find the intersection point between the ray and the plane of the disc. Then determin if the point is inside the disc. 

## Early-Z Testing

With Early-Z Testing, the depth test is performed before the fragment shader and the fragments that fail to pass the test will be discarded. With this we can reduce the cost of fragment shading.

The Directx 12 and Vulkan supports this feature by default. To enable it on OpenGL, see [OpenGL's wiki](https://www.khronos.org/opengl/wiki/Early_Fragment_Test#Explicit_specification).

### Depth Prepass

Also we can add an explicit depth prepass.

## Surface Area Heuristic

![](https://pbr-book.org/3ed-2018/Primitives_and_Intersection_Acceleration/BVH%20split%20bucketing.svg)

An effective algorithm to partition primitives and build Bounding Volume Hiearchy (BVH).

When computing the bounding volume for each of the primitives, we keep track of the largest distance between the centroids of two bounding volumes. Then, we perform the Surface Area Heuristic algorithm to determine the axis at which the largest distance occurs. 

We divide the distance range into multiple buckets (e.g., 32). Then, we attempt to partition at each boundary of the bucket and select the boundary with the lowest cost. The cost is calculated as the ratio between the two surface areas of the combined bounding volumes after the partition.

{% endraw %}