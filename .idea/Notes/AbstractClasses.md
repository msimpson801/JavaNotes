### Abstract classes

Let's create an abstract class named Dog

```Java
abstract class Dog {
    String breed;

    public void bark() {
        System.out.println("Bark");
    }
}
```

In our main method if we try to instantiate our abstract class we will get an error

```Java
public class Main {
    public static void main(String[] args) {

        var myDog = new Dog();
        myDog.bark();

    }
}
```

```
java: org.example.Dog is abstract; cannot be instantiated
```

**_NOTE:_** We can not instantiate an abstract class but we can EXTEND it

```Java
class Labrador extends Dog {  
  
}
```

Now if we instantiate our Labrador class which extends Dog then we can use the bark method in our abstract class

```Java
public class Main {
    public static void main(String[] args) {

        var myDog = new Labrador();
        myDog.bark();

    }
}
```

### Abstract methods

Another feature of abstract classes is abstract methods. Let's add one to our abstract dog class.

```Java
abstract class Dog {
    String breed;

    public void bark() {
        System.out.println("Bark");
    }

    public abstract void bite();
}
```

Notice how their is no body of this method, only a method signature.

Because Labrador extends our abstract class which has the abstract method bite, our class Labrador will need to implement this method.

```Java
class Labrador extends Dog {

    public void bite() {
        System.out.println("Chomp, chomp");
    }
}
```

Abstract methods is just a way of saying that every class that extends an abstract class must have the methods x, y and z. Each specific dog class can choose how to implement a method.

A major difference between an interface and an abstract class is that an abstract class can have a mixture of abstract and non-abstract method, whereas in an interface all methods are abstract
