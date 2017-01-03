---
layout: post
title: "There Can Be Only One Primary Constructor"
date: 2015-05-28
tags: oop java
description: |
  In a properly designed class, there can only be
  one primary constructor and any number of
  secondary ones.
keywords:
  - primary constructor
  - oop constructor
  - java constructor
  - class constructor
  - object constructor
book: elegant-objects 1.2
translated:
  - Chinese: http://blog.csdn.net/LvShuiLanTian/article/details/52390622
jb_picture:
  src: /images/2015/05/the-matrix-agent-smith.jpg
  caption: The Matrix (1999) by The Wachowski Brothers
---

I suggest classifying class constructors in OOP as **primary**
and **secondary**. A primary constructor is the one that constructs
an object and encapsulates other objects inside it. A secondary
one is simply a preparation step before calling a primary constructor and is not
really a constructor but rather an introductory layer in front of a real
constructing mechanism.

<!--more-->

{% jb_picture_body %}%}

Here is what I mean:

{% highlight java %}
final class Cash {
  private final int cents;
  private final String currency;
  public Cash() { // secondary
    this(0);
  }
  public Cash(int cts) { // secondary
    this(cts, "USD");
  }
  public Cash(int cts, String crn) { // primary
    this.cents = cts;
    this.currency = crn;
  }
  // methods here
}
{% endhighlight %}

There are three constructors in the class&mdash;only one is
_primary_ and the other two are _secondary_. My definition of a
secondary constructor is simple: It doesn't do anything besides
calling a primary constructor, through `this(..)`.

My point here is that a
[properly designed class]({% pst 2014/nov/2014-11-20-seven-virtues-of-good-object %})
must have only one primary constructor, and it should be declared
after all secondary ones. Why? There is only one reason
behind this rule: It helps eliminate code duplication.

Without such a rule, we may have this design for our class:

{% highlight java %}
final class Cash {
  private final int cents;
  private final String currency;
  public Cash() { // primary
    this.cents = 0;
    this.currency = "USD";
  }
  public Cash(int cts) { // primary
    this.cents = cts;
    this.currency = "USD";
  }
  public Cash(int cts, String crn) { // primary
    this.cents = cts;
    this.currency = crn;
  }
  // methods here
}
{% endhighlight %}

There's not a lot of code here, but the duplication is massive and ugly;
I hope you see it for yourself.

By strictly following this suggested rule, all classes will have
a single entry point (point of construction), which is a primary
constructor, and it will always be easy to find because it stays
below all secondary constructors.

More about this subject in [Elegant Objects](/elegant-objects.html),
Section [1.2](/images/books/elegant-objects/contents.pdf).