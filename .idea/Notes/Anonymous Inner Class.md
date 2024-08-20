To understand anonymous inner classes let's first create a class called Animal

```Java
class Animal {
    public void makeNoise () {
        System.out.println("Yap, yap, yap");
    }
}
```

As you can see it's a pretty simple class with one method called makeNoise.

In our main method. We can instantiate our class and call the makeNoise method.

```Java
        Animal myAnimal = new Animal();
        myAnimal.makeNoise();
```

Expected output

```
Yap, yap, yap
```

What if we wanted to create a new type of animal object like a yeti which there is only ever going to be one instance of and we want it to make noise in a completely different way than a regular animal object. This is where we can use anonymous inner classes, like so.

```Java
 Animal yeti = new Animal(){
            @Override
            public void makeNoise() {
                System.out.println("Growl I am a yeti");
            }
        };

        yeti.makeNoise();
```

Now whenever we invoke `yeti.makeNoise()` method it will not make the typical "Yap, yap, yap" noise..

We can use anonymous inner classes to implement interfaces. For example, Java Comparator is **an interface for sorting Java objects**.  We can use an anonymous inner class to implement this interface and compare two Fruit objects.

Our fruit object
```Java
@AllArgsConstructor
@NoArgsConstructor
@Data
class Fruits {
    String name;
    int quantity;
}
```

Implementing the Comparator interface to compare to fruit objects by their quantity

```Java
 Comparator <Fruits> fruitsQuantityComparator = new Comparator<Fruits>() {
            @Override
            public int compare(Fruits fruits1, Fruits fruits2) {
                return fruits1.getQuantity() - fruits2.getQuantity();
            }
        };
```

