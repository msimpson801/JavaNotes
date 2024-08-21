## **What is an interface**

Interface in Java is a bit like the Class, but with a significant difference: an interface can only have method signatures, fields and default methods.

Define an interface

Defining an interface is similar to defining a class

``` Java 
interface Animal {

}
```

The key difference is that within an interface definition, we implement nothing

Notice how the methods defined by this interface feature a return type, a name, parameters, and  nothing else. It’s up to classes that implement these interface roles to fill in the method code

```Java
interface Animal {


// all animals must make a sound
// interface method (does not have a body)
public void makeSound();


// all animals must sleep
public void sleep();
}
```

## **Implementing an interface**

Like abstract classes, we cannot create objects of interfaces. To use an interface other classes must implement it.A class which implements an interface provides method bodies for all of the interface's methods. For example:

```Java
class Dog implements Animal {

    public void makeSound() {
// The body of makeSound() is provided here
        System.out.println("I am a silly dog: WOOF WOOF");
    }

    public void sleep() {
// The body of sleep() is provided here
        System.out.println("Sleep Sound: ZzzzzzzZZzzzzZZzzz");
    }
}

```


## **Multiple inheritance**

You can implement multiple Interfaces in a single class. While in Inheritance within Classes you were restricted to inherit only one class, here you can extend any number of interfaces. But do not forget to implement all of the methods of all the Interfaces, otherwise compilation will fail!

Let’s create a new interface Pet.

```Java
interface Pet {
public abstract void beFriendly();
public abstract void play();
}
```



The interface Animal and Pet have several abstract methods. The class Dog implements all the methods of the Animal and Pet interface.

```Java
class Dog implements Animal, Pet {

    public void makeSound() {
        System.out.println("I am a silly dog: WOOF WOOF");
    }
    public void sleep() {
        System.out.println("Sleep Sound: ZzzzzzzZZzzzzZZzzz");
    }

    public void beFriendly() {
        System.out.println("I am waggin my tail");
    }

    public void play() {
        System.out.println("I am chasing a ball");
    }
}
```




Default methods

Before Java 8, we could only declare abstract methods in an interface. However, Java 8 introduced the concept of default methods. Default methods are methods that can have a body

```Java 
interface Animal {

    public void makeSound();

    public void sleep();

    default void wakeUp() {
        System.out.println("Waking up: Yawn!");
    }
}

```


Default methods allow an interface to define an implementation for a method so that when a class implements the interface it does not need to provide its own version of the method

So here the Dog class doesn’t need to implement the default method wakeUp method but it does need to implement the abstract methods makeSound and sleep


If you want to customize a default method in a class, just override it like a regular method:

```Java
class Dog implements Animal {

    public void makeSound() {
        System.out.println("I am a silly dog: WOOF WOOF");
    }

    public void sleep() {
        System.out.println("Sleep Sound: ZzzzzzzZZzzzzZZzzz");
    }


    @Override
    public void wakeUp() {
        System.out.println("I am  a dog waking up");
    }
}
```


