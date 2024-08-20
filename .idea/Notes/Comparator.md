Java comparator is an interface for comparing two java objects. Java Comparator compares two Java objects in a “compare(Object 01, Object 02)” format.

To understand comparators better, let's create a simple fruits class with two fields name and quantity.

```Java
@AllArgsConstructor
@NoArgsConstructor
@Data
class Fruits {
    String name;
    int quantity;
}
```

Now let's create a fruits list which we will sort using comparator interface.

```Java
   List <Fruits> fruitsList = Arrays.asList(
                new Fruits("Clementines", 2),
                new Fruits("Grapes", 4),
                new Fruits("Apples", 3),
                new Fruits("Bananas", 5)
        );
```

Now that we have our list of fruits lets implement our comparator interface using an anonymous inner class

```Java
  Comparator <Fruits> fruitsQuantityComparator = new Comparator<Fruits>() {
            @Override
            public int compare(Fruits fruits1, Fruits fruits2) {
                return fruits1.getQuantity() - fruits2.getQuantity();
            }
        };
```

We can now use our comparator to sort our list.

```Java
    fruitsList.sort(fruitsQuantityComparator);
```

N.B Sorting the list this way will mutate the original array.

If we do not want to change the original array we stream through our array and pass our comparator as an argument to the sort method

``` Java
        List <Fruits> sortedFruits = fruitsList
                .stream()
                .sorted(fruitsQuantityComparator)
                .collect(Collectors.toList());
```

Print out the sorted array and we will see that the returned list has been sorted

``` Java
    sortedFruits.forEach(System.out::println);
```

Output

```
Fruits(name=Clementines, quantity=2)
Fruits(name=Apples, quantity=3)
Fruits(name=Grapes, quantity=4)
Fruits(name=Bananas, quantity=5)
```

Print out the original array and we will see that it is unchanged

```Java
      fruitsList.forEach(System.out::println);
```

Output

```
Fruits(name=Clementines, quantity=2)
Fruits(name=Grapes, quantity=4)
Fruits(name=Apples, quantity=3)
Fruits(name=Bananas, quantity=5)
```

We can create another comparator to sort our array in alphabetical order

```Java
     Comparator <Fruits> fruitNameComparator = new Comparator<Fruits>() {
            @Override
            public int compare(Fruits fruits1, Fruits fruits2) {
               return fruits1.getName().compareTo(fruits2.getName());
            }
        };
```