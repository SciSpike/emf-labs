# Examples of Adapters

## Introduction

In this example, we will demonstrate four ways of using adapters in EMF applications.
This lab is inspired by example in Budinsky et al.: Eclipse Modeling Framework: A Developer's Guide, Addison Wesley 2003.

The examples will have a common theme: counting the number of changes performed on
instances.
First, we will keep track of cumulative number of changes performed on all instances of Artists, and then we will keep track of changes on individual Artist instances.

## Direct Registration of Adapters: com.idata.music.adapters.ex1

In this example, we will attach adapters directly to the notifier instances.
First, we will create an adapter that extends the AdapterImpl class:

```java
public class ChangeCounterAdapter extends AdapterImpl {

  protected int changeCount = 0;


  public int getChangeCount() {
    return changeCount;
  }
  public static ChangeCounterAdapter INSTANCE = new ChangeCounterAdapter();
  public void notifyChanged(Notification notification) {
    // we care about changes to artists
    // we will ignore set methods that do not change the value
    // ("touch")
    if ((notification.getNotifier() instanceof Artist) && !notification.isTouch()) {
      changeCount++;
    }
  }
}
```

You can notice that we are using a singleton adapter instance; it will accumulate the number of changes for all modification of artist.
Method `isTouch()` can prevent unnecessary notifications.
Class `ChangeCounterExample` with the main method demonstrates the use:

```java
public class ChangeCounterExample {
  public static void main(String[] args) {
    Artist a1 = MusicFactory.eINSTANCE.createArtist();
    ChangeCounterAdapter ca = ChangeCounterAdapter.INSTANCE;

    // adapters are added directly to the adapted (observed) instances
    a1.eAdapters().add(ChangeCounterAdapter.INSTANCE);
    a1.setName("Beethoven");
    a1.setNotes("Not bad!");
    a1.setNotes("Very good!");
    a1.setNotes("Great");
    a1.setNotes("Great"); // just a "touch" - we wont' consider this one

    Artist a2 = MusicFactory.eINSTANCE.createArtist();
    a2.eAdapters().add(ChangeCounterAdapter.INSTANCE);
    a2.setName("Bach");
    a2.setNotes("Not bad!");

    // We should see 6 changes
    System.out.println("Number of changes to artists: " + ca.getChangeCount());
  }
}
```

Now, go to Eclipse and run the ChangeCounterExample class.
You can also set a breakpoint in an interesting place and observe the values of objects in executions.

## Factory Registration of Adapters: `com.scispike.music.adapters.ex2`

In this variation of the previous example, we use a factory to perform the registration.
This example is an intermediate step towards a more sophisticated factory based approach.

The registration is achieved through a call to the factory:

```java
ChangeCounterAdapterFactory.INSTANCE.adapt(a1, ChangeCounterAdapter.class);
```

The program AdapterFactoryExample produces the same result as the first example.
A better use of factories comes in the next example.

## Typed Adapter Factory: com.scispike.music.adapters.ex3

We will change the approach here: instead of having a singleton instance for the adapter, we will create individual adapter instances.
We will introduce a class `InstanceChangeCounterAdapterFactory` that extends `MusicAdapterFactory`.
The `MusicAdapterFactory` class is generated class, which extends a class from the EMF framework `AdapterFactoryImpl`.
The `InstanceChangeCounterAdapterFactory` subclass `redefinescreateArtistAdapter` method to return an adapter that increments the counter when notification occurs.

The example program creates a different output than the previous examples: it reports changes to individual artist objects.

You can set a breakpoint in the main of the example program and see that every artist instance has attached its own adapter instance.

## Extending Behavior: com.scispike.music.adapters.ex4

This is the most complex case of adapter use, where we extend the behavior of classes without introducing their subclasses.
In this exercise we were using the Artist interface.
Now, we would like to add some additional behaviors to Artists, but we do not want to (or cannot) change the definition of the interface and class.  
The new behaviors we want to add are method `getCount` and comparison method `isChangedMoreThan`.
We will show how adapters can be used to add the required new behavior.

We will begin by defining an interface.

```java
public interface InstanceChangeCounter {
  int getCount();
  boolean isChangedMoreThan(EObject otherObject);
}
```

We will then create an adapter that implements this interface.
The class will also extend the `AdapterImpl` class.

Here is the beginning of the class:

```java
public class InstanceChangeCounterAdapter extends AdapterImpl implements InstanceChangeCounter {
  protected int count;
  public int getCount() {
    return count;
  }
```

Next, we will define the method that will compare our adapter instance with another `EObject` - if this is an artist, we will get its adapter and access the count information.
Notice that the adapt method is going to return an existing adapter for an artist.
We cast it to `InstanceChangeCounter` so we can compare the counters.
If `otherObject` is not an `Artist`, we just return `false`.

```java
public boolean isChangedMoreThan(EObject otherObject) {
  InstanceChangeCounter otherChangeCounter =
    (InstanceChangeCounter) InstanceChangeCounterAdapterFactory
      .INSTANCE
      .adapt(otherObject, InstanceChangeCounter.class);

      return otherChangeCounter == null
        || otherChangeCounter.getCount() < count;
}
```

The rest of the code is straightforward.

```java
    public void notifyChanged(Notification notification) {
      if (!notification.isTouch()) {
        ++count;
      }
    }
    public boolean isAdapterForType(Object type) {
      return type == InstanceChangeCounter.class;
    }
  }
```

The example application contains the code:

```java
InstanceChangeCounter a1ChangeCounter = (InstanceChangeCounter)
  InstanceChangeCounterAdapterFactory
    .INSTANCE
    .adapt(a1, InstanceChangeCounter.class);
  System.out.println("Number of changes to artist a1: " + a1ChangeCounter.getCount());
```

Using the isChangedMoreThan method to compare the “added” information:


```java
if (a1ChangeCounter.isChangedMoreThan(a2)) {
  System.out.println("a1 is changed more than a2");
}
else {
  System.out.println("a1 is changed more than a2");
}
```

##Conclusion

 Effectively, we have extended the existing Artist class using the EMF Adapter mechanism.
