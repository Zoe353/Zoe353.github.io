---
layout: post
title: "05/07/2020-Adv.-Java-Programming-Input-and-Output"
markdown: kramdown
date: 2020-05-07
---

Working with files and directions  
There are some simple examples listed below.

Creating a new file  
```
public class FileCreationExample{
    public static void main(String[] args){
        File myFile = new File("C:\\Users\\Zhimin\\Desktop\\myFile.txt");
        try{
            boolean fileCreated = myFile.createNewFile();
            System.out.println(fileCreated);
        }catch(IOException ioe){
        }    
    }
}
```  
Working with directories  
```
public static void main(String[] args){
    FilenameFilter filter = (file, filename)->{  //FilenameFilter is a functional interface
        return filename.contains(".");
    };
    
    String[] contents = new File(".").list(filter); // filter is to filter the contents
    for(String file: contents){
        System.out.println(file);
    }

    //create a new directory
    new File("myDirectory").mkdir();
}
```  
Using the Path class  
```
public static void main(String[] args){
    Path path = Paths.get("Hello World.txt");
    try{
        Files.deleteIfExists(path);
    }catch(IOException ioe){
    }
    Path path2 = Paths.get("C:\\Users\\Zhimin\\Desktop\\myFile.txt");
    System.out.println(path2.getParent()); // get the parent directory
    System.out.println(path2.getRoot()); // get the root directory
    System.out.println(path2.getFileName()); // get the file name
}
```  
Copying files with the Path class  
```
public static void main(String[] args){
    Path source = Paths.get("C:\\Users\\Zhimin\\Desktop\\example.txt");
    Path dest = Paths.get("C:\\Users\\Zhimin\\Desktop\\new.txt");
    
    try{
        File.copy(source, dest, REPLACE_EXISTING); // the optional arg: replace the file content if it is existed.
    }catch(IOException ioe){
    }
}
```
