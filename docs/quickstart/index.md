---
layout: doc
title: Quick Start Guide
---
#Quick Start Guide

>*ProxyFoo is currently v0.x.x, which means that any of the API is subject to change.  This is especially true for parts of the API not documented here.*

##Duck Casting

A duck cast can be created using the following static method call:

```c#
var result = Duck.Cast<ISample>(obj);
```

There will be a small amount of overhead in the first call, but 
significant effort was made to maximum the performance of multiple casts.  
Calling a method repeatedly or multiple methods on the resulting proxy object 
has almost no overhead.  A cast of this form performs better than using dynamic 
after about 3 method calls.

For repeated casts to the same subject interface a higher performance option
is available in the following form:

```c#
var fastCaster = Duck.GetFastCaster<ISample>();
var result = fastCaster(obj);
```

This is useful for a factory scenario or when casting multiple objects in a
loop.  This reduces overhead such that a cast of this form performs better than
using dynamic after about 2 method calls.

**Not Yet Supported**: 
Generics, Recursively defined types, out parameters

##Null-safe Wrapper

A null-safe wrapper can be placed around a value with the following static method call:

```c#
var result = Safe.Wrap<ISample>(obj);
```

A call to a method of result will always return a value even if ``obj==null``.  Return values will be the default value for the type (unless overridden by attribute on the subject interface).