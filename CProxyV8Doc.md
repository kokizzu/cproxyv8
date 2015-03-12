# Abbreviations #


  * GC - Garbage Collector
  * class - declared class in C++
  * exposed - class/method/property that has been exposed to Java Script
  * Tclass - C++ class type (Point, Line, etc)
  * TPri   - Primitive type (int, bool, double, etc)
  * Type   - Tclass | Tpri

# Methods/Properties in C++ class - Take Note #

> C++
```
      class A
      {
      private:
        Tclass obj;

      public:
        type b;   //ok
        type* b2; //ok-WARNNING: GC will delete this if is exposed
   
         //ok POINTER: GC will delete the returned  obj
         Tclass* getPointer() {return new Tclass(); };

         //ok REFERENCE: GC will NOT delete the returned obj
         Tclass& getReference() {return obj;);
   
         //this is ok.
         Tpri  getPrimitive(); 

         //ok: Generic call, at this moment is the only way to pass parameters 
         //to the object
         v8::Handle<v8::Value> methodName(const v8::Arguments& args)

         //WARNING-FATAL: this will provoke invalid memory ref.
         Tclass getStackReference() {return obj;); 
      };
```

# Handy macros #
Everything is accessed with the following macros.

**Macro parameters**:
  * CLASS: the name of the class in C++
  * PROPERTY: the name of the property in C++
  * METHOD: the name of the method in C++
  * ID: the name for JS
  * GET: true/false true allow the value of the property to used in JS
  * SET: true/false true allow the value change from JS

Macros whit ID parameter allow the programmer to change the name of the class, property or method in Java Script.

## Expose a classes to JS ##
> ### CPROXYV8\_CLASS(class) ###

> C++
```
           ProxyClass<Point>* cPoint = PROXY_CLASS(Point);
```
> Java Script
```
           x = new Point();
```

> ### CPROXYV8\_CLASS\_ID(class,ID) ###

> C++
```
           ProxyClass<Point>* cPoint = PROXY_CLASS(Point,XY);
```
> Java Script
```
           x = new XY();
```

## Expose a property class to JS ##
> ### CPROXYV8\_PROPERTY(class, property, get, set); ###

> C++
```
           CPROXYV8_PROPERTY(Point, x, true, true);
```
> Java Script
```
           p = new Point();
           p.x = p.x + 10
```

> ### CPROXYV8\_PROPERTY\_ID(class, property, id, get, set); ###

> C++
```
           CPROXYV8_PROPERTY(Point, equis, x, true, true);
```
> Java Script
```
           p = new Point();
           p.x = p.x + 10
```

## Expose a method class to JS ##
> ### CPROXYV8\_METHOD(class, method); ###

> C++
```
           CPROXYV8_METHOD(Point, Translate);
```
> Java Script
```
           p = new Point();
           p.Translate(10,10);
```

> ### CPROXYV8\_METHOD\_ID(class, method, id); ###

> C++
```
           CPROXYV8_PROPERTY(Point, Translate, translate);
```
> Java Script
```
           p = new Point();
           p.translate(10,10);
```