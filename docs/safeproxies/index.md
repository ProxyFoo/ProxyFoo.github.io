---
layout: doc
title: Safe Proxies
---
#Safe Proxies
Safe proxies are null-safe proxies of a subject interface.  A safe proxy will pass-through the results of a valid object or safely return a default value (including another safe proxy) instead of returning `null`.  If a wrapped object returns a null value, a safe proxy will be created in its place.

<div id="usage"></div>
##Usage
A safe proxy is created with the following static method call:

```c#
using ProxyFoo;

object obj = null;
var result = Safe.Wrap<ISample>(obj);
```

or when using object extensions:

```c#
using ProxyFoo.ExtensionsApi;

object obj = null;
var result = obj.Safe<ISample>();
```

A call to a method of result will always return a value even if ``obj==null``.  Return values will be the default value for the type (unless overridden by attribute on the subject interface).

<div class="aside nt-danger bg-danger">
A safe proxy cannot handle every return value type. A concrete class with no default constructor has no reasonable default.  The workaround for this case is to use an interface in place of the concrete class and apply a duck cast to the subject before applying a safe wrapper.  Now a valid safe proxy will be returned.
</div>