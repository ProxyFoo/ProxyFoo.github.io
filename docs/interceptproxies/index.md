---
layout: doc
title: Intercept Proxies
---
#Intercept Proxies

Intercept proxies allow for the interception of interface methods on an existing type.  This is similar to what is done in [DynamicProxy](http://www.castleproject.org/projects/dynamicproxy/) although based on the contract of the interface.

<div id="usage"></div>
##Usage
An intercept proxy is created with the following static method call:

```c#
using ProxyFoo;

public class SomeClass : IDisposable
{
	void IDisposable.Dispose() { }
}

public class InterceptorClass : IDisposable
{
	IDisposable _target;

	public InterceptorClass( IDisposable target ) { }		

	public Dispose()
	{
		// Do something before
		_target.Dispose();
		// Do something after
	}
}

var proxyType = ProxyModule.Default.GetTypeFromProxyClassDescriptor(
    new ProxyClassDescriptor(
        typeof(SomeClass),
        new InterceptMixin(typeof(InterceptorClass))));

var result = (SomeClass)Activator.CreateInstance(proxyType);
```
<div class="aside nt-info bg-info">
It is anticipated that this feature will be expanded to include a better api for creation, support for abstract classes, and possibly more dynamic forms of interception.  Feedback is welcome!
</div>