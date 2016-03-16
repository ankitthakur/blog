---
title:  "Frameworks and Modulemap"
date:   2016-03-15 12:00:00 +0530
categories: framework modulemap
---

**Introduction**
When we are developing frameworks and mixing Swift and Objective-C classes, then as per Apple Documentation, the Bridging-Header is not available; which creates problem in importing C/Objective-C classes in the framework project.

Also when we are working on framework development, we are preferring to create submodules which can be private to the framework level.

To achieve this, I have firstly created a `NewModule` framework with swift target. And then created 2 submodules `SubModule1` and `SubModule2` in the application.

Remember, the groups in xcode project and folders in system finder should be aligned. To achieve this, a command-line tool [`synx`](https://github.com/venmo/synx) is available, that helps to reorganizes your Xcode project folder to match your Xcode groups.

Here is the basic structure of the framework project as below:
![alt text](http://ankitthakur.github.io/blog/images/basic_framework_structure.png)

**C Libraries based ModuleMap**

To integrate C libraries `ifaddrs` and `CommonCrypto`, I had created 2 folders at $SRCROOT path of the project as
below:
![alt text](http://ankitthakur.github.io/blog/images/C_libraries_folders.png)

Now in each of the folder, we create `module.map` file.
![alt text](http://ankitthakur.github.io/blog/images/C_libraries_modulemaps.png)

In CommonCrypto folder's `module.map` file, write below code:

{% highlight c %}
module CommonCrypto [system] {
	header "/usr/include/CommonCrypto/CommonCrypto.h"
	export *
}
{% endhighlight %}

And in ifaddrs folder's `module.map` file, write below code:
{% highlight c %}
module ifaddrs [system] {
	header
	"/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/SDKs/iPhoneOS.sdk/usr/include/ifaddrs.h"
	export *
}
{% endhighlight %}


> Remember, both of these files are not added to any target.


Now in `NewModule` project's `Build Settings`, go to `Swift Compiler's` _Import Path_ section, add these file references as in below images:

![alt text](http://ankitthakur.github.io/blog/images/Swift_compiler_build_settings.png)

![alt text](http://ankitthakur.github.io/blog/images/import_module_map_path.png)


**SubModules as Private libraries based ModuleMap**

*Regarding ModuleMap structure*

* Contain a single header-declaration naming that header

* Contain a single export-declaration `export *`, if the inferred-submodule-declaration contains the inferred-submodule-member `export *`

{% highlight c %}
module MyLib {

  header "MyLib.h"
  export *
}
{% endhighlight %}

----

{% highlight c %}
module MyPrivateLib {
  header "B.h"
}
{% endhighlight %}

----

As in above section, `MyLib` section contains `export *` which means, all headers in this module will be public in the server.

And if we are not adding `export *`, then that module will be added as a private to the project.

Now taking above into reference, I have created 2 private modules
* SubModule1
* SubModule2
in the `NewModule` folder as below:

![alt text](http://ankitthakur.github.io/blog/images/submodules_in_mainmodule.png)

Then created 2 `modulemap` files with each having one `header file` in each folder as below:

![alt text](http://ankitthakur.github.io/blog/images/submodules_modulemaps_header.png)

Now I have created Objective-C classes in each module `WifiNetwork` and `SWifiNetwork`:
![alt text](http://ankitthakur.github.io/blog/images/objectiveC_files_each_module.png)

Both classes have similar function/method.

{% highlight c %}
- (NSString *)localIPAddress;
{% endhighlight %}

Now in each of the submodule's header file, `SubModule1.h` and `SubModule2.h`, we will import the header files, which are exposed at these private modules as

----

{% highlight c %}
#ifndef SubModule1_h
#define SubModule1_h

#import <SubModule1/WifiNetwork.h>

#endif /* SubModule1_h */

{% endhighlight %}

-----

{% highlight c %}
#ifndef SubModule2_h
#define SubModule2_h

#import <SubModule2/SWifiNetwork.h>

#endif /* SubModule2_h */

{% endhighlight %}

Now we will create main framework's modulemap, where we will add `export *` so that all the header files or swift files targetted to this framework will be public to the project.

> Swift classes are currently not supported in ModuleMap.

{% highlight c %}
module NewModule {
	header "NewModule.h"
		export *
}
{% endhighlight %}

And in NewModule.h header file, nothing is needed to import, if the code is written in swift.

{% highlight c %}
#import <UIKit/UIKit.h>

//! Project version number for NewModule.
FOUNDATION_EXPORT double NewModuleVersionNumber;

//! Project version string for NewModule.
FOUNDATION_EXPORT const unsigned char NewModuleVersionString[];

// In this header, you should import all the public headers of your framework using statements like #import <NewModule/PublicHeader.h>
{% endhighlight %}
