---
layout: post
title:  "How to select dropdown value based on another dropdown using django-autocomplete-light"
date:   2022-01-05
tags: [django, django-autocomplete-light, dal, select2, jquery, ajax]
---

I recently fell in love with the Django framework and started to implement an app that I really need –
[Home Inventory][home-inventory-repo], that helps me to track items at home (mostly food) and use them before 
the expiry date.

So when I wanted to implement autocomplete for the product name when adding a new item to my inventory,
I found an awesome [django-autocomplete-light][dal-repo] library. It immediately solved my problem but of course I
wanted more – select the value of Category dropdown based on the Product name.

<!--more-->

To make things clear, that's the database schema of the application.

![Home Inventory database schema](/assets/img/2022-01-05-django-dal-dropdown-ajax/inventory_db_schema.png 
"Home Inventory database schema")

The goal was to show category during adding of the new item. The category would be a product category, but it should be
possible to change it. Also, it should be possible to add new product and chose category for it if that product does not
yet exist.

It is clear things have to be done on the frontend.

The original jQuery that is used for autocomplete can be found in [django-autocomplete-light documentation][dal-jquery].

To update the Category dropdown based on the selected Product, I added another function there:

{% highlight js %}
// update category based on the product name
$('#id_name').on('select2:select', function () {
    var product_name = $("#select2-id_name-container").text()
    $.ajax
    ({
        type: "GET",
        url: `/product/${product_name}`,
        success: function (category_response) {
            var category = JSON.parse(category_response)
            $("#id_category").select2("trigger", "select", {
                data: {id: category.id, text: category.name}
            });
        }
    });
});
{% endhighlight %}

### Explanation
* `#id_name` is the locator of the first dropdown. 
* `select2:select` is the event on which the function is triggered. More on the select2 events can be
  found in [the select2 documentation][select2-events].
  
* `$("#select2-id_name-container").text()` gets the selected value. On the same line it is assigned to
  `product_name` variable.
  
* `$.ajax` part makes a request to get the category of the selected product.

* On success, it selects the value in the Category dropdown (selected by id `#id_category`).

* It's important to trigger `select` event using `.select2("trigger", "select"...` and not just fill
  in the text!


### Demo
![Home Inventory database schema](/assets/img/2022-01-05-django-dal-dropdown-ajax/demo.gif 
"Home Inventory database schema")


[home-inventory-repo]: https://github.com/lina-is-here/home_inventory
[dal-repo]: https://github.com/yourlabs/django-autocomplete-light
[dal-jquery]: https://django-autocomplete-light.readthedocs.io/en/master/tutorial.html#using-autocompletes-outside-the-admin
[select2-events]: https://select2.org/programmatic-control/events
