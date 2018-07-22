# Implement an EOperation

## Introduction


The goals of this exercise are to give us some experience on the following topics:

* Declaring of EOperations in the ecore model
* Generating EOperations
* Overriding code by changing the `@generated` tag

Again, we'll keep the screenshots to a minimum and see if you can navigate the project artifacts on your own.

## 1. Modify the ecore model

To create a new `EOperation` we have to start by changing the `ecore` model.

Add a new method to the ecore model class `Artist` called `printState`.
The method should take no parameter and have a `void` return. That is in Java terms, it should look like this:

```java
  public void printState()
```

## 2. Verify that the method is present in the genmodel.

To ensure that the changes to the `ecore` model is reflected in the `genmodel`, it is safest to follow the following strategy:

1. Close the `genmodel`
2. Update and save the `ecore` model
3. Reopen the `genmodel`

Verify that the `printState` `EOperation` is present in the genmodel.

## 3. Regenerate the code

Let's generate the code again (right-click on the root of the `genmodel` and select `Generate All`).

## 4. Modify the `@generated` tag

Navigate to the implementation of the Artist::printState operation and change the tag to read:

```java
@generated NOT
```

## 3. Implement the `printState`

Now that you've told the EMF framework not to generate this method again, you should be able to change the implementation. Modify the implementation to print out the properties of the artist and perhaps the number of works that you have of this artist.

## 4. Let's cheat a bit...

At this point we would like to be able to run the code.
Later in the course we will learn how to write standalone EMF programs, or use JUnit for testing, but for now we will use a trick to runour operation.
We will tie its execution to the execution of the generated presentation related plugin .edit.

We will need to do the following steps:

1. In the generated plugin `com.scispike.music.edit` open the class `ArtistItemProvider`.
   Locate method getText.
   This method will be called from the UI when it needs the text for the UI elements representing Artists.
2. Change the `@generated` tag by adding `NOT`
3. Add a line of code that will cast the parameter object to Artist and call method `printState` on it.
4. Run the Eclipse application.
5. Either view an existing music library or create a new one.
   Observe how our method prints on the console of the parent Eclipse instance.
   That will happen every time the UI needs a text for an `Artist` instance.

## Summary

We've seen that we can add operations (or methods in Java terms) to the model.
There is no operational semantics supported in `ecore`, so we simply declare the models and then implement them in Java.
