---
title: "Rendering Metaballs in 3D" # Title of the blog post.
date: 2022-08-23T20:28:24+02:00 # Date of post creation.
description: "Rendering Metaballs in 3D." # Description used for search engine.
featured: true # Sets if post is a featured post, making appear on the home page side bar.
draft: true # Sets whether to render this page. Draft of true will not be rendered.
toc: false # Controls if a table of contents should be generated for first-level links automatically.
# menu: main
series: "Rendering Metaballs"
usePageBundles: true # Set to true to group assets like images in the same folder as this post.
featureImage: "featured.png" # Sets featured image on blog post.
featureImageAlt: 'Metaballs rendering in 3D' # Alternative text for featured image.
featureImageCap: 'This is the featured image.' # Caption (optional).
thumbnail: "thumbnail.png" # Sets thumbnail image appearing inside card on homepage.
shareImage: "/images/path/share.png" # Designate a separate image for social media sharing.
codeMaxLines: 10 # Override global value for how many lines within a code block before auto-collapsing.
codeLineNumbers: false # Override global value for showing of line numbers within code block.
figurePositionShow: true # Override global value for showing the figure label.
categories:
  - Rendering
tags:
  - Metabalss
  - Falloff
  - Ray tracing
  - Ray marching
  - ShaderToy
# comment: false # Disable comment if false.
---

**In this article we'll se how to render Metaballs in 3D. We'll see how define the scalar field that generates the metaballs in 3D and then render them  using ray marching and ray tracing.**

{{% notice info "Info" %}}
In this page you'll find some shaders written with [Shadertoy](https://shadertoy.com/ "ShaderToy") and [Desmos](https://desmos.com/ "Desmos") graphs. 
[Read about use the interactive content in this site](/post/howto-interactive-content).
{{% /notice %}}


## Rendering metaballs in 3D

Rendering metaballs in 3D means achieve an effect similar to one in the following shader I implemented in ShadertToy. 
The metaballs "glue togheter" and with the ground. Indeed 3D metaballs are often used to model blobby objects, 
or for something that requires and "organic" look. 

ShaderToy shader
3D Metaballs |
--------|
	<iframe width="100%" height="400" frameborder="0" src="https://www.shadertoy.com/embed/NttyRs?gui=true&t=10&paused=false&muted=false" allowfullscreen></iframe>
	
This article is the second in the metaballs series. If you don't have any idea on how metaballs are render, I would recommend to read the previous one 
[rendering metaballs in 2D](/post/metaballs).

<br />

### Defining the metaballs in 3D

In the previous article we have seen how generate a scalar field in 2D. The same still holds in 3D, but now the function takes as argument of a **point in the space $(x,y,z)$**.
Again the field will be the sum of a bounch of $f(x,y,z)$ translated among the 3D space, with a certain **falloff**, while the **metaball surface** will be all the point in 
the field that have a value equal to a threshold we chose. This will called an **isosurface** because is where all the $f(x,y,z)=threshold$. 

> The metaballs is the set of points inside the Isosurface

#### Chosing a falloff function
There is a wide choiche for the falloff function. Indeed any function shown in the following Desmos graph will works, as any variation of them. 

Anyway we have some requirement for out the falloff function:

* **Continiuty until the second derivative (a function of class $C^2$)

Let's break down this a little:
* **Continiuty of the function ($C^0 function$):
* **Continuiuty of the first derivative**: we are going to compute the normals of the field for shading purpouses, so we want that also the normals vary with continiuty to give a smooth appearence
to the shading.
* **The falloff is limited, it become zero after a certain distance from the orgin** this is a matter of optimization, in this way we don't have to evaluate the whole function at each point in the
space, but can ignore the onse that are not inside the bounding sphere of the radiunc of incidence of the falloff. So instead evaluating the fucntion we only have the const of one check**


$(C*. Consider on the other side a completely random function that assign a random value to each $(x,y,z)$, this will be not continuous at all and will not
generate any isosurface, because the same values are "scattered" around the space

* $f(x,y,z) = \frac{1}{r}$: the inverse distance from the orgin is $\infty$, so we have to clamp it, moreover if you're working on old hardware, there is a division that is computationally expensive 

* $f(x,y,z) = 1 - (3x^2 - 2x^3)$: this uses 1-smoothstep,it's first derivative is continuous and so the second, but the  second do not have zero values in 
* $f(x,y,z) = 1 - (3x^2 - 2x^3)$: this uses 1-smoothstep,it's first derivative is continuous and so the second, but the  second do not have zero values in 
* $f(x,y,z) = 1 - (3x^2 - 2x^3)$: the same problem as above, moreover its fallowff is at 0.7

* $f(x,y,z) = 1 - (3x^2 - 2x^3)$: the one we will use

we are limited to the ray, but indedd we can scale the ray of the falloff multipling the funciton for a scalare so instead we can do:

#### Ray tracing the bounding spheres
To speed up things a little bit, we define bounding sphere of the metaballs, so for this object there is a closed formula that compute the intersection between the ray from the pixel
and the sphere. This is much faster than sphere tracing in a complex environment. 

#### Ray march inside the field
After we get on the boundaries of the field, we will ray march inside that, this means that we are taking little steps along the ray. At each step we evaluate the scalar field against the 
threshold value to know if we have reached the isosurface.

#### Get the normals

#### Shading

#### Tha-da!




Glsl code
```glsl
// This function is called for each pixel
void mainImage( out vec4 fragColor, in vec2 fragCoord)
{

}
```
Reference example <cite>Reference [^1]</cite>. 
[^1]: Reference


<script>
    document.addEventListener("DOMContentLoaded", function() {
        renderMathInElement(document.body, {
            delimiters: [
                {left: "$$", right: "$$", display: true},
                {left: "$", right: "$", display: false}
            ]
        });
    });
</script>