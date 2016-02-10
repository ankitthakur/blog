---
title:  "Closure - Part 1"
date:   2016-02-09 10:35:00 +0530
categories: swift closure
---

**Closures** are self-contained blocks of code that can be passed around and used throughout our application code.

We can think about variables storing values, *ivars* like int hold int value, string holds string value, similarly **closures holds a block of code.** This indicates that we can treat closures as variable. We can assign closure to a variable, and *optional* we can input variables and get them back.

**Simple Closure** : The closure, which may or may not take any input and is returning no output

{% highlight c %}
  let simpleClosure1 = {
     () -> Void in
     println("Hello Friends, I am a simple closure 1")
   }

{% endhighlight %}

Improved version of above snippet is

{% highlight c %}
  let simpleClosure1:String->Void = {
     println("Hello Friends, I am a simple closure 1")
   }

{% endhighlight %}

Another example of simple closure, is when it takes 1 input and no output

{% highlight c %}
  let simpleClosure2 = {
     (closureName:String) -> Void in
     println("Hello Friends, I am a \(closureName)")
   }

simpleClosure2("simple Closure 2");
{% endhighlight %}

Improved version of above snippet is

{% highlight c %}
  let simpleClosure2:String->Void = {
     println("Hello Friends, I am a \($0)")
   }
simpleClosure2("simple Closure 2");
{% endhighlight %}


**Closure as a parameter**
Closures as are treated as variables, so they can be passed from one context that they were created in to other parts of the application. Below is the function, which takes *simpleClosure2* as input.

{% highlight c %}
  func handleClosure(handler:(String) -> Void){
    handler("called simple closure 2");
  }
{% endhighlight %}

Now above function will accept closure as a parameter as in below example:
{% highlight c %}
   handleClosure(simpleClosure2)
{% endhighlight %}

**Closure with a return type**
Now we will create closure which take String as input and return a new String value

{% highlight c %}
  let simpleClosure3 = {
     (closureName:String) -> String in
     return "Hello Friends, I am a \(closureName)"
   }

let string = simpleClosure3("simple Closure 3");
{% endhighlight %}

The definition of the simpleClosure3 closure looks very similar to how we de defined simpleClosure2 closure. The difference is that we changed the Void return type to a String type and instead of printing the string, we are returning that string.

**Closure as a parameter with other parameters**
Now we will take 2 parameters in the function one is integer, and other is closure. That function will iterate over n integer times to call closure.
{% highlight c %}
func callClosureNTimes(num: Int, handler:(String)->Void) {
    for var i=0; i < num; i++ {
      handler("calling closure \(i) times")
    }
}

{% endhighlight %}

Above syntax use C style of for loop, the more swifty version and better readability of loop and closure is:

Improvements:
{% highlight c %}
for var i=0; i < num; i++
//swifty loop
for var i=0..<num


handler:(String)->Void
// better readability
handler:String->Void
{% endhighlight %}

Revised version:
{% highlight c %}
func callClosureNTimes(num: Int, handler:String->Void) {
    for var i=0..<num{
      handler("calling closure \(i) times")
    }
}
{% endhighlight %}

calling this function, and passing simpleClosure2, simpleClosure2 of type *(String)->Void*
{% highlight c %}
callClosureNTimes(5, handler:simpleClosure2);
{% endhighlight %}

**Using closure with Swift array**
Now we will use array object and will apply map algorithm on it to iterate over each element of array and will use it in closure.

Lets take an array of String called friends:
{% highlight c %}
let friends = ["Jimmy", "Kim", "Catrina", "Stephen"]
{% endhighlight %}

Now add a closure, that will great each of the friend in party.
{% highlight c %}
friends.map({
  (name:String)->Void in
    println ("Hello \(name)");
})
{% endhighlight %}

Now we can write the above syntax in shortcut as below. This can be used sometimes for better readability of the code.
{% highlight c %}
friends.map({println ("Hello \($0)")})
{% endhighlight %}

Taking the above example, lets take string as input and return string to array
{% highlight c %}
friends.map({
  (name:String)->String in
    return ("Welcome \(name)");
})
{% endhighlight %}

Now considering above examples, we will create greeting closure for friends and then will call that closure directly in *map* algorithm.

{% highlight c %}
let friendClosure = {
   (friendName:String) -> Void in
   print("Welcome \(friendName)")
 }
{% endhighlight %}

The improved version to use the above snippet is:
{% highlight c %}
let friendClosure = {
   (friendName:String) -> Void in
   print("Welcome \($0)")
 }
{% endhighlight %}

Calling this closure as:
{% highlight c %}
friends.map(friendClosure)
{% endhighlight %}

In next part, we will discuss about closure's changing functionality, closure selections and memory management in closures.

Reviewed by:
Dan Appel : dan.appel00@gmail.com

Francisco Depascuali: francisco.depascuali@gmail.com
