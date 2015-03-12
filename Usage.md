To test this example download the code, you will need the lasted [V8 source code](http://code.google.com/p/v8/wiki/Source?tm=4) and [CProxyV8 Source Code](http://code.google.com/p/cproxyv8/source/checkout)

# Basic Example #

This example show to C++ classes Point and Line and how the properties and methods are exposed to Java Script.

The example is in file shellcpv8.cc part of the download code and is modification of shell.cc that comes with V8.

# C++ Code #
## Class declarations ##
```
   class Point 
   {
   public:
      /** Constructor without params is required */
       Point(int px = 0, int py = 0) : x_(px), y_(py) {}
       Point(Point* p) : x_(p->x_), y_(p->y_) {}

       int x_;
       int y_;

      /** Example call to function without params */
      int GetManhattanLength()  { return std::abs(x_) - std::abs(y_); }

      /** this will create a JS object, with JS destructor */
       Point* Copy() { return new Point(); }

      /** Example call to function with params */
       v8::Handle<v8::Value> Translate(const v8::Arguments& args) 
       { 
         // throw if we didn't get 2 args
         if (args.Length() != 2)
           return ThrowException(v8::String::New("Expected 2 arguments"));

         x_ += args[0]->Int32Value();
         y_ += args[1]->Int32Value();
         return v8::True(); 
       };
   };

   class Line
   {
   public:
     /** Constructor without params is required */
  
     /** Example of object property, it can be manipulate directly */
     Point start;  

     /** this will create a JS object, without destructor and a reference to this 
      *  end instance 
      *
      */
     Point& GetEnd() { return end; }

   private:
     Point end;
   };
```

## Main function ##
> Expose properties and methods with CProxyV8 and register the function template for each    class in the global.

### Using the ProxyClass directly ###
```
   int main(int argc, char* argv[]) 
   {
     v8::V8::SetFlagsFromCommandLine(&argc, argv, true);
     v8::HandleScope handle_scope;

     // Create a template for the global object.
     v8::Handle<v8::ObjectTemplate> global = v8::ObjectTemplate::New();

     ProxyClass<Foo>* pPoint = ProxyClass<Foo>::instance(); //Get the proxy class for Point

     pPoint->Expose(&Point::x_,"x",true,true); //expose x_ for read/write
     pPoint->Expose(&Point::y_,"y",true,true); //expose y_ for read/write

     pPoint->Expose(&Point::Copy, "copy");
     pPoint->Expose(&Point::GetManhattanLength, "getManhattanLength");
     pPoint->Expose(&Point::Translate, "translate");
  
     ProxyClass<Line>* pLine = ProxyClass<Line>::instance(); //Get the proxy class for Line
     pPoint->Expose(&Line::start,"start",true,true); //expose object start for read/write
     pPoint->Expose(&Line::GetEnd, "getEnd");

     global->Set(v8::String::New("Point"), pPoint->GetFunctionTemplate()); // add Point to JS
     global->Set(v8::String::New("Line"), pLine->GetFunctionTemplate()); // add Line to JS
     ...
```

### With helper macros ###

```
   int main(int argc, char* argv[]) 
   {
     v8::V8::SetFlagsFromCommandLine(&argc, argv, true);
     v8::HandleScope handle_scope;

     // Create a template for the global object.
     v8::Handle<v8::ObjectTemplate> global = v8::ObjectTemplate::New();

     CPROXYV8_PROPERTY_ID(Point,x_,x,true,true);
     CPROXYV8_PROPERTY_ID(Point,y_,y,true,true);

     CPROXYV8_METHOD_ID(Point,Copy, copy);
     CPROXYV8_METHOD_ID(Point,GetManhattanLength, getManhattanLength);
     CPROXYV8_METHOD(Point,Translate);
  
     CPROXYV8_PROPERTY(Line,start,true,true);

     CPROXYV8_METHOD_ID(Line,GetEnd,getEnd);

     global->Set(v8::String::New("Point"), CPROXYV8_CLASS(Point)->GetFunctionTemplate()); // add Point to JS
     global->Set(v8::String::New("Line"), CPROXYV8_CLASS(Line)->GetFunctionTemplate()); // add Line to JS
     ...
```

## Shell Execution ##

```
      V8 version 0.4.3.1
      > x = new Point()
      [object Point]
      > line = new Line()
      [object Line]
      > x.x
      0
      > x.x = 10
      10
      > line.getEnd().x
      0
      > line.getEnd().x = 10
      10
      > line.getEnd().x
      10
      > end = line.getEnd();
      [object Point]
      > end.x
      10
      > end.Translate(10,10)
      true
      > end.x
      20
      > line.getEnd().x
      20
      > x.x
      10
      >_
```