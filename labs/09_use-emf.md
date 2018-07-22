# Write a program that saves and loads EMF instances

## Introduction

In this lab we will extend the previously written program with the code that will save the instances to a file and demonstrate reading from the file.
We will be using ResourceSet, and Resources.
You will use some typical code fragments for saving and loading serialized EMF instances from standalone programs.

## Saving

First thing that we need to do in a standalone program is to register the XMI resource factory for the .music extension.
We will start by accessing the singleton instance of the Registry:

```java
Resource.Factory.Registry reg = Resource.Factory.Registry.INSTANCE;
```

The registry maintains a map between file extensions and factories that enable saving and loading of resources. One could use the standard XMI serialization, but one could also define custom serialization mechanisms.

```java
Map m = reg.getExtensionToFactoryMap();
m.put("music", new XMIResourceFactoryImpl());
```

You may need to add a jar file that contains the XMIResourceFactoryImpl.
Open the property page for your project and add org.eclipse.emf.ecore.xmi_<SOME_VERSION>.jar to the libraries.
You will find it in the plugins directory of your Eclipse installation.

Resources always belong to a resource set. We will create an instance of the default implementation of the resource set and then ask it to create a resource.
The parameter passed will be a URI with the file name. When the file is created, you will be able to see it in the root of the project.
Since Eclipse does not know about this file, you will need to execute “Refresh” from the context menu in the Resource Navigator.

```java
ResourceSet resSet = new ResourceSetImpl();
Resource res = resSet.createResource(URI.createURI("MusicLibrary1.music"));
```

Adding the content to the resource is easy: we just need to add the root object: the music library instance we created in our program.
All accessible objects will be stored.

```java
res.getContents().add(musicLibraryInstance);
try {
  res.save(null);
} catch (Exception e) {
  System.out.println("Exception on save!" + e);
}
```

The null argument passed to the save method is for the parameter that accepts a Map that may contain various options for saving.
They may be used by custom implementations of a resource.

This should handle the saving in the file.
Try it out.
You may want to inspect the content of the saved file in the text editor.
It will look similar to this:

```xml
<?xml version="1.0" encoding="ASCII"?>
<com.idata.music:MusicLibrary xmi:version="2.0" xmlns:xmi="http://www.omg.org/XMI" xmlns:com.idata.music="http:///com/idata/music.ecore">
  <artists name="Beethoven">
    <works name="Symphony No. 1"/>
  </artists>
</com.idata.music:MusicLibrary>
```

 ## Loading 1

Now we can read IN the file.
*In the continuation of your program*, you will continue by opening the file and then reading from it.

Let’s first create a new `ResourceSet` in which we are going to load the previously saved music library

```java
ResourceSet resSet2 = new ResourceSetImpl();
```

We load the resource by using the `getResource` method.
We will pass the URI with the name of the file.
The second argument, set to true, is for the `loadOnDemand` parameter.
If the URI cannot be resolved in the resource set, the resource will be created and loaded – exactly what we need!

```java
Resource res2 = resSet2.getResource(URI.createURI("MusicLibrary1.music"), true);
```

At this point we can inspect the content.
You may call the method `getContents` to get an `EList` with the contained objects.
Iterate through the content and print it out.

## Loading 2

Now create a separate program that will open the file and read the instances from it.
You can reuse the second half of the previous program.
Execute the program, opening the file `MusicLibrary1.music` that you saved in the run of the previous program.

Surprise: *It does not work!*

The program does not know about the relationship between the packages and the resource that we are loading.
EMF stores these relationships in the registry, which is populated during the initialization of the packages.
Therefore, we need to add one more line to our code:

```java
MusicPackage mp = MusicPackage.eINSTANCE;
```

In the previous program the package initialization was automatically executed as we were using the MusicLibrary.

You could place this line anywhere in the code before loading of the resource, but we suggest placing such initialization code at the beginning of the program.

Your program should now successfully load the objects of our music library.
