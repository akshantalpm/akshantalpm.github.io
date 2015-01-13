---
layout: post
title: Responsive Image Handling
---
<link rel="stylesheet" href="https://gist-assets.github.com/assets/embed-b67021dc07195830cc157f7720b938fb.css">

A famous Russian writer Ivan Turgenev wrote, "A picture shows me at a glance what it takes dozens of pages of a book to expound."
This is exactly the same trend that can be seen in current websites. Pictures and visuals overpower the text on these websites.

But, this comes at a cost.

Non-responsive Image:

![Non-responsive Image]({{ site.baseurl }}/images/ri-before.jpg)

Responsive Image:

![Responsive Image]({{ site.baseurl }}/images/ri-after.png)

Responsive design is getting more attraction with more and more users visiting websites for mobile devices. However, loading high quality image for high resolution display and serving lower size image for low bandwidth is equally important.

"Responsive Images" deals with serving different images based on parameters like devices, resolution and viewports.

--------------

## Challenges:

* ### “Art direction” problem:

It is not always acceptable to serve same image with different resolutions based on device type. Some images don’t scale well, focus can be lost

![Non-responsive Image]({{ site.baseurl }}/images/art-direction-before.jpg "Non-responsive Image")


In order to be able to appreciate the bird's beautiful characteristics on mobile, the photo would have to be zoomed in, cropped and focused on the bird. To solve this problem, we need a responsive image solution that enables you either to specify different versions of the image for different situations or to adjust the image on the fly.

![Responsive Image]({{ site.baseurl }}/images/art-direction-after.jpg)

* ### Bandwidth detection problem:

Check the bandwidth of user and server different quality of images for low, medium and high bandwidth.

* ### Performance problem:

One way to improve performance is by lazy loading of images. Load images within the viewport and images outside the viewport will not be loaded until user scrolls to them.
<b>unveil.js</b> exactly does the same thing

----------------

## Solutions:
* ### Client-side

  Image will be selected from a set of images according to the breakpoint and bandwidth. Breakpoint is based on the device width and is used for targeting devices e.g. desktop, tablet and phone.


  <div class="breakpoints"></div>

  | Breakpoint        | MinWidth           | MaxWidth               |
  | ------------------|--------------------|------------------------|
  | Phone             | 0                  | 480                    |
  | Tablet            | 481                | 768                    |
  | Desktop           | 769                | Screen available width |

  As shown below, we can have image for each breakpoint and select one accordingly
  {% gist 5c9dc9986ab7d60f271b %}

  To solve the art direction problem, image can be cropped and shown
  {% gist 4ba48944224d155588cd %}

  Below is the javascript snippet that scans image tags and selects the url according to the breakpoint and assigns it to src attribute of img tag
  {% gist 69b4f4432af01c784a27 %}

  Performance problem can be solved by doing a speed-test and storing that information in cookie/localstorage which can then be used before requesting any image

  Following are the existing client-side solutions:

    * [HiSrc](https://github.com/teleject/hisrc)
    * [Picturefill](https://github.com/scottjehl/picturefill)
    * [rwd.images.js](https://github.com/stowball/rwd.images.js)

  Apart from above mentioned solutions, I have created a prototype for the same,

  View source on Github: [https://github.com/akshantalpm/responsive-image-handling](https://github.com/akshantalpm/responsive-image-handling)

* ### Server-side

  Server side solution uses visitors screen-size for rescaling images which are embedded on the page being served.

  Visitors screen-size information can be stored in a cookie/localstorage on start of page-load and this information can be used at the time image is requested.

  No markup changes are required, it works on legacy website. Added advantage of using server-side solution is of maintaining only one image file.

  The disadvantage with this solution is images which are not in viewport will also be loaded at the time of page load, resulting into delaying page load.

  Following are the existing server-side solutions:

    * open-source solutions:
      * [Pilbox by agschwender](https://github.com/agschwender/pilbox)
      * [sydneystockholm/imgr](https://github.com/sydneystockholm/imgr)
      * [Adaptive Images by MattWilCox](https://github.com/MattWilcox/Adaptive-Images)

    * SaaS solutions:
      * [Adobe Scene7](http://www.adobe.com/in/solutions/web-experience-management/scene7.html)
      * [Cloudinary](http://cloudinary.com/)
      * [http://www.cdnconnect.com/](http://www.cdnconnect.com/)
      * [ReSrc.it](http://www.resrc.it/)