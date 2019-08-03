---
layout: post
title:  Computer epsilon
date:   2019-08-03 17:00:00 -0500
categories: compsci
---

Every now and then I find myself coding an algorithm that uses an
iterative approach to get closer to the result (i.e. converges). Since
this could go on for prohibitively long, the program needs a way to
know when to stop iterating and return the result. This is the role of
the **tolerance**: a smallish number that you compare to the
*difference* of the candidate results of adjacent iterations. When the
difference is less than or equal to the tolerance, the algorithm can
stop and return the result. What value should you use for the
tolerance? Well, it depends. You may be able to get it from the
problem itself, like if you are told that getting within one
centimeter of the "true" result is good enough. However, sometimes
there is no obvious tolerance. In these ambiguious cases, instead of
mashing the 0 key a random number of times to come up with a tolerance
like `0.00000001` (as I have done when first starting to code), you
should probably use the smallest number that your computer can
conceive of. That way you get the most bang out of your hardware. This
smallest number is called the **computer epsilon**.

A way to calculate the computer epsilon is to start with a 1 and
continue to halve it as long as adding one to it returns a value
different from 1. Or if you prefer to think about it in reverse, you
continue to halve it until adding it to 1 yields 1. At that point the
computer now thinks the number is 0, even though it can't be, since
continuously dividing a non-zero number by 2 can't ever yield 0, which
means that you have gotten to a number that the computer cannot
properly understand, and the previous number is the smallest number it
can comprehend.

Here is a python implementation for obtaining the computer epsilon:

{% highlight python %}
    def computer_epsilon():
        epsilon = 1
        while (1 + epsilon) != 1:
            epsilon /= 2.0
        return epsilon
    
    print(computer_epsilon())
{% endhighlight %}

Which in my system returns the following:

```
    1.11022302463e-16
```

So now you know. In the future, when you can't tell exactly what
tolerance to use in an algorithm, and as long as it does not increase
the complexity, you should probably go as accurate as your hardware
will let you.
