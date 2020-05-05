---
layout: post
title: "05/03/2020-Adv.-Java-Programming"
markdown: kramdown
date: 2020-05-03
---

Modular Programming in Java  
Modularity makes it easier to write well encapsulated code by breaking up large code bases into small sections.  
Introducing modules means that JDK could be broken up into small sections.  
A module is a group of related code. So it contains some code, and it might contain some other resources too. It also has to 
have a name. Module name lives in the global space and they need to be unique. This means that every single Java module in existence should have 
a different name. A module also contains some information about the module itself. By default, everything in a module is hidden to the outside world.  

<em>Creating a module</em>  
```
/*
* Greeting.java
*/
package helloworld;

import java.awt.image.BufferedImage;

public class Greeting{
    BufferedImage image;
    public static void main(String[] args){
        System.out.println("hello world");
    }
}

```   
Select new, then Java Module Info.  
There are two things we can do with the module-info file. The first thing we can do is to specify if we want to 
use any other modules(requires ...). The second thing we can do is exporting packages from the current module.
```
/*
* module-info.java
*/
module HelloWorld{
    requires java.desktop; // to specify if I want to use any other modules.
    export helloworld; // make the helloworld package avalible to the whole world
}

```  

Compiling and running modular applications from the command line is slightly different from the normal java application.  
Compiling: javac -d output/helloworld src/helloworld/com/bethan/greetings/HelloWorld.java src/helloworld/module-info.java  
Running: java --module-path(-p) output --module(-m) helloworld/com.bethan.helloworld.HelloWorld

 
  
