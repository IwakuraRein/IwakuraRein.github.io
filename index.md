---
layout: default
---

# Welcome

## About me

I am currently a Master student at [University of Pennsylvania](http://cg.cis.upenn.edu/). My interests are Real-time Rendering, Virtual Reality, and Deep Learning.

## Selected Projects

### <a href="https://github.com/IwakuraRein/Nagi" target="_blank">Nagi</a>

Nagi is a toy path tracer developed in CUDA.

<img src="./docs/projects/imgs/nagi.png" alt="Nagi screenshot" />

Artist: <a href="https://blendswap.com/profile/35454" target="_blank">NewSee2l035</a> 

### <a href="https://github.com/IwakuraRein/Naku" target="_blank">Naku</a>

Naku is a toy rasterization-based renderer developed in Vulkan and C++.

<video src="https://user-images.githubusercontent.com/28486541/200195281-3906004f-3220-4657-b108-eec6fa4fe30d.mp4" data-canonical-src="https://user-images.githubusercontent.com/28486541/200195281-3906004f-3220-4657-b108-eec6fa4fe30d.mp4" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-width:95%;" draggable="false" autoplay="autoplay" loop="loop"></video>

<p></p>

### CIS 565 Course Projects

<table style="width:95%">
    <tr>
        <th><a href="https://github.com/IwakuraRein/CIS-565-1-CUDA-Flocking" target="_blank">Boids Flocking Simulation with CUDA</a></th>
        <th><a href="https://github.com/IwakuraRein/CIS-565-5-Vulkan-Grass-Rendering" target="_blank">Grass Rendering with Vulkan</a></th>
    </tr>
    <tr>
        <th><a href="https://github.com/IwakuraRein/CIS-565-1-CUDA-Flocking" target="_blank"><img src="./docs/projects/imgs/2.1-50000.gif" alt="Boid Flocking"/></a></th>
        <th><a href="https://github.com/IwakuraRein/CIS-565-5-Vulkan-Grass-Rendering" target="_blank"><img src="./docs/projects/imgs/my_grass.gif" alt="Grass Rendering"/></a></th>
    </tr>
    <tr>
        <th><a href="https://github.com/IwakuraRein/CIS-565-4-CUDA-Denoiser" target="_blank">Denoising Path Tracing with CUDA</a></th>
        <th><a href="https://github.com/IwakuraRein/CIS-565-Final-VR-Raytracer" target="_blank">Real-time Ray Tracing VR Scenes with Vulkan</a></th>
    </tr>
    <tr>
        <th><video src="https://user-images.githubusercontent.com/28486541/196747599-32b3307a-4af8-43af-bf47-4a27321f0234.mp4" data-canonical-src="https://user-images.githubusercontent.com/28486541/196747599-32b3307a-4af8-43af-bf47-4a27321f0234.mp4" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-width:95%;" autoplay="autoplay" draggable="false" loop="loop"></video></th>
        <th>
            <p>In progress. Plan to introduce:</p>
            <ul>
                <li><a href="https://benedikt-bitterli.me/restir/" target="_blank">ReSTIR</a></li>
                <li><a href="https://www.youtube.com/watch?v=2GYXuM10riw" target="_blank">Ridiance Cache</a></li>
                <li><a href="https://research.adobe.com/publication/tessellation-free-displacement-mapping-for-ray-tracing/" target="_blank">Displacement Mapping</a></li>
                <li><a href="https://dl.acm.org/doi/10.5555/3151666.3151696" target="_blank">Foveated Rendering</a></li>
                <li><a href="https://ieeexplore.ieee.org/document/9089460" target="_blank">Stereo Rendering</a></li>
            </ul>
        </th>
    </tr>

</table>

These are the course projects of <a href="https://cis565-fall-2022.github.io/" target="_blank">CIS 565 - GPU Programming and Architecture</a>. In this course, I will delve into GPU architecture and learn about CUDA, WebGL, and Vulkan. Its six non-trivial projects will further develop my C++ programming skills.

### Generating Anime Avatars

<div style="text-align: center;">
<img src="./assets/img/avaters/Avater0.png"/>
</div>

I created a anime face dataset and trained a <a href="https://github.com/IwakuraRein/FastGAN-pytorch"  target="_blank">FastGan</a> model to generate the avater on the left. Refresh to see more.

### Teleport

<!--<video src="https://user-images.githubusercontent.com/28486541/199053796-11756267-042a-4419-823d-a4d8bf4ac0e7.mp4"></video>-->

<!--<img src="./docs/projects/imgs/teleport_screenshot1.png" alt="Teleport Screenshot"/>-->

<video src="https://user-images.githubusercontent.com/28486541/199053796-11756267-042a-4419-823d-a4d8bf4ac0e7.mp4" data-canonical-src="https://user-images.githubusercontent.com/28486541/199053796-11756267-042a-4419-823d-a4d8bf4ac0e7.mp4" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-width:95%;" autoplay="autoplay" draggable="false" loop="loop"></video>

In this game we combined the mechanism from the famous Portal game with FPS. Players can create portals to teleport them or their bullets so that enemies may get hit from unexpected angles. This is the final project for the Game Design Course. 

### Dog Fight

<!--<video src="https://user-images.githubusercontent.com/28486541/199054465-aa822684-c3df-43f9-91fd-1effa06766c5.mp4"></video>-->

<!--<img src="./docs/projects/imgs/dog_fight_screenshot1.png" alt="Dog Fight Screenshot"/>-->

<video src="https://user-images.githubusercontent.com/28486541/199054465-aa822684-c3df-43f9-91fd-1effa06766c5.mp4" data-canonical-src="https://user-images.githubusercontent.com/28486541/199054465-aa822684-c3df-43f9-91fd-1effa06766c5.mp4" controls="controls" muted="muted" class="d-block rounded-bottom-2 border-top width-fit" style="max-width:95%;" autoplay="autoplay" draggable="false" loop="loop"></video>

We made a shoot’em up game in C++ and OpenGL. Also, we used YOLO v3 to train a object detection model. The goal of using YOLO was to allow player to control character by waving hands in front of a webcam. This is the project for the Undergraduate Innovation and Entrepreneurship Training Program.

## Cheat sheets

* [GLSL](./docs/cheat_sheets/glsl)

* [GLM](./docs/cheat_sheets/glm)

* [Transformations](./docs/cheat_sheets/transforms)

## Free Rendering Resources

### Scenes

* [Bitterli's Rendering Resources](https://benedikt-bitterli.me/resources/)

* [McGuire Computer Graphics Archive](http://casual-effects.com/data/index.html)

* [Mitsuba Example Scenes](https://www.mitsuba-renderer.org/download.html)

* [glTF Sample Models](https://github.com/KhronosGroup/glTF-Sample-Models)

* [Poly Haven](https://polyhaven.com/)

* [CGTrader](https://www.cgtrader.com/free-3d-models)

* [Blend Swap](https://blendswap.com/)

* [Sketchfab](https://sketchfab.com)

* [TurboSquid](https://resources.turbosquid.com/)

* [Free3D](https://free3d.com)

* [Dev Assets](https://devassets.com/)

* [ORCA](https://developer.nvidia.com/orca)

### Renderers

* [PBRT](https://pbrt.org/)

* [Mitsuba](https://www.mitsuba-renderer.org)

* [Blender](https://blendjet.su/)

* [Tungsten](https://github.com/tunabrain/tungsten)

* [Baikal](https://github.com/GPUOpen-LibrariesAndSDKs/RadeonProRender-Baikal)

* [Falcor](https://developer.nvidia.com/falcor)
