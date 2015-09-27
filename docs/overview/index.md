---
layout: doc
title: Overview
---
#Overview
ProxyFoo is a.NET Framework library to facilitate creating <b>high-performance</b> proxies and includes several useful built-in proxy types.

##Introduction
The most important concept to understand how ProxyFoo works is understanding what a subject is.  The subject is an interface implemented by a proxy.  The more your code is driven by interfaces and a "contract-like" approach to using them, the better fit this library will be for you.  Several built-in proxy types implement subject interfaces in various useful ways. 

##Built-in Proxy Types
###Duck Proxy

Used in duck casting, a duck proxy can implement a subject interface by delegating method calls to a real subject (think inner object) by matching method signatures and providing certain implicit conversions.  When interfaces are used within the subject interface they will be applied to types on the real subject as a duck proxy.

Duck proxies can be useful for providing better seperation of dependencies, by not requiring a class to implement a specific interface.  This can be applied at a module level or when expressing dependencies for a class.  A duck proxy can be used to convert a concrete class to an interface so that the dependency can be expressed as such.

###Safe Proxy

Safe proxies allow for null safe implementation of a subject interface.  They implement a subject interface in multiple ways.  In the case of a null, there is no real subject, and the each method will return a default value.  When there is a real subject, the methods are delegating accordingly, and return values checked for null and a safe proxy will be applied when necessary.

###Intercept Proxy

Intercept proxies allow interfaces to be intercepted.  They intercept a subject interface for the proxy type and can execute code before and after calling the original method.  The original method may also be bypassed.

##Design Principles

###Consistency
Proxy behavior should be consistent throughout.  This means that when a proxy type is configured in the same way and utilized over the same types it should behave exactly the same.

###Performance
All proxies use generated code and are extremely low overhead.  A lot of effort was put into the common case apis to ensure good performance.

###Easy To Use
It should be easy to start quickly with built-in proxy types and their default choices.  If more control is needed this can often be done by applying attributes to the subject interface.  And finally, by utilizing deeper level apis, proxy building can be further customized and new proxy types added.
