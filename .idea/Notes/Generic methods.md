First let's create a method which accepts an array of Strings and prints them to the screen

```Java
public class Main {
    static void print (String [] array) {
        for (String element: array) {
            System.out.println(element);
        }
    }

    public static void main(String[] args) {

        String [] names = {"Mark", "Dave", "Gerald"};

        print(names);
    }
}
```

If we try to use our print method with an array of numbers we will get an error.

```Java
public class Main {
    static void print (String [] array) {
        for (String element: array) {
            System.out.println(element);
        }
    }

    public static void main(String[] args) {

        String [] names = {"Mark", "Dave", "Gerald"};
        int [] numbers = {1,2,3};

        print(names);
        print(numbers);
    }
}
```

#### Error
```
java: incompatible types: int[] cannot be converted to java.lang.String[]
```

We can change our print method to accept generic types.
Here, the type T indicates that it can refer to any type of class like Integer, String, Double, Dog, etc.

```Java
   static <T> void print (T [] array) {
        for (T element: array) {
            System.out.println(element);
        }
    }
```

There is one last step to get our print method to work with our array of numbers.

``` Java
//Change this 
int [] numbers = {1,2,3};

//To this
Integer [] numbers = {1,2,3};
```

**_Note:_** We need to change our array of int, which is a primitive types to accept an array of Integer objects. We can not use primitive types with generics.

We now have a generic method which works with Integers, Strings, User defined objects

```Java
 public static void main(String[] args) {

        String [] names = {"Mark", "Dave", "Gerald"};
        Integer [] numbers = {1,2,3};
        Dog [] dogs = {new Dog("Fido"), new Dog("Lassie")};

        print(names);
        print(numbers);
        print(dogs);
        
    }
```

