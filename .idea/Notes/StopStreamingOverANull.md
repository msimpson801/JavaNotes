### Handling null lists when using a stream
Sometimes, we want to stream through a list and perform certain actions.

```java
   public static void main(String[] args) {
    List <Fruit> fruits = List.of(
            Fruit.builder().type("Lemon").colour("Yellow").build(),
            Fruit.builder().type("Lime").colour("Green").build(),
            Fruit.builder().type("Banana").colour("Yellow").build()
    );

    var yellowFruits = fruits.stream()
            .filter(fruit -> "Yellow".equals(fruit.getColour()))
            .toList();

    System.out.println(yellowFruits);
}
```
This code streams through a list of fruits and applies a filter to keep only those that are yellow.

But what happens if the list is potentially null? If we try to stream through it, it will result in a NullPointerException.

To fix this, we can use Optional to safely handle the potential null value:

```Java 
public static void main(String[] args) {
    List<Fruit> fruits = null;

    var yellowFruits = Optional.ofNullable(fruits)
            .stream()
            .flatMap(List::stream)
            .filter(fruit -> "Yellow".equals(fruit.getColour()))
            .collect(Collectors.toList());

    System.out.println(yellowFruits);
}
```


### Explanation:

Optional.ofNullable(fruits): Wraps the fruits list in an Optional. If fruits is null, it creates an empty Optional.

stream().flatMap(List::stream): If the Optional is not empty, it streams the list. flatMap flattens the stream of lists into a single stream.

filter(fruit -> "Yellow".equals(fruit.getColour())): Filters out fruits that are not yellow.

collect(Collectors.toList()): Collects the filtered results into a list.

This approach ensures that if fruits is null, the resulting list yellowFruits will be empty rather than causing a NullPointerException.