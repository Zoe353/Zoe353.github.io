---
layout: post
title: "05/02/2020-Adv.-Java-Programming"
markdown: kramdown
date: 2020-05-02
---

Hi, today is 05/02/2020, Saturday.

Part3: Functional Programming in Java  
1. Functional interfaces in Java  
A functional interface is an interface that has only one abstract method.  
```
public interface GreetingMessage{  
    public abstract void greer(String name);
}
```

Implement this interface in another class,  

```
public class Main(){ 
    public static void main(String[] args){  
        GreetingMessage gm = new GreetingMessage(){  
            @Override  
            public void greet(String name){
                System.out.println("Hello" + name);
            }
        };
        gm.greet("Betham");
    }
}
```
At this moment, the codes to implement the functional interface is quite long and messy considering all it does id 
provide one new line of functionality. Lambdas were introduced to improve this. 
 
2. Implementing lambdas in Java  
Lambdas provide a short and simple way to implement functional interfaces in Java.  
```
public class Main(){ 
    public static void main(String[] args){  
        GreetingMessage gm = new GreetingMessage(){  
            @Override  
            public void greet(String name){
                System.out.println("Hello" + name);
            }
        };
        gm.greet("Betham");
        
        //Using lambdas-quicker and simpler way of implementing
        GreetingMessage gm2 = (String name)->{
            System.out.println("Hello" + name);
        }
        gm2.greet("Bethan");
    }
}
```  

3. Using method references in Java  
Method references are a shorthand way of writing a certain type of lambda expression. 
 
```
public class Square{
    int sideLength;
    public Square(int sideLength){
        this.sideLength = sideLength;
    }
    public int calculateArea(){
        return sideLength*sideLength;
    }
}

@FunctionalInterface
public interface shapes{
    public abstract int getArea(Square person);
}

public class Main{
    public static void main(String[] args){
        Square s = new Square(4);
        
        //Shapes shapes = (Square square)->{
        //    return square.calculateArea();
        //};
        
        Shapes shapes = Square::calculateArea;
        System.out.println("Area" + shapes.getArea(s));
    }
}

```  
4. Streams in Java  
Streams provide a clean and simple way to iterate over a collection in Java instead of using a forEach loop.  
Streams allow functional programming techniques to be used.  
When we use a forEach loop in Java, it uses something called external iteration. What actually happens under the hood is an iterator 
object is created.  
There are a few issues with external iteration:  
Hard to write parallel iterations  
Requires boilerplate code  
Difficult to read meaning  
Hard to abstract away from behavior  
Streams bring the solution to those issues. They use internal iteration instead of external iteration.  

```
books.stream().filter(book -> {

    return book.getAuthor().startsWith("J");})
    //more filters
    //more filters
.forEach(System.out::println);

```  
The filter method used in this example is a lazy method. All it does is adds books with authors beginning with J.
to the new string. The forEach method is a eager method. We could add more lazy method, however, we could not add any more
eager methods.  
5. Parallel streams  
One adv is that we can run iterations in parallel.  
```
books.parallelstream().filter(book -> {

    return book.getAuthor().startsWith("J");})
    //more filters
    //more filters
.forEach(System.out::println);

```  
Although it is very easy to use parallel streams, this doesn't mean that we necessarily should. There is only 
a performance impact when we are using a very large amount of data. Also this code dependent on how may cores are available.


