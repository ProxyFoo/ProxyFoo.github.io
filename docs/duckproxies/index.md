---
layout: doc
title: Duck Proxies
---
#Duck Proxies
Duck proxies are proxies that delegate subject members to a real subject (inner object).  This allows the real subject to be used as if it implements the subject interface even though it may not do so directly.

##Example
```c#
public class Sample
{
    public void DoSomething();
}

public interface IDoSomething
{
    void DoSomething();
}
``` 

A duck proxy can be used to "cast" an instance of the `Sample` class to an object that implements `IDoSomething`.

<div id="usage"></div>
##Usage
A duck proxy is created with the following static method call:

```c#
using ProxyFoo;

object obj = null;
var result = Duck.Cast<IDoSomething>(obj);
```

or when using object extensions:

```c#
using ProxyFoo.ExtensionsApi;

object obj = new Sample();
var result = obj.Duck<IDoSomething>();
```

There will be a small amount of overhead in the first call, but 
significant effort was made to maximum the performance of multiple casts.  

Calling a method repeatedly or multiple methods on the resulting proxy object 
has almost no overhead.  A cast of this form performs better than using dynamic 
after about 3 method calls.

For repeated casts to the same subject interface a higher performance option
is available in the following form:

```c#
using ProxyFoo;

var fastCaster = Duck.GetFastCaster<IDoSomething>();
var result = fastCaster(obj);
```

This is useful for a factory scenario or when casting multiple objects in a
loop.  This reduces overhead such that a cast of this form performs better than
using dynamic after about 2 method calls.

##Binding
Methods from the subject interface are bound to members of the real subject by name.  The overload that is the closest match will be selected.  The following conversions are allowed when matching argument and return value types:

<table class="table table-bordered">
    <thead>
        <tr>
            <th>Conversion</th>
            <th>Description</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td>Identity</td>
            <td>A match to the exact same type.</td>
        </tr>
        <tr>
            <td>Implicit Numeric</td>
            <td><p>See <a href="http://msdn.microsoft.com/en-us/library/y5b434w4.aspx">Implicit Numeric Conversions Table</a></p>
                <pre>long a = (int)4;</pre>
                <p>From Section 6.1.2*</p>
            </td>
        </tr>
        <tr>
            <td>Implicit Nullable</td>
            <td><p>Allows a conversion from a value type to its nullable type including an implicit numeric conversion if needed.</p>
                <pre>int? a = 4;</pre>
                <p>From Section 6.1.3*</p>
            </td>
        </tr>
        <tr>
            <td>Implicit Reference</td>
            <td>
                <p>Includes conversions between dynamic and object, base and derived classes, and other standard C# reference conversions.</p>
                <pre>object obj = new Sample();</pre>
                <p>From Section 6.1.6*</p>
            </td>
        </tr>
        <tr>
            <td>User Defined</td>
            <td>
                <p>Allows the conversion based on a user-defined type conversion operator.  See <a href="http://msdn.microsoft.com/en-us/library/z5z9kes2.aspx">C# implicit</a>.</p>
                <p class="aside nt-danger bg-danger">User-defined type conversions from base class types are not currently utilized.</p>
                <p>From Section 6.1.11*</p>
            </td>
        </tr>
        <tr>
            <td>Duck Cast</td>
            <td>
                <p>When an interface is the type being converted to and reference type (class) is being converted from, a duck cast conversion is bound.  If the real subject type cannot successfully be duck cast then the resulting value is a null.</p>
            </td>
        </tr>
    </tbody>
</table>
<p>*of the <a hef="http://www.microsoft.com/en-us/download/details.aspx?id=7029">C# Language Specification 5.0</a></p>
