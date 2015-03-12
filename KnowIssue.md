# Invalid Memory References #


This library allow you to expose objects created in C++ directly to the Java Script, this may lead to the following situation (full code in Usage page):

Java Script shell:
```
   >end = line.getEnd();   
```

When the code exec this, the instance Point who belongs to Line is now exposed to Java Script and can be accessed directly.

```
   >end.x = 10;   
```

Now the PROBLEM: if line is disposed by the garbage collector or by the C++ program, end will be pointed to an invalid reference. When line is disposed the instance of point inside is disposed too.

# Usage solutions #

Do not expose references to this internal objects, unless you are complete sure that they are going to be available all the times.

# Possible Implementation Solutions #

I still thinking how to solve the problem, main idea is to add listeners in both sides to synchronized the disposal process.