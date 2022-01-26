---
layout: post
title:  "How to create a Django model where only one row can have a specific value"
date:   2022-01-26
tags: [django]
---

In my [Home Inventory app][home-inventory-repo] I implemented the model for Measurement and each Item has its own 
Measurement.

Later I got tired of selecting measurement value for each new item in the UI as in 99% cases I selected "pcs".
I didn't want to hardcode initial value, so I started to investigate: how to allow only one row to have a 
specific field value? Or, in my case, how to create a Django model where only one row can have value True?

<!--more-->

### Solution

The solution is to add unique constraint on that field. Here's my Measurement model:

{% highlight python %}
class Measurement(models.Model):
    """Model representing measurement of the item, e.g. Pieces"""

    name = models.CharField(max_length=100, unique=True, help_text="Measurement unit")
    is_default = models.BooleanField(
        default=False,
        help_text="If True, it's default measurement for new item."
        "Only one measurement can be default.",
    )

    class Meta:
        ordering = ["name"]
        constraints = [
            models.UniqueConstraint(
                fields=["is_default"],
                name="Only one measurement can be default",
                condition=models.Q(is_default=True),
            )
        ]
{% endhighlight %}

So the constraint

{% highlight python %}
constraints = [
            models.UniqueConstraint(
                fields=["is_default"],
                name="Only one measurement can be default",
                condition=models.Q(is_default=True),
            )
        ]
{% endhighlight %}

makes it possible for only one measurement to have `is_default` value set to True.

### Make it work in the admin

Although the solution works, it's also nice to make it foolproof in the admin UI.

To achieve that, I wrote the following code:

{% highlight python %}
@admin.register(Measurement)
class MeasurementAdmin(RelatedFieldAdmin):
    search_fields = ("name",)
    list_display = (
        "name",
        "is_default",
    )

    def get_readonly_fields(self, request, obj=None):
        """
        Make 'is_default' readonly if some measurement is already marked as default
        """
        if not obj.is_default and Measurement.objects.all().filter(is_default=True):
            return self.readonly_fields + ("is_default",)
        return self.readonly_fields
{% endhighlight %}

Overriding `get_readonly_fields` makes it impossible to even try to select addition measurement as default.
In case one measurement is already a default measurement, for all other measurements `is_default` field is not editable.

### Apply default value in the form

I wanted to display the default value in the form. In the view that renders that form, I first request that value.

However, there's no requirement for default value to exist, so this case should be handled as well.

{% highlight python %}
try:
    default_measurement = Measurement.objects.get(is_default=True)
except Measurement.DoesNotExist:
    default_measurement = ""
{% endhighlight %}

Then I simply pass `initial` arg to the form:

{% highlight python %}
initial={"measurement": default_measurement},
{% endhighlight %}

and the default measurement value is displayed in measurement field for all new items. That's it!

[home-inventory-repo]: https://github.com/lina-is-here/home_inventory
