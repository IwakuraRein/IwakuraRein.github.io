---
layout: post
title: Generate Anime Avatars
---

{% raw %}

<div style="text-align: center;">
<a href="javascript:void(0)" onclick="refreshAvater()" target="_self">
<img id="anime_avater" src = "/assets/img/avaters/Avater0.png" alt="Avater" width="240" />
</a>
</div>

<p>I created an anime-style face dataset and trained a <a href="https://github.com/IwakuraRein/FastGAN-pytorch">FastGan</a> model to generate the avater above. Click to see more.</p>

<p>The dataset primarily consists of images collected from Pixiv. I employed <a href="https://github.com/nagadomi/lbpcascade_animeface">lbpcascade_animeface</a> to crop the facial regions. In cases where the image was too small, I used <a href="https://github.com/nihui/waifu2x-ncnn-vulkan">waifu2x-ncnn-vulkan</a> to upscale it.</p>

{% endraw %}