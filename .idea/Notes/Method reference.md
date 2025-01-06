
When working with streams you can occasionally use a method reference in place of a lambda expression in order to simplify/minify the number of lines of code

To understand this better. First let's create a person class.

```Java
@AllArgsConstructor
@NoArgsConstructor
@Data
class Person {
    String name;
    int age;
}
```

Now in our main method let's create a list of people.

```Java
 List <Person> people = Arrays.asList(
               new Person("Mark", 66),
               new Person("Brendy", 88),
               new Person("Paddy", 44)
       );
```

If we want to print each person on our list we can stream through our list and `forEach` person on our list we will print them to the screen.

```Java
people.forEach(person -> System.out.println(person));
```

Output

```
Person(name=Mark, age=66)
Person(name=Brendy, age=88)
Person(name=Paddy, age=44)
```

Alternatively we can replace our lambda function for printing out each person with a method reference, like so

```Java
people.forEach(System.out::println);
```

The syntax is 

```
Class::method
```

`System.out` is our class and `println` is our method

Let's look at another example.We know that our person object consists of two properties name and age, and that each person object has a getter and a setter. Let's say we want to stream through our list of people and create a list of just each person's name, we can do this using a map function.

```Java
       List <String> namesOnly = people.stream()
               .map(person -> person.getName())
               .collect(Collectors.toList());

       namesOnly.forEach(System.out::println);
```

Output

```
Mark
Brendy
Paddy
```

We can see in the above code snippet that our map method is passed a person object and calls the `getName` method on each person. We can refactor the above to use a method reference.

```Java
       List <String> namesOnly = people.stream()
               .map(Person::getName)
               .collect(Collectors.toList());

       namesOnly.forEach(System.out::println);
```

We can also use method reference with static methods. Let's create a static method which takes in a person object and determines if they are over 60.

```Java
    public static boolean isOver60 (Person person){
        return person.getAge() > 60;
    }
```

We can now user this static method to stream through the list of people and filter out people who less than 60 years old.

```Java
       List <Person> peopleOver60 = people.stream()
               .filter(person -> isOver60(person))
               .collect(Collectors.toList());
```

We can replace our Lambda with a method reference

```Java
public class Main {
    public static boolean isOver60 (Person person){
        return person.getAge() > 60;
    }

    public static void main(String[] args) {

       List <Person> people = Arrays.asList(
               new Person("Mark", 66),
               new Person("Brendy", 88),
               new Person("Paddy", 44)
       );

       List <Person> peopleOver60 = people.stream()
               .filter(Main::isOver60)
               .collect(Collectors.toList());
    }
}
```

Finally let's look at Method references being used with object constructors. First let' change our person object so that we can create a person object with the name as a required argument, meaning we can create a person object with a name and no age.

```Java
@AllArgsConstructor
@RequiredArgsConstructor
@Data
class Person {
    private final String name;
    int age;
}
```

Now we can create a person object in one of two ways

```Java
        //Required args only constructor
       var person1 = new Person("Brendy");
       
       //All args constructor
       var person2 = new Person("Mark", 66);
```

Now that we can create an instance of a person object with just a name and not an age. Let's create a list of names. Stream through that list of names and create a new Person object .

```Java
 public static void main(String[] args) {

      var names = Arrays.asList("Brendy", "Paddy", "Mark");
      
      var people = names.stream()
              .map(name -> new Person(name))
              .collect(Collectors.toList());
    }
```

As we can see above we are using a lambda to create a new Person. We can replace this lambda with method reference

```Java
 public static void main(String[] args) {

      var names = Arrays.asList("Brendy", "Paddy", "Mark");

      var people = names.stream()
              .map(Person::new)
              .collect(Collectors.toList());
    }
```