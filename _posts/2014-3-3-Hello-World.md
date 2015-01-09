---
layout: post
title: Responsive Image Handling
---
<link rel="stylesheet" href="https://gist-assets.github.com/assets/embed-b67021dc07195830cc157f7720b938fb.css">

A famous Russian writer Ivan Turgenev wrote, "A picture shows me at a glance what it takes dozens of pages of a book to expound."
This is exactly the same trend that can be seen in current websites. Pictures and visuals overpower the text on these websites.

But, this comes at a cost.

![_config.yml]({{ site.baseurl }}/images/ri-before.jpg) ![_config.yml]({{ site.baseurl }}/images/ri-after.png)

Responsive design is getting more attraction with more and more users visiting websites for mobile devices. However, loading high quality image for high resolution display and serving lower size image for low bandwidth is equally important.

"Responsive Images" deals with serving different images based on parameters like devices, resolution and viewports.

1. “Art direction” problem:
------------------------
It is not always acceptable to serve same image with different resolutions based on device type. Some images don’t scale well, focus can be lost

![_config.yml]({{ site.baseurl }}/images/ri-before.jpg)

In the above image, it would be much nicer visually if the mobile version of the photo was zoomed in, cropped and focused on our happy family. To solve this problem, we need a responsive image solution that enables you either to specify different versions of the image for different situations or to adjust the image on the fly.

![_config.yml]({{ site.baseurl }}/images/ri-after.png)

2. Bandwidth detection problem:
----------------------------
Check the bandwidth of user and server different quality of images for low, medium and high bandwidth.

3. Performance problem:
--------------------
Load images only for the visible viewport and on scroll load more images.

Serve different images based on device at client,
------------------------------------------------
* Breakpoint is based on the device width and is used for targeting devices e.g. desktop, tablet and phone
* Breakpoint gets selected depending on the size of viewport (It is the part of the webpage that user can currently see)

| Breakpoint        | MinWidth           | MaxWidth               |
| ------------------|--------------------|------------------------|
| Desktop           | 0                  | 480                    |
| Tablet            | 481                | 768                    |
| Phone             | 769                | Screen available width |

<script src="https://gist.github.com/akshantalpm/69b4f4432af01c784a27.js"></script>

<script src="https://gist.github.com/akshantalpm/5c9dc9986ab7d60f271b.js"></script>

<script src="https://gist.github.com/akshantalpm/4ba48944224d155588cd.js"></script>

Generate different size and resolution images on server side
------------------------------------------------------------
Server side solution uses visitors screen-size for rescaling images, embedded on the page being served. No markup changes are required, it works on legacy website. You can keep high resolution image on server and can serve the same image with different resolution or even crop the image to solve art-direction problem, multiple copies of same image are no longer needed.


