**_@RequiredArgsConstructor_ generates a constructor requiring an argument for the _final_ and _@NonNull_ field**

Let's imagine we have an employee class with two fields, only one of which is marked as final and we use Lombok's @RequiredArgsConstructor annotation

```Java
@RequiredArgsConstructor
public class Employee {

    private final String name;
    private int salary;
}
```

If we were to delombok this, it would look like this. Only the name has been added to the constructor.

```Java
public class Employee {

    private final String name;
    private int salary;

    public Employee(String name) {
        this.name = name;
    }
}
```