---
layout: post
title: GLM
categories:
- Cheat sheets
tags:
- Graphics
---
{% raw %}

**[OpenGL Mathematics (GLM)](https://github.com/g-truc/glm)** is a header only C++ mathematics library for graphics software based on the OpenGL Shading Language (GLSL) specifications. GLM provides classes and functions designed and implemented with the same naming conventions and functionality than GLSL so that anyone who knows GLSL, can use GLM as well in C++.

## Data Types

### Vectors

* `glm::vec3{ }`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(0,0,0)}">

* `glm::vec3{ 1.f }`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1,1,1)}">

* `glm::vec3{ 1.f, 2.f, 3.f }`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1,2,3)}">

* `glm::vec4{ glm::vec3{ 1.f, 2.f, 3.f }, 4.f }`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1,2,3,4)}">

### Matrices

GLM uses column major ordering by default.

* `glm::mat3{ 0.f }`
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} 0 & 0 & 0 \\ 0 & 0 & 0 \\ 0 & 0 & 0 \\ \end{pmatrix}">

* `glm::mat3{ 1.f }`
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} 1 & 0 & 0 \\ 0 & 1 & 0 \\ 0 & 0 & 1 \\ \end{pmatrix}">

* ```cpp
  glm::mat3{{1.f, 2.f, 3.f},
            {4.f, 5.f, 6.f},
            {7.f, 8.f, 9.f}}
  ```
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} 1 & 4 & 7 \\ 2 & 5 & 8 \\ 3 & 6 & 9 \\ \end{pmatrix}">

* ```cpp
  glm::mat3{glm::vec3{1.f},
            glm::vec3{2.f},
            {3.f, 3.f, 3.f}}
  ```
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} 1 & 2 & 3 \\ 1 & 2 & 3 \\ 1 & 2 & 3 \\ \end{pmatrix}">

* ```cpp
  glm::mat2x3{{1.f, 2.f, 3.f},
              {4.f, 5.f, 6.f}};
  ```
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix} 1 & 4 \\ 2 & 5 \\ 3 & 6 \\ \end{pmatrix}">

* `glm::mat4{ glm::mat3{ 2.f } }`
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{pmatrix}2 \cdot I & \begin{matrix} 0\\0\\0 \end{matrix} \\ \begin{matrix} 0 & 0 & 0 \end{matrix} & 1 \end{pmatrix}">

### Quaternions

TODO

## Calculations

```cpp
#include <glm/glm.hpp>

glm::vec3 v1{ 1.f, 2.f, 3.f };
glm::vec3 v2{ 4.f, 5.f, 6.f };
glm::mat3 m1{{1.f, 2.f, 3.f},
             {4.f, 5.f, 6.f},
             {7.f, 8.f, 9.f}};
glm::mat3 m2{{1.f, 1.f, 1.f},
             {2.f, 2.f, 2.f},
             {3.f, 3.f, 3.f} };
glm::mat3 m3{{3.f, 3.f, 3.f},
             {2.f, 2.f, 2.f},
             {1.f, 1.f, 1.f}};
```
* `2.f * v1`
  
  <img src="http://latex.codecogs.com/svg.latex?2 \cdot \overrightarrow{(1,2,3)} = \overrightarrow{(2,4,6)}">

* `v1 * v2`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1\cdot4,2\cdot5,3\cdot6)} = \overrightarrow{(4,10,18)}">

* `glm::dot(v1, v2)`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1,2,3)} \cdot \overrightarrow{(4,5,6)} = 32">

* `glm::cross(v1, v2)`
  
  <img src="http://latex.codecogs.com/svg.latex?\overrightarrow{(1,2,3)} \times \overrightarrow{(4,5,6)} = \overrightarrow{(-3,6,-3)}">

* ` m1 * m2`
  
  <img src="http://latex.codecogs.com/svg.latex?M_1 \times M_2 = \begin{pmatrix} 12 & 24 & 36 \\ 15 & 30 & 45 \\ 18 & 36 & 54 \\ \end{pmatrix}">

* `v1 * m1`
  
  <img src="http://latex.codecogs.com/svg.latex?V_1 \times M_1 = \begin{pmatrix} 14 & 32 & 50 \end{pmatrix}">

* `m1 * v1`
  
  <img src="http://latex.codecogs.com/svg.latex?M_1 \times V_1 = \begin{pmatrix} 30 \\ 36 \\ 42 \end{pmatrix}">
  
* `m1 * m2 * m3`
  
  <img src="http://latex.codecogs.com/svg.latex?\begin{align*}M_1 \times \left ( M_2 \times M_3 \right ) & = M_1 \times \begin{pmatrix} 18& 12& 6\\ 18& 12& 6\\ 18& 12& 6\end{pmatrix}\\ & = \begin{pmatrix} 216 & 144 & 72 \\ 270 & 180 & 90 \\ 324 & 216 & 108 \end{pmatrix}\end{align*}">

## Functions

### Basic

* `glm::radians(x)`

* `glm::degrees(x)`

* `glm::inverse(m)`


See [GLSL's functions](./glsl.md#functions).

### Matrix Transformation

* `mat4 glm::translate(mat4 m, vec3 pos)`

* `mat4 glm::rotate(mat4 m, vec3 angle, vec3 axis)`

* `mat4 glm::scale(mat4 m, vec3 scale)`

* `mat4 glm::ortho(float left, float right, float bottom, float top, float zNear, float zFar)`

* `mat4 glm::perspective(float fovy, float aspect, float zNear, float zFar)` 

### Type Cast

```cpp
#include <glm/glm.hpp>
#include <glm/gtc/type_ptr.hpp>

glm::vec3 v1{ 1.f, 2.f, 3.f };
glm::mat3 m1{{1.f, 2.f, 3.f},
             {4.f, 5.f, 6.f},
             {7.f, 8.f, 9.f}};
float* pV1 = glm::value_ptr(v1); // {1, 2, 3}
float* pM1 = glm::value_ptr(m1); // {1, 2, 3, 4, 5, 6, 7, 8, 9}

float pV2[3]{3.f, 2.f, 1.f};
float pM2[9]{ 3.f, 3.f, 3.f, 2.f, 2.f, 2.f, 1.f, 1.f, 1.f };
glm::vec3 v2{glm::make_vec3(pV2)}; // {3, 2, 1}
glm::mat3 m2{glm::make_mat3(pM2)};
//3  3  3
//2  2  2
//1  1  1
```
{% endraw %}