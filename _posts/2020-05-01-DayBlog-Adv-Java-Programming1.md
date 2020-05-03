---
layout: post
title: "05/01/2020-Adv.-Java-Programming"
markdown: kramdown
date: 2020-05-01
---

Hi, this is a just daily blog to record what I have learnt or reviewed today. Umm, to push myself keep learning everyday.  
Part1: Generics in Java  
Def: Generics are a way to tell a compiler what type of object a collection can contain.  
1.Generic method  
Generic methods in Java are methods that allow you to create a new type parameter just for that method. This is useful 
if you are writing a method but want to be flexible about the type of objects you can pass in.

```
public static List arayToList(Object[] array, List<Object> list){  
    for(Object object: array){  
        list.add(object);
    }  
    return list; 
} 
```
It can compiler without error, but using objects means that we lose type safety.  
The solution is to create a new type of variable T. Because T is a generic type, it doesn't matter
what type I use, as long as it is the same type every time I use it. The type of the array list can also be a supertype
of the type of the array.  
```
public static <T> List<T> arrayToList(T[] array, List<T> list){  
    for(T object: array){  
        list.add(object);
    }  
    return list;
}
```
2.Using varargs(variable-length arguments)  
varargs allows us to write a method that takes a variable number of arguments.  
```
private static void printShoppingList(String... items){
    System.out.println("SHOPPING LIST");  
    for(int i = 0; i < items.length; i++){  
        System.out.println(i+1+":"+items[i]);  
    }  
    System.out.println();
}
```  
3.The substitution principle  
It's an important concept in object-oriented programming, which allows us to write maintainable and reusable code.  
It means that if you have a variable of a given type, you can assign it to a value that is a subtype of that type. But the principle does not apply
with type of lists.  
4.Using wildcard in genetic programming  
A wildcard is essentially an unknown type, and can give you more flexibility when writing methods.  
```  
static void printBuildings(List<?extends Building> buildings){  
    for(int i = 0; i < buildings.size(); i++){  
        System.out.println(buildings.get(i).toString() + " " + (i+1));
    }  
    System.out.println();  
}
```
This means I can now pass in lists of any type that extends the building class.  
Wildcards can also be used to specify that super types can be used when a subtype is specified.  
```
static void addHouseToList(<?super House> buildings){  

}
``` 
Now I can pass in a list of buildings to this method, because Building is a super type of House type.

Part2: Data structures  
1.The Collections framework  
Collections are a essential part of Java programming language. They let you group objects together, in a container, which you can
iterate over, in Java. There are lots of different implementations of collections. We can choose the collection depending
on our exact requirements.  
There are some key factors to bear in mind: Ordering(Is the order important?); Duplicates(allowed?); 
Speed(how fast it will be to perform operations); Memory used;  
In Java, there is a set of interfaces that define different types pf collection. At the very top of the hierarchy is iterable.
All types of collection implement iterable. And it declares the foreach method. Then, there is the collection interface, which extends iterable.
This interface declares all of the methods that every collection must have. There are no classes that are a concrete
implementation of collection directly. Before we get to concrete classes, there is another layer of interfaces, which includes set, list and Queue.  
Sets are a type of collection that do not allow duplicate elements. They are also unordered.  
List, on the other hand, allow duplicate entries. The order of elements in a list is also significant.
Queues are a type of collection that lets you add elements to the heads of the collection.FIFO  
There is also another structure called maps. Maps do not actually extend the collection interface. This is because
they contain key value pairs, which are not suited to being elements in a collection, however, they are still considered
to be a part of the collections framework in Java.  
2.Linkedlist  
A linked list is a doubly linked collection of elements. Each entry in the list also hold a reference to the address of the next and 
the precious item in the list.  
Advantages: they are quick for inserting and removing elements in the middle of a list.  

```

public class LinkedListExample{  

    public static void main(String[] args){  
        LinkedList<String>  myList = new LinkedLisr();
        myList.add("A");
        myList.add("B");
        myList.add(1,"C");
        System.out.println(myList);
        myList.remove("B");
        System.out.println(myList);   
    }
}
```

Disadvantages: Linked lists take up more memory than array lists. Because in a linked list each entry contains a reference to the list
and also to the next and previous elements.  
3.Implement a queue  
Example: using a linkedlist(add(), poll())  
4.HashMap  
Key-value  
```
public class HashMapExample{  
    public static void main(String[] args){  
        HashMap<String, Integer> phonebook = new HashMap<>();  
        phonebook.put("Kevin", 12345);  
        phonebook.put("Jill", 98789);  
        phonebook.put("Brenda", 123123);  
        System.out.println(phonebook); 
        phonebook.put(null,000);
        if(phonebook.containsKey("Brenda")){  
            phonebook.remove("Brenda");
        }
        phonebook.clear(); //clear all
    }
}
```  
The output is not printed out in the same order that we added it. This is because HashMaps is unordered. Entries are 
stored by their contents, not by their positions. Also HashMaps do not allow duplicate keys. HashMaps allow you to have 
null as the value for a key.  
5.LinkedHashMap  
When we use a linked HashMap, the order of the entries is retained. With Linked HashMaps, we can specify if we want 
the elements to retrieved in the order that they were added in, or in the order that they have been accessed in. To choose 
between these options, we need to alter the constructor we used when we created the Linked HashMap, in the first line of the 
main method.  
The first argument is the initial capacity of the map. The second argument is called the Load Factor. This specifies how full
the map can be before it is made bigger.The third argument is a optional argument. This is a Boolean Value, which specifies the ordering mode.
If it is false, it will use the insertion order. If it is true, it will use access order instead.

 