---
layout: post
title: The right way to reactive system programming with Vert.x
image: /img/vertx.jpg 
tags: [Java,Vert.x,Reactive Programming]
---

## Introduction
You probably know that Vert.x as a reactive system based on an event loop. Vert.x is a polyglot web framework that shares common functionalities among its supported languages Java, Kotlin, Scala, Ruby, and Javascript. Regardless of language, Vert.x operates on the Java Virtual Machine (JVM). Being modular and lightweight, it is geared toward microservices development. It allows for high scale, high availability and clustering. You may think, why not use it for my next scale project. But if you are not used to asynchronous programming in reactive systems you may find yourself dealing with unexpected situations which you didn’t anticipate. let’s look at this example the following code :
```
readFile(); 
doSomethingWithFile();
```
Because we do not use any threads, a programmer that review this code would say that readFile() would run until it completes and and then doSomethingWithFile() would run.But we using Vert.x! and in Vert.x that would not necessarily happen!! Why? readFile() probably contains blocking code and doSomethingWithFile() may start before readFile() has done reading the file.
e.g.

```
object.someCodeMethod(handler -> { //A handler for code that will be run after this medhod }
```

Let’s go back to our example of readFile() and doSomethingWithFile(). In our example we have dependency , The method doSomethingWithFile() depends on method readFile() and should only run after readFile() has completed the task. This means we will need to implement this in our code.

Let’s look at the example below:
<script src="https://gist.github.com/OneManCrew/bbe213a90b229263475f6175088cfba7.js"></script>

Executing the example can print the following:
```
Start Process file.
Done Process file.
Succeeded reading the file.
```
The problem here as you can see is that we start processing the file before we finished reading it.

## How to Solve this?
Very simple! we need to provide to the readFile() method a handler to the code block that will process the file after we succeeded reading the file.
<script src="https://gist.github.com/OneManCrew/600f35f4268b9946ce90559b776b4cf1.js"></script>

The console print at this time will be:
```
Succeeded reading the file.
Start Process file.
Done Process file.
```

## Summary
As we can saw working with Vert.x can be very tricky! Because we using a asynchronized way to coding,We need to pay attention how we doing it, The code may not be execute like in synchronized systems.
