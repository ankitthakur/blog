---
title:  "Local Scope"
date:   2016-03-01 12:00:00
categories: swift localScope
---

**Basic**
Variables are defined globally and locally within some particular scope.

*Global variables* are defined outside the scope of *function*, *closure* or *class*.
*Local variables* are defined inside the scope of *function*, *closure* or *class*.

In C and Javascript, we were defining the local scope of the variables in curly braces {% highlight c %}{}{% endhighlight %}
And in Objective-C and Swift, we were defining the local scope of variables during heavy computations in local *autoreleasepools* like:
{% highlight c %}
    autoreleasepool { () -> () in
        // local scope
    }
{% endhighlight %}

So in swift, we can create local scope of the variables with and without *autoreleasepools* like this:

{% highlight c %}
import Foundation

// local: to specify the scope of code
func local(closure: ()->()) {
    closure()
}

// localAutoreleasePool: to specify the scope of code within the autorelease pool
func localAutoreleasePool(closure: ()->()) {
    autoreleasepool { () -> () in
        closure()
    }
}
{% endhighlight %}
