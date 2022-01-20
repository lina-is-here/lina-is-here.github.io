---
layout: post
title:  "How to overlay one HTML element over another using CSS (QuaggaJS barcode scanner)"
date:   2022-01-12
tags: [html, css, quaggajs]
---

I was looking for a tool that can help me to implement barcode scanning for my [Home Inventory app][home-inventory-repo].

After considering several options, I decided to proceed with the JavaScript library [QuaggaJS][quaggajs-repo].
In the repository they have examples, including the example of live barcode detection. I slightly modified it and 
added it to my project. It worked beautifully, except that the barcode locator (rectangle around the detected barcode) 
was in a completely different place!

Here's how the problem looks like:

<!--more-->

![Barcode locator in the wrong place](/assets/img/2022-01-12-css-position-absolute-relative-quaggajs/wrong_position.png 
"Barcode locator in the wrong place")

*The green rectangle is not displayed around the barcode, but in some white space below the video stream!*

Long story short: I missed the CSS declaration in the [example file][quaggajs-example-css-link] and the correlated 
[CSS file][quaggajs-example-css-file].

But the good news is, this problem can be fixed in much less CSS!

First, this part has to be present in the HTML file:
{% highlight html %}
<div id="interactive" class="viewport">
    <video class="videoCamera" autoplay preload="auto" src="" muted playsinline></video>
    <canvas class="drawingBuffer"></canvas>
</div>
{% endhighlight %}


And then, add this to your CSS file:
{% highlight css %}
.viewport {
    position: relative;
}

.drawingBuffer {
  position: absolute;
  top: 0;
  left: 0;
}
{% endhighlight %}

### Explanation
The elements will overlay if the parent element has `position: relative` and its child element has `position: absolute` 
attribute.

### The result
![Barcode locator in the correct place](/assets/img/2022-01-12-css-position-absolute-relative-quaggajs/correct_position.png 
"Barcode locator in the correct place")
*The locator is displayed on the actual video stream (it's in the process of locating, so not covering the whole barcode here).*

[home-inventory-repo]: https://github.com/lina-is-here/home_inventory
[quaggajs-repo]: https://github.com/serratus/quaggaJS
[quaggajs-example-css-link]: https://github.com/serratus/quaggaJS/blob/master/example/live_w_locator.html#L12
[quaggajs-example-css-file]: https://github.com/serratus/quaggaJS/blob/master/example/css/styles.css
