If the value is not present, then the function passed as an argument is executed and the value returned from the function is returned.

*NB The function that is passed as an argument is a supplier function*

**A supplier is a function that produces a result without accepting any inputs**

In order to understand this better let's create an optional which will return us null

```Java
    Optional<String> name = Optional.empty();
```

Now let's use the `orElseGet` method to give us a name if the optional is null or empty which in our case it is

```Java
  public static void main(String[] args) {

        Optional<String> name = Optional.empty();

        String result = name.orElseGet(() -> "Default name");

        System.out.println(result);

    }
```

Alternatively we could create a method which accepts no inputs and returns a result AKA a supplier function, and pass that as an argument to the `orElseGet`.

Below is a method to get a random name from a list of three possible names

```Java
  public static String getARandomName () {
        List <String> names = List.of("Mark", "Brendy", "Paddy");

        Random rn = new Random();
        int range = 2 - 0 + 1;
        int randomNum =  rn.nextInt(range) + 0;

        return names.get(randomNum);
    }

```

Now in our main method we pass `getARandomName` to `orElseGet`

```Java
 public static void main(String[] args) {

        Optional<String> name = Optional.empty();

        String result = name.orElseGet(() -> getARandomName());

        System.out.println(result);

    }
```

It is important to note that we can not return anything from our supplier function because are optional is a String then we must return a String from our supplier function. We can't return a random int or user defined object