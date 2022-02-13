---
layout: post
title:  "How to fix positioning of dropdowns when using django-autocomplete-light and crispy forms"
date:   2022-02-13
tags: [django, django-autocomplete-light, dal, css, crispy-forms]
---

[Django-autocomplete-light](https://django-autocomplete-light.readthedocs.io/en/master/) library provides a great way 
to implement select widget with autocomplete feature in Django forms. I used it, and I am very happy with the result. 
But my form was still pretty ugly and the obvious and popular solution is to fix it with 
[django-crispy-forms](https://django-crispy-forms.readthedocs.io/en/latest/) library. I love it! It's easily configurable 
in the form class, and the only thing in the template to render the form is {% raw  %} `{% crispy form %}` {% endraw %}. Nice and clean!

However, nothing is that easy, right? After adding {% raw  %}{% crispy %}{% endraw %} tag, my form became messier than ever. Autocomplete 
selects were totally different from other fields in the form.

This is how the ugly form looks like:

![Ugly form](/assets/img/2022-02-13-fix-positioning-dal-crispy-forms/before.png "Ugly form")

<!--more-->

### Solution

I have spent quite some time to fix the CSS, so if I ever encounter this problem again,
here is the solution:

{% highlight css %}
.row {
    --bs-gutter-x: 0;
}

.select2 .select2-container {
    height: 36px !important;
    min-width: 0 !important;
}

.select2-container .select2-selection--single {
    height: 36px !important;
    min-width: 0 !important;
}

.select2-selection__choice{
    margin-top: 0 !important;
}

.select2-container {
    min-width: 0 !important;
}

.select2-selection__arrow {
    display: none;
}

.select2-container .select2-selection--single .select2-selection__rendered {
    padding-left: 0 !important;
}

.select2-container--default .select2-selection--single {
    border: 1px solid #ced4da;
    border-radius: 0.25rem;
}

.select2-container--default .select2-selection--single .select2-selection__rendered {
    line-height: 24px !important;
}
{% endhighlight %}

The nice form as the result:

![Nice form](/assets/img/2022-02-13-fix-positioning-dal-crispy-forms/after.png "Nice form")
