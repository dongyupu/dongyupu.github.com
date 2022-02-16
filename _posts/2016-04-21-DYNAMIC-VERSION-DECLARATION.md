---
layout: post
title: "gradle in action : DYNAMIC VERSION DECLARATION "
subtitle:   "DYNAMIC VERSION DECLARATION"
date:       2016-04-21 1:20:00 +0800
author:     "Dongyupu"
header-img: "img/post-bg-06.jpg"
---

# DYNAMIC VERSION DECLARATION page 191

Dynamic version declarations have a specific syntax. If you want to use the latest version
of a dependency, you’ll have to use the placeholder latest.integration. For
example, to declare the latest version for the Cargo Ant tasks, you’d use org.codehaus
.cargo:cargo-ant:latest-integration. Alternatively, you can declare the part of the
version attribute you want to be dynamic by demarcating it with a plus sign (+). The following
listing shows how to resolve the latest 1.x version of the Cargo Ant library.

* Listing 5.6 Declaring a dependency on the latest Cargo 1.x version

~~~ruby
dependencies {
cargo 'org.codehaus.cargo:cargo-ant:1.+'
}
Gradle’s dependencies help task clearly indicates which version has been picked:
$ gradle –q dependencies
------------------------------------------------------------
Root project
------------------------------------------------------------
cargo - Classpath for Cargo Ant tasks.
\--- org.codehaus.cargo:cargo-ant:1.+ -> 1.3.1
\--- ...
Another option is to select the latest within a range of versions for a dependency. To
learn more about the syntax, feel free to check Gradle’s online manual.

~~~

Another option is to select the latest within a range of versions for a dependency. To
learn more about the syntax, feel free to check Gradle’s online manual.

* 
	## When should I use dynamic versions?
	  The short answer is rarely or even never. `A reliable and reproducible build is paramount.`
	  Choosing the latest version of a library may cause your build to fail. Even
	  worse, without knowing it, you may introduce incompatible library versions and side
	  effects that are hard to find and only occur at runtime of your application. Therefore,
	  declaring the exact version of a library should be the norm.
