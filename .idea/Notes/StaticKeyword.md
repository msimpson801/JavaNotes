
---

## Static Methods

Static methods are the methods in Java that can be called without creating an object of that class. Static methods can be directly invoked using the syntax `<classname>.<methodname>`.

Let’s imagine we have a static method called `greet` in our `Greeting` class:

```java
class Greetings {
   static void greet() {
       System.out.println("Hello");
   }
}
```

We invoke the method like so:

```java
public static void main(String[] args) {
   Greetings.greet();
}
```

A static method can only access static variables; it cannot access instance variables. For instance:

```java
class Greetings {
   String spanishGreeting = "Hola";
   static String greeting = "Hello";

   // Static method can access static variable greeting
   // But cannot access non-static variable spanishGreeting
   static void greet() {
       System.out.println(greeting);
       System.out.println(spanishGreeting); // This will cause an error
   }
}
```

Trying to access a non-static variable would result in the following error:

```
non-static variable spanishGreeting cannot be referenced from a static context
```

## Static Variables

In object-oriented programming, a class is a blueprint for creating objects. Each object created from the class has its own set of instance variables that hold unique values for each object.

However, there are times when we need an attribute that should be common to all objects of the class, meaning it should have the same value for every instance. This is where static variables come in.

A static variable is declared at the class level, using the `static` keyword. It is not tied to any particular instance of the class but is shared among all instances.

When a static variable is modified or accessed, the change is reflected in all objects of the class since they all share the same static variable.

For example, consider a class called `Colleague` with a static variable called `totalColleagues` to keep track of the total number of colleagues:

```java
class Colleague {
   static int totalColleagues = 0;

   Colleague() {
       totalColleagues++;
   }
}
```

```Java
  public static void main(String[] args) {
       Colleague colleague1 = new Colleague("Stuart");
       Colleague colleague2 = new Colleague("Eoin");
       Colleague colleague3 = new Colleague("Edward");
       
       System.out.println("The total number of colleagues is");
       //Output is 3
       System.out.println(Colleague.colleagues);


```


---
