## Constants in Java

A constant is a value whose value does not change. To create a constant in Java we use the modifiers `static` and `final`.

* The `final` modifier makes the variable final, which means unchangeable
* The `static` modifier allows the constant variable to be used without first creating an instance of that class.

Let's try and understand the `static` modifier with an example. Let's create a class called `MyAwesomeClass` with a static variable called `DAYS_OF_THE_WEEK`. Because this variable is `static` it can be used elsewhere without explicitly creating a myAwesomeClass object, see below:

```Java
class MyClass {
    static int DAYS_OF_THE_WEEK = 7;
}

public class Main {
    public static void main(String[] args) {

        System.out.println(MyClass.DAYS_OF_THE_WEEK);

    }
}
```

At the moment our variable days of the week can still be changed/mutated. For example:

```Java
class MyClass {
    static int DAYS_OF_THE_WEEK = 7;
}

public class Main {
    public static void main(String[] args) {

        MyClass.DAYS_OF_THE_WEEK = 5000;

        System.out.println(MyClass.DAYS_OF_THE_WEEK);

    }
}
```

Expected output

```
5000
```

Let's now add the modifier final so that the variables value cannot be reassigned.

```Java
class MyClass {
    static final int DAYS_OF_THE_WEEK = 7;
}
```

Now if we run our application and try and reassign the value of days of the week as 5000 we get the following error.

```
cannot assign a value to final variable DAYS_OF_THE_WEEK
```

**_NOTE:_** That the constants name is capitalized. According to Java naming convention, constant names in Java should be capitalized.
