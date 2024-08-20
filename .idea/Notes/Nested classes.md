
As the name suggests a nested class is a class that be found within another class. The purpose of nested classes is to group classes that belong together, which makes your code more readable and maintainable.

There are two types of nested classes

* Static
* Non-static

```Java
// Outer class - class enclosing another class 
class OuterClass {
    ...
    // (non-static) inner class
    class InnerClass {
        ...
    }
    // static nested class
    static class StaticNestedClass {
        ...
    } 
}
```

## Inner class

**Non-static nested classes** are also known as _inner classes_.

One advantage of inner classes, is that they can access attributes and methods of the outer class.
Below we can see that the Lion class has access to the zoo name.

```Java
public class Zoo {
    private String zooName;

    public Zoo(String zooName) {
        this.zooName = zooName;
    }


    public class Lion {
        private String lionName;

        public Lion(String lionName) {
            this.lionName = lionName;
        }

        //Method has access the attribute of outer class zooName
        public void introduceYourself() {
            System.out.println(
            "Hello, my name is " + lionName + 
            " I am being held captive at " + Zoo.this.zooName);
        }

    }
}
```

Now in our main method if we instantiate our inner class and call the introduceYourself method it will output the lion's name and the name of the zoo

```Java
public class Main {
    public static void main(String[] args) {
        Zoo outerClassZoo = new Zoo("Belfast zoo");
        Zoo.Lion innerClassLion = outerClassZoo.new Lion("Leo the lion");

        //Output
        //Hello, my name is Leo the lion I am being held captive at Belfast zoo
        innerClassLion.introduceYourself();
    }
}
```

**_NOTE:_** To instantiate a non-static inner class, you must first instantiate the outer class. Then, create the inner object within the outer object with this syntax:

```Java
Outer outer = new Outer();
Outer.Inner inner = outer.new Inner();

//In our case
Zoo outerClassZoo = new Zoo("Belfast zoo");
Zoo.Lion innerClassLion = outerClassZoo.new Lion("Leo the lion");

```

## Static Inner Class

An inner class can also be `static`, which means that you can access it without creating an object of the outer class. 

Here we have our outer class Sandwich with an inner class filling.

```java
public class Sandwich {

    private String breadType = "Croissant";

    static class Filling {
        private String filling  = "Cheese";

        public void printSandwichInfo() {
            System.out.println("My filling is" + filling);
        }
    }
}
```

We can create our inner class without having to first create our outer class

```Java
public class Main {
    public static void main(String[] args) {
        Sandwich.Filling myFilling  = new Sandwich.Filling();

        myFilling.printSandwichInfo();
        
    }
}
```

**_NOTE:_**  A `static` inner class does not have access to the fields of the outer class

```Java
public class Sandwich {

    private String breadType = "Croissant";

   static class Filling {
        private String filling  = "Cheese";

        public void printSandwichInfo() {
            System.out.println("My filling is" + filling);
            
            //We can not access the bread type field from outer clas
            System.out.println("My bread type" + Sandwich.this.breadType);
        }
    }
}
```