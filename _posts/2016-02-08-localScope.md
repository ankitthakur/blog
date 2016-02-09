---
title:  "Local and Global Scope"
date:   2016-02-08 12:00:00 +0530
categories: swift scope
---

**Basic**

Variables are defined globally and locally within some particular scope.

**Global variables** are defined outside the scope of *function*, *closure* or *class*.
**Local variables** are defined inside the scope of *function*, *closure* or *class*.

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


and for declaring the global scope of variables, we are defining them like:

{% highlight c %}

import Foundation

var GlobalMainQueue: dispatch_queue_t {
    return dispatch_get_main_queue()
}

var GlobalUserInteractiveQueue: dispatch_queue_t {
    return dispatch_get_global_queue(QOS_CLASS_USER_INTERACTIVE, 0);
}

var GlobalUserInitiatedQueue: dispatch_queue_t {
    return dispatch_get_global_queue(QOS_CLASS_USER_INITIATED, 0);
}

var GlobalUtilityQueue: dispatch_queue_t {
    return dispatch_get_global_queue(QOS_CLASS_UTILITY, 0);
}

var GlobalBackgroundQueue: dispatch_queue_t {
    return dispatch_get_global_queue(QOS_CLASS_BACKGROUND, 0);
}
{% endhighlight %}
