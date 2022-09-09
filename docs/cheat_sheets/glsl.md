---
layout: post
title: GLSL
categories:
- Cheat sheets
tags:
- Graphics
---

{% raw %}

The **OpenGL Shading Language (GLSL)** is the principal shading language for *OpenGL* and *Vulkan*.

## Index

In GLSL, you can index a vector with eithr xyzw, stpq, or rgba. They all mean exactly the same thing.

## Data Types

* `vecN`, `bvecN`, `ivecN`, `uvecN`, `dvecN`

* `mat2`, `mat3`, `mat4`, `dmat2`, `dmat3`, `dmat4`

  Matrices are column-major. See [glm](./glm.md##Data_Types) for more information.

* `sampler2D`, `samplerCube`

## Functions

* `abs(x)`

* `sign(x)`

* `floor(x)`

* `ceil(X)`

* `fract(x)`

* `mod(x,y)`

* `min(x.y)`

* `max(x.y)`

* `clamp(x,minVar,maxVar)`

* `mix(x,y,a)`
  
  <img src="http://latex.codecogs.com/svg.latex?x \cdot (1-a) + y\cdot (1-a)">

* `step(edge,x)`
  
  <img src="http://latex.codecogs.com/svg.latex?\left\{\begin{matrix} 0 & x \leqslant edge \\ 1 & x \geqslant edge \\ x & \mathrm{otherwise} \\ \end{matrix}\right.">

* `smoothstep(edge0,edge1,x)`
  
  <img src="http://latex.codecogs.com/svg.latex?\left\{\begin{matrix} 0 & x \leqslant edge_0 \\ 1 & x \geqslant edge_1 \\ x & \mathrm{otherwise} \\ \end{matrix}\right.">

* `radians(x)`

* `degress(x)`

* `sin(x)`

* `cos(x)`

* `tan(x)`

* `asin(x)`

* `acos(x)`

* `atan(x)`

* `pow(x,y)`

* `exp(x)`

* `exp2(x)`
  
  <img src="http://latex.codecogs.com/svg.latex?2^x">

* `log(x)`
  
  <img src="http://latex.codecogs.com/svg.latex?\ln{x}">

* `log2(x)`
  
  <img src="http://latex.codecogs.com/svg.latex?\log_2{x}">

* `sqrt(x)`

* `inversesqrt(x)`
  
  <img src="http://latex.codecogs.com/svg.latex?\frac{1}{\sqrt{x}}">

* `length(x)`

* `distance(x,y)`

* `dot(x,y)`

* `cross(x,y)`

* `normalize(x)`

* `reflect(x,N)`
  
  For the incident vector I and surface orientation *N*, returns the reflection direction. *N* must already be normalized in order to
achieve the desired result.
  
  <img src="http://latex.codecogs.com/svg.latex?x-2 \cdot N \cdot \operatorname{dot} \left (N, x \right )">

* `faceforward(N,x,y)`
  
  <img src="http://latex.codecogs.com/svg.latex?\left\{\begin{matrix} N & \operatorname{dot} \left (x, y \right ) < 0 \\ -N & \operatorname{otherwise} \\ \end{matrix}\right.">

* `refract(x,N,eta)`
  
  For the incident vector I and surface normal *N*, and the ratio of indices of refraction eta, return the refraction vector. *I* and *N* must already be normalized to get the desired results.
  
  <img src="http://latex.codecogs.com/svg.latex?k=1.0-eta \cdot eta \cdot (1.0-\operatorname{dot}(N, I) \cdot \operatorname{dot}(N, I)) \\ result = \begin{cases} 0 & k<0 \\ eta * I-(eta * \operatorname{dot}(N, I)+\sqrt{k}) * N & \mathrm {otherwise}\end{cases}">

* `texture(sampler, coord)`

## Internal Variables

* `out vec4 gl_position`
  
  For writing the **homogeneous** vertex position. It can be written at any time during shader execution. This value will be used by primitive assembly, clipping, culling, and other fixed functionality operations, if present, that operate on primitives after vertex processing has occurred. Its value is undefined after the vertex processing stage if the vertex shader executable does not write `gl_Position`.

* `out float gl_PointSize`
  
  Intended for a shader to write the size of the point to be rasterized. It is measured in pixels. If `gl_PointSize` is not written to, its value is undefined in subsequent pipe stages. 

* `out float gl_ClipDistance`

  It allows the shader to set the distance from the vertex to each user-defined clipping half-space. A non-negative distance means that the vertex is inside/behind the clip plane, and a negative distance means it is outside/in front of the clip plane. Each element in the array is one clip plane. In order to use this variable, the user must manually redeclare it with an explicit size. With GLSL 4.10 or [ARB_separate_shader_objects](https://www.khronos.org/registry/OpenGL/extensions/ARB/ARB_separate_shader_objects.txt), the whole `gl_PerVertex` block needs to be redeclared. Otherwise just the gl_ClipDistance built-in needs to be redeclared.
  
* `in float gl_ClipDistance[]`
  
  Intended for writing clip distances, and provides the forward compatible mechanism for controlling user clipping. The element `gl_ClipDistance[i]` specifies a clip distance for each plane i. A distance of 0 means the vertex is on the plane, a positive distance means the vertex is inside the clip plane, and a negative distance means the point is outside the clip plane. The clip distances will be linearly interpolated across the primitive and the portion of the primitive with interpolated distances less than 0 will be clipped.
  
  The array is predeclared as unsized and must be explicitly sized by the shader either redeclaring it with a size or implicitly sized by indexing it only with constant integral expressions. This needs to size the array to include all the clip planes that are enabled via the API; if the size does not include all enabled planes, results are undefined. The size can be at most `gl_MaxClipDistances`. The number of varying components (see gl_MaxVaryingComponents) consumed by `gl_ClipDistance` will match the size of the array, no matter how many planes are enabled. The shader must also set all values in `gl_ClipDistance` that have been enabled via the API, or results are undefined. Values written into `gl_ClipDistance` for planes that are not enabled have no effect.

* `in float gl_CullDistance[]`
  
  Provide a mechanism for controlling user culling. The element `gl_CullDistance[i]` specifies a cull distance for plane i. A istance of 0 means the vertex is on the plane, a positive distance means the vertex is inside the cull volume, and a negative distance means the point is outside the cull volume. Primitives whose vertices all have a negative cull distance for plane i will be discarded.
  
  They array is predeclared as unsized and must be sized by the shader either redeclaring it with a size or indexing it only with constant integral expressions. The size determines the number and set of enabled cull distances and can be at most gl_MaxCullDistances. The number of varying components (see `gl_MaxVaryingComponents`) consumed by `gl_CullDistance` will match the size of the array. Shaders writing gl_CullDistance must write all enabled distances, or culling results are undefined.

  As an output variable, `gl_CullDistance` provides the place for the shader to write these distances. As an input in all but the fragment language, it reads the values written in the previous shader stage. In the fragment language, `gl_CullDistance` array contains linearly interpolated values for the vertex values written by a shader to the `gl_CullDistance` vertex output variable.

* `in int gl_VertexID` (*OpenGL*), `int gl_VertexIndex` (*Vulkan*)

* `in int gl_InstanceID` (*OpenGL*), `int gl_InstanceIndex` (*Vulkan*)

* `in vec4 gl_FragCoord`

  The variable `gl_FragCoord` is available as an input variable from within fragment shaders and it holds the window relative coordinates (*x*, *y*, *z*, 1/*w*) values for the fragment. If multi-sampling, this value can be for any location within the pixel, or one of the fragment samples. The use of **centroid** does not further restrict this value to be inside the current primitive. This value is the result of the fixed functionality that interpolates primitives after vertex processing to generate fragments. The *z* component is the depth value that would be used for the fragment’s depth if no shader contained any writes to gl_FragDepth. This is useful for invariance if a shader conditionally computes gl_FragDepth but otherwise wants the fixed functionality fragment depth.

* `in bool gl_FrontFacing`

  True if the fragment belongs to a front-facing primitive. One use of this is to emulate two-sided lighting by selecting one of two colors calculated by a vertex or geometry shader.

* `out float gl_FragDepth`

## Extensions

TODO

{% endraw %}