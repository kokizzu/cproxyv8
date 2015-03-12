### Reflection ###
Current functionality is enough to provide the communication between C++/V8.
Extending the class CProxyV8 and keeping more information about methods and properties can be used to achieve [reflection](http://en.wikipedia.org/wiki/Reflection_(computer_science)) to be use in C++ and JS.

### Call Methods from C++ that may execute C++ or JS implementation ###

CProxyClass will provide an easy way to call functions declared in JS from c++

C++
```
  ProxyClass<Point> p = ProxyClass<Point>->Instance();
  p.call(pointInstance, "moveTo", 10, 10);
```

where "moveTo" can be declared in C++ or JS. If for example "moveTo" is declared in JS and have efficiency problems it can be moved to C++ without impacting the code.

### Auto get/set methods ###

Automatically generate get/set methods for properties. The current process expose properties directly, making validations impossible to do.

### Auto pre/post conditions ###

Another possibility for validations are methods called before/after access a property or method, given a clean way to validate the object state. This pre/post conditions can be configured in JS or C++, so validations may be in the script allowing great flexibility

Example:
C++
```
    class Car
    {
    public:
       int speed;
    ...

    ProxyClass<Point> p = ProxyClass<Point>->Instance();
    p.installPrecondition(pointInstance, "moveTo", "ajustToBounds");

```

where "ajustToBounds" can be declared in C++ or JS.