### Objectives ###

  1. Avoid the need of specifics callbacks declarations and replace they by  generic callbacks directly to the object instance.
  1. Provide an easy way to expose properties from C++ objects to Java Script V8
  1. Provide an easy way to expose methods from C++ objects to Java Script V8
  1. Avoid the need of MACROS to declare types or keep tracking of classes. The compiler must take care of this with templates.
  1. Achive this without modification of V8 code.
  1. Reflexion_(to be implemented)_, provide a way to call methods from C++ to C++ or JS with the same syntax. See [Ideas](http://code.google.com/p/cproxyv8/wiki/SomeIdeas)

Usage: [Basic Example](http://code.google.com/p/cproxyv8/wiki/Usage)

### Advantages ###

#### Types ####
Types are handle automatically by C++ templates, if at some point the type of property or the type returned by methods needs to be changed, it does not affect the expose process to JS, the programmer can change the type where the property or method is declared

#### Access control, statistics, etc ####
Different kind of CProxyClass can be implemented to achieve things like statistics and controlling access to properties and methods.

#### Reflection (to be implemented) ####
CProxyClass behavior is almost like the Class class in Java, therefore  [reflection](http://en.wikipedia.org/wiki/Reflection_(computer_science)) can be achieve by adding more functionality to this class or a child of it