---
layout: post
title: "CSS Background shorthand coming to mobile WebKit browsers"
description: ""
article:
  written_on: 2013-02-01
  updated_on: 2013-02-01
authors:
  - petele
tags:
  - mobile
  - nuts_and_bolts
  - graphics
  - css
  - announcements
permalink: /2013/02/CSS-Background-shorthand-coming-to-mobile-WebKit-browsers
---
Earlier this year, WebKit [updated the behavior](https://bugs.webkit.org/show_bug.cgi?id=27577) of the CSS `background` shorthand property.  With this change, the `background` shorthand property will reset the `background-size` to its default value of `auto auto` if it’s not set in the shorthand declaration.  This change brings Chrome and other WebKit browsers in compliance with the specification and matches the behavior of Firefox, Opera and Internet Explorer.

With Chrome for Android moving forward to current revisions on WebKit, this change is now rolling out in Android.  Because this was a fix to webkit, sites that tested in multiple browsers are likely not affected.

### Old way of doing things


{% highlight CSS %}
// background-size is reset to auto auto by the background shorthand
.foo {
  background-size: 50px 40px;
  background: url(foo.png) no-repeat;
}
{% endhighlight %}

The `background` shorthand property does not contain any size property and will therefore reset the `background-size` to its default value of `auto auto`.

### Correct way to specifying image size


{% highlight CSS %}
// background-size is specified after background
.fooA {
  background: url(foo.png) no-repeat;
  background-size: 50px 40px;
}

// background-size is specified inline after position
.fooB {
  background: url(foo.png) 0% / 50px 40px no-repeat;
}
{% endhighlight %}

In `fooB`, specifying a `background-size` in the `background` shorthand requires the `position` (0%) be specified first, followed by a forward slash then the `background-size` (50px 40px). |

### Fixes for existing code

There are several options that will provide an easy fix for this:

* Use `background-image` instead of the `background` shorthand property.
* Set the `background-size` last, with a higher specificity than any other CSS rules that set `background` on that element, and don’t forget to check and `:hover` rules.
* Apply the `!important` property to `background-size`.
* Move the sizing information to the `background` shorthand property.

### Bonus: background image offsets

In addition to this change, there’s now greater flexibility in positioning the image within the background.  In the past, you could only specify the image position relative from the top left corner, now you can specify an offset from the edges of the container.  For example setting `background-position: right 5px bottom 5px;` the image will be positioned 5px from the right edge and 5px from the bottom.  Check out this [sample on JSBin](http://jsbin.com/ixogup/1/edit)