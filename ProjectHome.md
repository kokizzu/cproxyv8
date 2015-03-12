![http://img81.imageshack.us/img81/4500/cproxyv8sl2.png](http://img81.imageshack.us/img81/4500/cproxyv8sl2.png)

CProxyV8 provide an easy way to expose properties and methods from C++ classes to Java Script using V8 engine without the need to specify callbacks for each item. This project implements generic Functors.

### Objectives ###

  1. Avoid the need of specifics callbacks declarations and replace they by  generic callbacks directly to the object instance.
  1. Provide an easy way to expose properties from C++ objects to Java Script V8
  1. Provide an easy way to expose methods from C++ objects to Java Script V8
  1. Avoid the need of MACROS to declare types or keep tracking of classes. The compiler must take care of this with templates.
  1. Achive this without modification of V8 code.
  1. Reflexion_(to be implemented)_, provide a way to call methods from C++ to C++ or JS with the same syntax. See [Ideas](http://code.google.com/p/cproxyv8/wiki/SomeIdeas)

Usage: [Basic Example](http://code.google.com/p/cproxyv8/wiki/Usage)


### Related work ###

  * v8-juice (v8 (J)avaScript (U)serland (I)ntegratable (C)omponents (E)mporium) [v8-juice](http://code.google.com/p/v8-juice)