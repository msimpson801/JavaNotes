Lambda methods are usually passed as a parameter to a function

```Java
public static void main(String[] args) {

        List <String> names = Arrays.asList(
                "Mark",
                "Brendy",
                "Paddy"
        );

        names.forEach(name -> System.out.println(name));

    }
```

In the above code snippet we use a lambda expression to print every name in the names array.

Lambdas can be stored as variables.
The lambda expression should have the same number of parameters and the same return type as that method

If we look at the Lambda in the above code snippet we can see that it has one argument and returns nothing, therefore we can use a Consumer interface to store our lambda expression.

* ==A consumer interface accepts a single input and returns no output==.*

```Java
public static void main(String[] args) {

        List <String> names = Arrays.asList(
                "Mark",
                "Brendy",
                "Paddy"
        );

        Consumer<String> printLambda = (n) -> System.out.println(n);

        names.forEach(printLambda);

    }
```

Another example of storing a Lambda as a variable.

Because the filter method takes in a Predicate interface we can store our lambda in a variable which is of type predicate, like so.

```Java
public static void main(String[] args) {

        List <String> names = Arrays.asList(
                "Mark",
                "Brendy",
                "Paddy"
        );

        Predicate <String> nameStartsWithB = name -> name.startsWith("B");

        List <String> namesBeginningWithB = names.stream()
                .filter(nameStartsWithB)
                .toList();

    }
```