---
layout: post
title: Procedual Content Generation Tips
categories:
- Cheat sheets
tags:
- Graphics
---
{% raw %}

## L-system

TODO

## Wave Function Collapse

![](https://raw.githubusercontent.com/mxgmn/WaveFunctionCollapse/master/images/wfc.gif)

- Gridify the space. Populate it with the initial values.
- Iterate the empty cells and record the possible values that can be put in each. The number of the viable values for a grid is defined as its entropy.
- Iterate the grid with the minimum postive entropies and select one value to fill in.
- Keep looping until all entropy is non-postive.

## Noise

See also: [GLSL Noise Algorithms](https://gist.github.com/patriciogonzalezvivo/670c22f3966e662d2f83).

### Useful Fade Function

Rather than just using linear interpolation, we can use a fade function.

- [Easing functions Cheat Sheet](https://easings.net/)
- [Bias and Gain](http://demofox.org/biasgain.html)

### Useful Random Functions

- [float, small and random - 2005](https://iquilezles.org/articles/sfrand/)
- [PCG, A Family of Better Random Number Generators](https://www.pcg-random.org/#)
- [Hash Without Sine](https://www.shadertoy.com/view/4djSRW)

### [Perlin Noise](https://www.shadertoy.com/view/Md3SzB)

![](img/noise.jpg)

1. Given a point in 2D space, find its 4 surrounding lattice points.
2. Assign gradient for each lattice point with random funcion.
3. Calculate distance vector for each lattice point.
4. Calculate influence vector for each lattice point: `influence_vector = dot(gradient_vector, distance_vector)`.
5. Interpolate between all the influence vectors to get final value.

### [Worley Noise](https://www.shadertoy.com/view/MstGRl)

Also known as **Voronoi Noise**.

1. Divide the space into grids.
2. Give a point in space, find its nearest N grids and generate a random position ("cell") inside each grid.
3. Calculate the minimum distance between the sample and its surrounding cells.
4. Normalize the distance by dividing it with the longest possible distance.


### Multi-Octave Noise

Also known as **Fractional Brownian Motion (FBM)**

In practice, we can combine sevral noise with different frequency together.

Octave contributions are modulated using:
- frequency: sample rate.
- persistance: decay of amplitude as frequency increases.

```C
PerlinNoise2d(float x, float y) {
  float total = 0;
  float persisstence = 1 / 2.0f;

  for (int i = 0; i < N_OCTAVES; ++i) {
    float frequency = pow(2, i);
    float amplitude = pow(persistence, i);
    total += amplitude * sampleNoisei(x * frequency, y * frequency);
  }
  return total;
}
```

## Implicit Surface

### Approximate Normal

<img src="http://latex.codecogs.com/svg.latex?\vec{n} = \mathrm{normalize}\left (\begin{bmatrix} f(x+\varepsilon , y, z)-f(x-\varepsilon , y, z)\\ f(x, y+\varepsilon, z)-f(x, y, z-\varepsilon) \\ f(x, y, z+\varepsilon)-f(x, y, z-\varepsilon) \\ \end{bmatrix} \right )">

<img src="http://latex.codecogs.com/svg.latex?f"> is the implicit surface.

### [Signed Distance Function](https://iquilezles.org/articles/distfunctions/)

TODO

### [March Cubes](https://jamie-wong.com/2014/08/19/metaballs-and-marching-squares/)

Divide the space into uniform grids. Sample each grid corner with the implicit surface.

![img](https://jamie-wong.com/images/14-08-11/lerp-labels.png)

For example, <img src="http://latex.codecogs.com/svg.latex?A">, <img src="http://latex.codecogs.com/svg.latex?B">, <img src="http://latex.codecogs.com/svg.latex?C"> is outside, <img src="http://latex.codecogs.com/svg.latex?D"> is inside, and <img src="http://latex.codecogs.com/svg.latex?P">, <img src="http://latex.codecogs.com/svg.latex?Q"> is on the boundary (<img src="http://latex.codecogs.com/svg.latex?f(P) \approx f(Q) \approx 1">). 

<img src="http://latex.codecogs.com/svg.latex?\left\{\begin{matrix} Q_x & = &B_x \\ \frac{Q_y-B_y}{D_y-B_y} & \approx & \frac{f\left(Q_x, Q_y\right)-f\left(B_x, B_y\right)}{f\left(D_x, D_y\right)-f\left(B_x, B_y\right)} \\ f(Q_x, Q_y) & \approx & 1 \\ \end{matrix}\right.">

<img src="http://latex.codecogs.com/svg.latex?Q_y=B_y+\left(D_y-B_y\right)\left(\frac{1-f\left(B_x, B_y\right)}{f\left(D_x, D_y\right)-f\left(B_x, B_y\right)}\right)">

{% endraw %}