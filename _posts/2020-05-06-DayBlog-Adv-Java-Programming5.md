---
layout: post
title: "05/06/2020-Adv.-Java-Programming-Input-and-Output"
markdown: kramdown
date: 2020-05-06
---

Input and Output(I/O)  
Streams are a way of reading data or writing data. There are lots of different use cases for using streams and they are 
used more often than you might think.  
Uses of Streams:  
* Reading and writing files in a program;  
* Taking user input from the console  
* Communicating through sockets  

Streams represent a flow of data, and they can only go in one direction. Output streams write out data and input streams read in data.  
There are two core abstract classes in Java Streams API, InputStream and OutputStream. As these are abstract classes, we cannot instantiate them, but they each have several 
concrete implementations for handling different types of data.  
Implementations of InputStream: FileInputStream, ByteArrayInputStream, FilterInputStream, and more.  
Implementations of OutputStream: FileOutputStream, ByteArrayOutputStream, FilterOutputStream, and more.  
There are two more abstract classes: Reader and Writer. These are similar to InputStream and OutputStream and they are built on the same concepts, but the main difference is that 
InputStream and OuputStream move bytes around whereas Reader and Writer move characters. Characters are easier and more intuitive to work with than bytes. They also handle unicode characters and other 
character encoding issues, which byte streams do not.  
There is a need to read simple inputs from the console with Java. The quick and simple way to do this is using the scanner class.  
```
public class PersonCreator{
    public static void main(String[] args){
        Scanner scanner = new Scanner(System.in);
        System.out.print("Enter the name, age and phone number:");
        String name = scanner.next();
        System.out.print("Enter the age:");
        int age = scanner.nextInt();
        System.out.print("Enter a phone number");
        Long phoneNumber = scanner.nextLong();   
    }
}
```  
The scanner class also has a method called useDelimiter, which lets you specify something other than a space to separate the tokens.  

A common type of input that needs to be read into a program is file inputs. There is a need that we want to be able to print out the file, line by line, in the program.  
One of the ways to do this is with a buffered reader. Buffered readers allow us to read lines of characters, and will work with multiple input encodings.  
```
public class bufferedReaderExample{
    public static void main(String[] args){
        File myFile = new File("example.txt");
        try{
            BufferedReader reader = new BufferedReader(new FileReader(myFile));
            String line;
            while(line = reader.readLine() != null){
                System.out.println(line);
            }
        }catch(IOException e){
        }
    }
}
```
An advantage of buffered reader is that it is synchronized, which means it can safely be used in a multi-threaded application.  
Using try-with-resources with I/O  
When using input and output resources, it's important to use try with resources whenever we can. Using try with resources 
makes sure that all resources are closed for us.  
```
public class TryWithResourceExample{
    public static void main(String[] args){
        try(BufferedReader reader = new BufferedReader(new StringReader("Hello World"));
            StringWriter writer = new StringWriter();){
            writer.write(reader.readLine());
            System.out.println(writer.toString());
        }catch(IOException ioe){
        }
    }
}
```