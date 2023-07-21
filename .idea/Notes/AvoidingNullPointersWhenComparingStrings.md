### Avoiding null pointer exceptions when comparing Strings

In Java when comparing Strings using the `.equals()` method, flipping the method call to compare the constant string and the variable string is done to ensure that the variable name is null safe.

Let's imagine we have a variable string called personName which could potentially be null. If we call our method with any non-null value it will be fine.

```Java
        String personName = "Mark le shark";

        if (personName.equals("Tall Paul")) {
            System.out.println("Hey Paul");
        }
        else {
            System.out.println("You are not Paul");
        }
```

Expected output

```
You are not Paul
```

If you invoke `.equals()` on `null` you will get `NullPointerException`

````Java
        String personName = null;

        if (personName.equals("Tall Paul")) {
            System.out.println("Hey paul");
        }
        else {
            System.out.println("You are not Paul");
        }
````

Expected result

```
NullPointerException: Cannot invoke "String.equals(Object)" because "personName" is nul
```

By flipping the comparison and ensuring that the constant string is on the left hand side you can avoid this issue.

```Java
        String personName = null;

        if ("Tall Paul".equals(personName)) {
            System.out.println("Hey paul");
        }
        else {
            System.out.println("You are not Paul");
        }
        
```

Expected result

```
You are not Paul
```


Since Java 11 you can use `Objects.equals()` which provides a null safe way to check two object including Strings. You can use it like so.

```Java
        String personName = null;

        if (Objects.equals(personName, "Tall Paul")) {
            System.out.println("Hey paul");
        }
        else {
            System.out.println("You are not Paul");
        }
```





