# Write a program that uses generated code

## Introduction

In this exercise, we will be creating a standalone Java program which will have a `main()` method.
The tasks are to create instances of our EMF generated classes, then use their methods to set the values of attributes and references.
At the end, we will print the values on the standard output.

You will use common EMF coding patterns for creating of instances and access to referenced objects.

Reminder:
    creating instances is always done through factories. For example:

```java
MyType x = MyTypeFactory.eINSTANCE.createMyType();
```

Another thing to get used to: access to the references with multiplicities is done by accessing the collection first.
For example:

```java
x.getRef().add(y);
```

## Instructions

1. Create a class MusicClient that will contain the main method. We recommend
creating it within the com.idata.music package. This will make the setup simpler,
since you do not need to worry about adding various EMF related jar file to the project.
2. Create a main method.
3. In the main method create an instance of MusicLibrary, add a couple of Artists, and their works. Set the attributes and add the instances to the appropriate references.
4. At the end, access the instance of the MusicLibrary, print its attributes, the artists and
their works.
